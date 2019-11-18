# 1px问题  
1.在ios8+中当devicePixelRatio=2的时候使用0.5px
```
div{
     border:1px solid #000;
}    
@media (-webkit-min-device-pixel-ratio: 2) {
    div{
          border:0.5px solid #000;
    }
}
```
2 transform: scale(0.5)
```
//下面是通过伪类实现的1px线条
.onePxHei(@borColor:#efefef,@borPx:1px){
  &:after{
    position: absolute;
    bottom: -1px;
    left: 0;
    width: 100%;
    height: 2px;
    -webkit-transform: scale(1,.5);
    transform: scale(1,.5);
    -webkit-transform-origin: 0 0;
    transform-origin: 0 0;
    content: '';
    background-color: @borColor;
  }
}  
```
```
本人用的是less，这是mixin
缺点：圆角无法实现，实现4条边框比较麻烦
```
伪类
```
border-1px($color)
  position: relative
  &:after
    display: block
    position: absolute
    left: 0
    bottom: 0
    width: 100%
    border-top: 1px solid $color
    content: ' '
```
```
@media (-webkit-min-device-pixel-ratio: 1.5),(min-device-pixel-ratio: 1.5)
  .border-1px
    &::after
      -webkit-transform: scaleY(0.7)
      transform: scaleY(0.7)

@media (-webkit-min-device-pixel-ratio: 2),(min-device-pixel-ratio: 2)
  .border-1px
    &::after
      -webkit-transform: scaleY(0.5)
      transform: scaleY(0.5)
```
```
用是stylus，这是mixin，封装函数形式（简单易懂，推荐）
```
使用background-image
```
//通过background实现的1px，设置到dom本身，不会绘制padding和margin区域
.onePxBottomByBg(@borColor:#efefef,@borPx:1px){
  background-image: -webkit-linear-gradient(top, @borColor, @borColor 50%, transparent 50%);
  background-image: linear-gradient(180deg, @borColor, @borColor 50%, transparent 50%);
  background-size: 120% @borPx;
  background-repeat: no-repeat;
  background-position: bottom left;
  background-origin: content-box;
}
```
```
优点：基本所有场景都能满足，包含圆角的button，单条，多条线     
缺点：大量使用渐变可能导致性能瓶颈
```
# 2、3倍图处理
```
bg-image($url)
  background-image: url($url + "@2x.png")
  @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3)
    background-image: url($url + "@3x.png")
```
```
本人用stylus，基本原理就是根据 @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3)判断，然后用UI给的不同图片。

```
# 比较常用的公共类
清除浮动
```
.clearfix
  display: inline-block
  &:after
    display: block
    content: "."
    height: 0
    line-height: 0
    clear: both
    visibility: hidden
```


