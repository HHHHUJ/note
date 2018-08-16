# REACT

## 1.react router
>exact的作用

'/home'匹配时会同时匹配'/',在'/'路径上添加exact,会严格匹配,并且有两个参数exact={true}(严格模式),exact={false}(正常模式)

>报错
```
browser.js:38 Uncaught Error: A <Router> may have only one child element
```
在router下只能有一个div