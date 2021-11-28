# 根据路由 vue-router 自动生成面包屑

面包屑每一项的名称是哪里来的呢，是你的路由`meta`的`title`

下面是路由配置的简单例子

```js
const routes = [
  {
    path: '/',
    name: 'Home',
    meta: {
      title: '首页'
    },
    component: Home
  },
  {
    path: '/about',
    name: 'About',
    meta: {
      title: '关于我'
    },
    component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')
  }
]
```

再来个嵌套的路由例子

```js
const routes = [{
    path: '/',
    component: {
      render(h) {
        return h('router-view')
      }
    },
    meta: {
      title: '首页'
    },
    children: [
      {
        path: '/',
        name: 'home',
        component: Home
      },
      {
        path: '/level2',
        name: 'level2',
        meta: {
          title: '第二级'
        },
        component: {
          render(h) {
            return h('router-view')
          }
        },
        children: [
          {
            path: '/level2',
            name: 'level2',
            component: () => import( /* webpackChunkName: "about" */ '../views/level2.vue'),
          },
          {
            path: '/level2/level3',
            name: 'level3',
            meta: {
              title: '第三级'
            },
            component: () => import( /* webpackChunkName: "about" */ '../views/level3.vue'),
          }
        ]
      }
    ]
  },
  {
    path: '/about',
    name: 'About',
    meta: {
      title: '关于我'
    },
    component: () => import( /* webpackChunkName: "about" */ '../views/About.vue')
  }
]
```

下面这块呢，是当你嵌套路由的时候，父级路由想嵌套子路由时的写法，
```js
component: {
    render(h) {
        return h('router-view')
    }
},
```

## 路由传参

如果你的面包屑路由需要传参的时候，也是支持的，仅支持query传参，就是在url上传参，你跳转路由传参时，组件也会自动帮你记录下来。

就是下面的传参方式

```
this.$router.push({
    name: 'detail',
    query: {
        id: 1,
    }
})
```

