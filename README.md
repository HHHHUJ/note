# note

this is my note

## problem collection

### **1.移动端 ios 不兼容 position:fixed 属性**

```
.head,.foot{position:fixed;left:0;height:38px;line-height:38px;width:100%;background-color:#99CC00;}
.head{top:0;}
.foot{bottom:0;}
.main{position:fixed;top:38px;bottom:38px;width:100%;overflow:scroll;background-color:#BABABA;}
```

只需要给中间滚动部分加上`.main{position:fixed;top:38px;bottom:38px;width:100%;overflow:scroll;background-color:#BABABA;}`即可

### **2.display:flex 布局在 chrom49 及以下版本出现的问题**

In specific, all percentage-based sizes must inherit from parent block elements,and if any of those ancestors fail to specify a size, they are assumed to be sized at 0 x 0 pixels

具体地讲,所有基于本分比的尺寸必须从父块元素继承,并且如果任一父项未能指定尺寸,则它们的尺寸假定为 0 x 0 像素  
比如说 flex 子元素用 flex-shrink:1,flex-flow:1,flex-basis:0,overflow-y:auto;是内容区域撑开,  
但是没有具体给出高度值,那么它的子元素就不能用 height:100%去继承它的高度.

解决办法: 给未撑开元素父元素加 position:relative;
子元素加 position:absolute;

### **3.图片在服务器上找不到，用默认图片代替**

解决办法：

```
<img :src="item.coverImg || 'static/imgs/no_data.png'" alt="" @error="handleImgError(index, item)">
```

```
  handleImgError(index, item) {
    const newData = Object.assign({}, item);
    newData.coverImg = 'static/imgs/no_data.png';
    this.dataList.splice(index, 1, newData);
  },
```

### **4.选中文本时的操作**

```
document.onmouseup = document.ondbclick = selceText;
  function selceText() {
    var txt;
    if (document.selection) {
      txt = document.selection.createRange().text
    } else {
      txt = window.getSelection() + '';
    }
    if (txt) {
      alert(txt);
    }
  }
```


### 5.原型和原型链

#### 原型
(1)所有的引用类型（对象、数组、函数），除了null，都具有对象特性，可自由扩展属性；

(2)所有的引用类型（对象、数组、函数）都有一个__proto__属性，属性值是一个普通的对象；

(3)所有的引用类型（对象、数组、函数）的__proto__属性值，指向它的构造函数的prototype属性值；

(4)所有的函数，都有一个prototype属性，属性值也是一个普通的对象


#### 原型链

每一个引用类型,如fn都有自身属性和__proto__上的属性，首先查找自己本身的属性，找不到再去fn.__proto__上找，也就是它的构造函数Fn.prototype上，Fn.prototype上没有再去Fn.prototype.__proto__找，这样就形成一个链型。

参考[https://cloud.tencent.com/developer/article/1381273](https://cloud.tencent.com/developer/article/1381273 "原型和原型链")

