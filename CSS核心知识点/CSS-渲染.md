


![示意图](https://github.com/death-hpw/my-images/blob/master/20191106142711.png)

1. CSS的加载不会影响DOM树的解析
2. CSS的加载会影响DOM树的渲染
3. CSS的加载会影响后面JS的执行

CSS放在Head中会阻塞页面的渲染,页面的渲染会等到CSS加载完成
CSS也会阻塞JS的执行，因为JS可能会影响CSS
CSS不会阻塞外部JS的加载，但会阻塞执行
CSS会不会阻塞DOM数的解析，但影响DOM树的渲染
JS

直接引入的JS会阻塞页面的加载
JS顺序执行