# REM 方案下的 px, dpx, rpx 使用说明。
* 应用中,对字体大小不使用`rem`, rem的基准值与屏幕宽度成正比, 这就造成相同分辨率的屏幕,越宽字越大,越窄字越小,
在开发过程中,我们创造了 `dpx` (dpr px) 这个单位, 按照dpr来放大 1*px, 2*px, 3*px 大小的字体,再按照屏幕dpr缩小, 这样就达到了字体
不缩放, 各种屏幕的字体看起来都差不多,也与屏幕宽度无关。
* 边框一般不使用`rem` , 在移动设备上最常见的就是`1px`的边框, 由上一条规则我们知道`rem`无法精确到`1px`, 它只是一个与屏幕宽度有关的
模糊值。 已知在两种情况下无法满足我们的需求: 1.需求1物理像素的边框。 2.在屏幕比较窄的1dpr的安卓设备上,由于rem不能整除造成rem换算成px
 不足 `1px`, 边框丢失。 为此我们创造了`rpx` (real px), 来表示物理像素。
* 默认情况下 `px` 被`babel-plugin-px2rem`或`postcss-plugins-px2rem`插件转化成`rem`

(注:https://github.com/songsiqi/px2rem 是按照注释来确定转不转rem和生成三种dpr的样式, 但是在webpack的production环境,会先执行uglify, 会造成注释被移除掉, 所以无法用注释来作为转与不转的标记 )

in
```css
   .cls {
        width: 75px;
        font-size: 12dpx
        border: 1rpx
   }

```

out
```css
    .cls {
        width: 2rem;
        border: 1px;
    }
    [data-dpr="1"] .cls { font-size: 12px }
    [data-dpr="2"] .cls { font-size: 24px }
    [data-dpr="3"] .cls { font-size: 36px }
```
