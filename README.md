# note
this is my note

## problem collection
### **1.移动端ios不兼容position:fixed属性**
```
.head,.foot{position:fixed;left:0;height:38px;line-height:38px;width:100%;background-color:#99CC00;}
.head{top:0;}
.foot{bottom:0;}
.main{position:fixed;top:38px;bottom:38px;width:100%;overflow:scroll;background-color:#BABABA;}
```
只需要给中间滚动部分加上`.main{position:fixed;top:38px;bottom:38px;width:100%;overflow:scroll;background-color:#BABABA;}`即可

### **2.display:flex布局在chrom49及以下版本出现的问题**

In specific, all percentage-based sizes must inherit from parent block elements,and if any of those ancestors fail to specify a size, they are assumed to be sized at 0 x 0 pixels

具体地讲,所有基于本分比的尺寸必须从父块元素继承,并且如果任一父项未能指定尺寸,则它们的尺寸假定为0 x 0像素    
比如说flex子元素用flex-shrink:1,flex-flow:1,flex-basis:0,overflow-y:auto;是内容区域撑开,  
但是没有具体给出高度值,那么它的子元素就不能用height:100%去继承它的高度.

解决办法: 给未撑开元素父元素加position:relative;
          子元素加position:absolute;

>呵呵
