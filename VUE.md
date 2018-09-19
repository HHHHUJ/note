# VUE

## vue懒加载和代码切割  ([链接]("https://alexjoverm.github.io/2017/07/16/Lazy-load-in-Vue-using-Webpack-s-code-splitting/" "vue实现懒加载"))
### 分为以下三种情况
> components，also known as async components
```
Vue.component('AsyncCmp', () => import('./AsyncCmp'))
或则
components: {
  UiAlert: () => import('keen-ui').then(({ UiAlert }) => UiAlert)
}
```
> router
```
const Login = () => import('./login')
new VueRouter({
  routes: [
    { path: '/login', component: Login }
  ]
})
或则
export default [{
  path: 'agent',
  name: 'agent',
  component: resolve => require(['@/views/agent/agent.vue'], resolve),
}];
```
> vuex modules
```
const store = new Vuex.Store()

// Assume there is a "login" module we wanna load
import('./store/login').then(loginModule => {
  store.registerModule('login', loginModule)
})
```
