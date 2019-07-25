---
title: Vue项目搭建与部署
date: 2019-07-25 18:19:57
tags:
- vue
- js
categories:
- 前端
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

## 插件操作

### 配置<code>vue-router</code>

安装依赖<code>vue-router</code>，在项目根目录下新建<code>router.js</code>文件，这里有两个<code>vue</code>页面

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
      path: '*',
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

### 配置<code>echarts</code>

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
