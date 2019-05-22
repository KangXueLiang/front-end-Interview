## CSS

- 常见面试题之左边固定，右边自适应布局  
  方案有很多中，这里总结了一下    
  html结构如下
  ```html
	<div class="parent">
		<div class="left"></div>
		<div class="right"></div>
	</div>
  ```
  - display:inline-block方案    
  ```css
  .parent {
    font-size: 0; // 这里是一个需要注意的地方，否则两个子元素之间会有明显的间距，因为两个子元素之间有个空格。
  }
  .left, .right{
    display: inline-block;
  }
  .left {
    width: 100px;
  }
  .right {
    width: calc(100% - 100px);
  }
  ```
  - 浮动+margin方案    
  把左边固定宽度的元素浮动，右边元素设置左margin即可
  ```css
  .left {
    width: 100px;
    float: left;
  }
  .right {
    margin-left: 100px;
  }
  ```
  - 浮动+BFC方案    
  把左边固定宽度的元素浮动，但是右侧盒子通过overflow: auto;形成了BFC，因此右侧盒子不会与浮动的元素重叠。
  ```css
  .parent {
    overflow: auto; // 这里需要注意下父元素也需要清除浮动
  }
  .left {
    width: 100px;
    float: left;
  }
  .right {
    overfolw: auto;
  }
  ```
  - 双浮动方案     
  左右元素都左浮动，左侧元素固定宽度，右侧元素通过calc计算，注意：父元素需要清除浮动
  ```css
  .parent {
    overflow: auto; // 这里需要注意下父元素也需要清除浮动
  }
  .left, .right {
    float: left
  }
  .left {
    width: 100px;
  }
  .right {
    width: calc(100% - 100px);
  }
  ```
  - absolute+margin方案     
  左侧元素绝对定位，脱离文档流，右侧元素设置margin-left即可
  ```css
  .parent {
    position: relative; // 给父元素一个定位
  }
  .left {
    width: 100px;
    position: absolute;
  }
  .right {
    margin-left: 100px;
  }
  ```
  - flex布局方案     
  ```css
  .parent {
    display: flex; // 给父元素一个定位
  }
  .left {
    width: 100px;
  }
  .right {
    flex: 1; // 右侧元素占据剩余的所有空间
  }
  ```
  - grid布局方案     
  ```css
  .parent {
    display: grid;
    grid-template-columns: 100px 1fr;
  }
  .left {
    grid-column: 1;
  }
  .right {
    grid-column: 1;
  }
  ```
  
  
