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

## 导航完成之前获取数据：导航完成前，在路由进入的守卫中获取数据，在数据获取成功后执行导航。

> 平时都是导航完成前获取数据，然后数据拿到之前用骨架屏或者loading来显示;
> 导航完成前获取数据，就用页面顶部进度条形式作为页面提示，具体代码如下：

1. 路由进入前获取数据
```
beforeRouteEnter (to, from, next) {        
    api.test.test({params}).then(({code,data})=> {            
        if(code === 0) {
          next(vm => {                
            vm.list = res.data;                           
          })  
        }      
    })    
}
```

2. 进度条加载
```
首先引入nprogress包
npm install --save nprogress
在main.js中增加如下代码
import NProgress from 'nprogress';
import 'nprogress/nprogress.css';
NProgress.configure({     
    easing: 'ease',  // 动画方式    
    speed: 500,  // 递增进度条的速度    
    showSpinner: false, // 是否显示加载ico    
    trickleSpeed: 200, // 自动递增间隔    
    minimum: 0.3 // 初始化时的最小百分比
})
router.beforeEach((to, from , next) => {
    // 每次切换路由时，调用进度条
    NProgress.start();
    next(); //必加next，才会跳转
});
router.afterEach(() => {  
    // 在即将进入新的页面组件前，关闭掉进度条
    NProgress.done()
})
```
## 监听自定义事件

> $on(event,callback)\
> $once({string}event,{function}callback)
```
handleTimer() {
  var timerr = setInterval(() => {
    console.log(123321);
  }, 2000)
  this.$once('hook:beforeDestroy', function () {
    clearInterval(timerr)
  })
}
```

## 滚动加载
> this.scrollHeight 有滚动条时，元素视口高度+被隐藏元素高度\
> this.scrollTop 滚动条的高度\
> this.clientHeight 视口高度

```
openModel() {
  this.formData = this.row;
  this.$nextTick(() => {
    let hasData = true;
    const that = this;
    document.querySelectorAll('.body').forEach((itemNode) => {
      itemNode.addEventListener('scroll', function () {
        if (this.scrollHeight - this.scrollTop - this.clientHeight <= 300) {
          if (hasData) {
            hasData = false;
            that.$api.rule.getMatchContent({ ruleId: that.ruleId, limit: 10, start: that.matchData.length }).then(
              (res) => {
                if (res.data.list.length) {
                  that.matchData.push(...res.data.list);
                  hasData = true;
                }
              });
          }
        }
      });
    });
  });
  this.$api.rule.getMatchContent({ ruleId: this.ruleId, limit: 15, start: 0 }).then(
    (res) => {
      if (res.data) {
        this.matchData = res.data.list;
        if (this.matchData.length > 0) {
          this.isShow = false;
        } else {
          this.isShow = true;
        }
      }
    });
},
```

