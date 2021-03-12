---
title: Vue项目搭建与部署
date: 2019-07-25 18:19:57
tags:
- Vue
categories:
- 前端
- JavaScript
---

  使用 <code> vue cli 3 </code> ，要求 <code>node.js 8.9</code>或更高版本，<code>UI</code> 插件选用的是<code>vux</code>

  <!---------------more------------------->
## 安装 <code>vue</code>

### 配置 <code>cnpm</code>

//todo

### 全局安装 <code>vue</code>

在命令行中输入
```
npm install -g @vue/cli
# 安装成功后检验
vue --version
```

## 创建新项目

创建新的项目
```
vue create hand-vux
```
输入上述命令后会被提示选取一个<code>preset</code>。可以选用默认包含了基本插件的 <code>Babel + ESLint</code> 的<code>preset</code>，也可以选“手动选择特性”来自定义。由于<code>vue cli 3</code>提供有一个图形化的界面，来对项目进行管理，方便又美观，不用白不用。

### 图形化界面
在命令行中输入<code>vue ui</code>启动可视化界面，默认是在<code>8000</code>

{% asset_img vue_1.png 截图_1 %}

打开界面后，下方状态栏显示的是我们最近一次打开的项目

{% asset_img vue_2.png 截图_2 %}

点击下方的 <code>更多</code>选取<code>Vue项目管理器</code>，可以创建/导入项目。项目安装依赖也比较简单，直接可在界面上操作。

## 部署

### 去除<code>URL</code>中的<code>#</code>井号
使用<code>vue-router</code>之后，<code>URL</code>链接中会带有<code>#</code>符号，这是由于<code>vue-router</code>默认采用的是<code>hash</code>模式，这样可以模拟一个完整的链接，当<code>URL</code>改变时，页面不会重新加载。但在实际部署时，往往需要去掉这个<code>#</code>符号，于是需要在<code>vue</code>项目还有后端<code>web</code>服务器进行一些配置。

#### 配置<code>vue-router</code>

在原<code>router.js</code>的配置中添加<code>base、mode</code>的配置。
```js
//route.js
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router)

export default new Router({
  base: "/hand/", // 项目部署时的名字
  mode: 'history', // 去除 URL 中的井号#，需要和后台一起配置才起作用
  routes: [
    {
      path: '/info/',
      component: () => import('./views/info'),
      meta: {
        title: 'Demo Info'
      }
    },
    {
      path: '*', // 返回自定义的错误页面
      redirect: '/404'
    },
    {
      path: '/404',
      name: '404',
      component: () => import('./views/error/404.vue')
    },

  ]
})
```

#### 项目打包

在图形项目管理器中点击<code>配置</code>在<code>Vue CLi</code>中设置<code>基础设置</code>。配置<code>公共路径</code>、<code>输入目录</code>与<code>router.js</code>中的<code>base</code>项一致。
{% asset_img vue_3.png 项目打包 %}
设置完成后，在<code>任务</code>选项卡里进行<code>build</code>打包。

#### <code>Web</code>容器部署

<code>vue-router</code>[官网](https://router.vuejs.org/zh/guide/essentials/history-mode.html#%E5%90%8E%E7%AB%AF%E9%85%8D%E7%BD%AE%E4%BE%8B%E5%AD%90)有<code>Apache</code>、<code>Nginx</code>的配置说明，这里以<code>Tomcat</code>为例。
首先将打包生成<code>dist</code>文件全部拷贝到<code>Tomcat</code>的<code>webapps</code>目录下，并将名字更改为在<code>router.js</code>配置中<code>base</code>对应的名字，这里为<code>hand</code>。再在该文件夹下新建<code>WEB-INF/web.xml</code>文件，在里面配置
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
  version="4.0"
  metadata-complete="true">

  <display-name>webapp</display-name>
  <description>webapp</description>
  <error-page>
    <error-code>404</error-code>
    <location>/index.html</location>
  </error-page>

</web-app>
```
将所有的<code>404</code>请求，转向项目对应的<code>index.html</code>页面，如果不转的话，是访问不了页面的。但这样的配置之后，我们需要它返回<code>404</code>页面的地方，也是不会返回的错误页面的，所以刚在<code>router.js</code>中需要把所有的未定义路由导向到自定义的错误页面。

在<code>Tomcat</code>的<code>server.xml</code>的<code>HOST</code>标签中添加如下配置：

```xml
<Context path="/hand" docBase="hand" reloadable="false" crossContext="true" />
```

启动<code>Tomcat</code>可以看到页面正常访问，且<code>URL</code>中没有<code>#</code>符号。


## 常用插件

### 配置<code>echarts</code>

首先安装<code>echarts</code>依赖，然后配置<code>main.js</code>

```js
import echarts from "echarts";

Vue.prototype.$echarts = echarts;

```

在<code>components</code> 目录下新建一个<code>Line.vue</code>文件。

```js
// Line.vue
<template>
    <div>
        <div ref="line" :style="{width: '100%', height: '300px'}" ></div> 
    </div>
</template>

<script>
export default {
    name: 'MLine',
    props: {
        options: Object
    },
    mounted() {
        var myChart = this.$echarts.init(this.$refs.line);
        myChart.setOption(this.options);
    }
}
</script>
```

在需要显示图表的页面中导入上述模块

```js
<template>
    <m-line :options="optionsLine"></m-line>
</template>

import MLine from "@/components/MLine.vue";

export default {
    components: {
        MLine
    },
    data() {
        return {
            optionsLine: {};
        }
    },

    methods: {

    },
    mounted: function() {
        // 更新 optionsLine
    }


}
```

### 配置<code>vux</code>

安装依赖: <code>vux</code>、<code>vuex-i18n</code>

配置<code>main.js</code>:

```js
import vuexI18n from 'vuex-i18n';

Vue.use(vuexI18n.plugin, store, {
  moduleName: 'i18n',
  onTranslationNotFound (locale, key) {
    console.warn(`i18n :: Key '${key}' not found for locale '${locale}'`)
  } }
)
```

配置<code>vue.config.js</code>

```js
module.exports = {
    configureWebpack: config => {
       require('vux-loader').merge(config, {
           options: {},
           plugins: ['vux-ui']
       })
    }
}
```

在页面中按需引入模块即可操作


## 附录

### <code>vue</code>对数据对象的属性更改后，视图没更新

在函数中对数据对象直接采用赋值的形式更新数据后，视图中的数据没有相应的改变。


```js
data () {
  params: {}
},
method: {
  queryInfo: funtion() {
    this.params.name = 'Tony'; // 不能触发数据的改变
    this.$set(this.params, 'name', 'Tony'); // 直接调用 set 来触发数据改变
  }
}
```