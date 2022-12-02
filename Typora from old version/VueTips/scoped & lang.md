# scoped

在给vue组件设置样式时，如果不添加scoped属性，在编译时是会将所有css属性汇总到一起的。如果重叠了，那么后引入的会覆盖先引入的。如果给style添加scoped属性，那么就可以实现模块化。

但是，app组件（根组件）的样式最好不要使用scoped，因为app是所有组件的总管，所以他要管理所有子组件，所以他的样式不需要模块化。

# lang

lang属性是用于分别css和less。如果是css语言，那么`lang="css"`；如果是less，那么`lang="less"`