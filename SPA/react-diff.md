

## 核心思想


- 两个相同的组件产生类似的DOM结构，不同的组件产生不同的DOM结构。

- 同一层级的一组节点，他们可以通过唯一的id进行区分。


## 基于三个策略

 - Web UI 中 DOM 节点跨层级的移动操作特别少，可以忽略不计，对树进行分层比较，
 两棵树只会对同一层级进行比较。（tree diff）(优先深度遍历)
 - 拥有相同类的两个组件将会生成相似的树形结构，拥有不同类的两个组件将会生成不同的树形结构（component diff）（old tree: p, new tree: div。则为不同类型的组件）
 - 对于同一层级的一组子节点，它们可以通过唯一id进行区分。（element diff）


 ### tree diff, 比较树的结构

  对virtualDOM进行遍历，找出不同。
  **只会对同层级进行比较**。
  在这一环节中，只会进行删除和创建操作。
  发现old tree有，new tree没有，则将其以及以下DOM删除。
  发现old tree没有，new tree有，则创建同层DOM。

 ### component diff， 比较组件的类型

  **如果是同一类型的组件，按照原策略继续比较 Virtual DOM 树即可
  如果不是，则将该组件判断为 dirty component，从而替换整个组件下的所有子节点**
  对于同一类型下的组件，有可能其 Virtual DOM 没有任何变化，如果能确切知道这一点，那么就可以节省大量的 diff 算法时间。因此， React 允许用户通过 shouldComponentUpdate()来判断该组件是否需要大量 diff 算法分析。

  一旦判断是不同组件类型，则直接删除创建。


 ### element diff，节点结构的比较

  - INSERT_MARKUP（插入）：如果新的组件类型不在旧集合里，即全新的节点，需要对新节点执行插入操作。
  - MOVE_EXISTING （移动）：旧集合中有新组件类型，且 element 是可更新的类型，generatorComponentChildren 已调用 receiveComponent，这种情况下 prevChild=nextChild，就需要做移动操作，可以复用以前的 DOM 节点。
  - REMOVE_NODE （删除）：旧组件类型，在新集合里也有，但对应的 elememt 不同则不能直接复用和更新，需要执行删除操作，或者旧组件不在新集合里的，也需要执行删除操作。

  如 A->B->C->D 转化为B->A->D->C
  又如A->B->C->D 转化为 A->B->E-C->D
  会反复的执行创建删除操作。
  创建B删除A创建D删除B....
  **发现新老集合里都含有相同的节点，只是节点的位置发生了变化，缺冗杂的发生以上操作，十分影响性能，实际上只需要移动DOM而已。针对这一问题，提出的解决方案则是添加唯一的KEY进行区分。而这也是为什么为什么在同一层级DOM添加key的原因。**

  如 A->B->C->D 转化为B->A->D->C

  初始化，lastIndex = 0， nextIndex = 0。
  lastIndex是一个标记值，只用作比较大小，来决定是否进行节点移动。初始值为0。
  nextIndex，新集合里下一个要取的节点的坐标。用于确定新集合当前已取到第几个节点，从而继续遍历，初始值为0。
  _mountIndex:旧集合中当前节点的坐标。

  1.从新集合取出节点B，发现旧集合中也有节点B，并且B.__mountIndex = 1，lastIndex = 0。
  2.**if(child._mountIndex < lastIndex) ? ——No，节点不移动。并且更新 lastIndex= Math.max(prevChild._mountIndex, lastIndex)，此时lastIndex值为1。**
  3.**并将B的位置更新为新集合中的位置，prevChild._mountIndex = nextIndex，即B._mountIndex = 0,**
  4. nextIndex ++ 进入下一步

  1.从新集合取出节点A，发现旧集合中也有节点A，并且A.__mountIndex = 0，lastIndex = 1。
  2.**if(child._mountIndex < lastIndex) ? ——Yes, 节点移动。更新 lastIndex= Math.max(prevChild._mountIndex, lastIndex)，此时lastIndex为1。**
  3.并将A的位置更新为新集合中的位置prevChild._mountIndex = nextIndex，即A._mountIndex = 1,4.nextIndex ++ 进入下一步



https://www.jianshu.com/p/650246766f67
  