# css存在的问题

  回想一下，在开发过程中，令人头疼的CSS是不是存在以下几个问题呢。
  - 全局污染，样式冲突。明白人都知道，说来都是泪，样式垮掉，多半是这问题。
  - 命名。复杂组件或页面里，命名冲突，冗杂。
  - 无变量。2017年以后才在CSS里引入变量，早些时候是没有的。
  
#社区解决方案
  - less、scss、stylus css预编译语言，提供了譬如变量、嵌套、混合、运算、占位符等方法，对css样式复用、样式抽离起了极大的促进作用。
  - BEM 流行的class命名规则，为了解决命名冲突提供了良好的思路。但命令过于冗杂。
  - CSS IN JS 顾名思义，CSS不在于JS抽离。最为流行的 styled-component开源库。
  - CSS Modules 极大的避免样式冲突，全局污染等问题，存在作用域，利于样式复用。
  Vue里scoped、modules比较


# scoped 

  可直接在vue等里使用，会生成带唯一属性来区分。一般情况下是唯一的，不会重复。
  值得注意的是，父组件的样式不会应用到子组件中。**除非：子组件的顶层container类名和父级
  里某个类名相同。**

  ```js
    // child.vue
    <template>
      <div class="red"> // 顶层container 类名存在于父级
        <h1>This is a child page</h1>
      </div>
    </template>
    <style scoped>
    .red {
      color: green;
    }

    // father.vue
    <template>
      <div>
        <h1 class="red">This is a test page</h1>
        <child></child>
      </div>
    </template>
    <style scoped>
    .red {
      color: red;
    }
    </style>

  ```

  以上例子说明，也是有可能造成样式冲突的。而解决的方法无疑是增加样式权重 !important。
  另外如果只是单纯的使用选择器，而不是class或id会造成性能的丢失。
  
  在使用第三方库，想修改第三方UI样式。有两种选择，
  1、不在scoped里做样式；2、深度选择器
  
  ### 深度选择器

  - .a >>> .b
  - /deep/ (sass,less)

  ### 动态内容
  
    v-html生成的内容不受scoped样式影响,可选择深度选择器

# module

  在vue里需要额外配置

  ```js
    {
        test: /\.css$/,
        use: [
          'vue-style-loader',
          {
            loader: 'css-loader',
            options: {
              // 开启 CSS Modules
              modules: true,
              // 自定义生成的类名
              localIdentName: '[local]_[hash:base64:8]'
            }
          }
        ]
      }
  ```

  scoped类名权重问题将不会存在,且在JS里可以通过this.$style.red访问

  ```js
    <template>
      <div :class="$style.red">
        <h1>This is a child page</h1>
      </div>
    </template>

    <style module>
    .red {
      color: green;
    }
    </style>

  ```


  总体而言，module要好于scoped。
