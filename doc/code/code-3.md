## 常用代码片断(代码库) - 媒体查询(css media)

### 概述

通过媒体查询为不同的设备和大小配置不同的样式。

### 代码展示

```css
/* media */
/* 横屏 */
@media screen and (orientation:landscape){
     
}
/* 竖屏 */
@media screen and (orientation:portrait){
     
}
/* 窗口宽度<960,设计宽度=768 */
@media screen and (max-width:959px){
     
}
/* 窗口宽度<768,设计宽度=640 */
@media screen and (max-width:767px){
     
}
/* 窗口宽度<640,设计宽度=480 */
@media screen and (max-width:639px){
     
}
/* 窗口宽度<480,设计宽度=320 */
@media screen and (max-width:479px){
     
}
/* windows UI 贴靠 */
@media screen and (-ms-view-state:snapped){
     
}
/* 打印 */
@media print{
     
}


```