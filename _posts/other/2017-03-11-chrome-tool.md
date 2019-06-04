---
title: Chrome 工具
tags: 备忘录
---
### 利用 Chrome 浏览器的开发者工具截取整个页面
打开 Chrome 浏览器，进入需要截图的网站页面
等待页面加载完毕后，通过下面方法打开开发者工具：在页面任何地方点击鼠标右键，在弹出菜单中选择“检查（Inspect）”选项。<br/>
或者使用快捷键组合：Alt + Command+ I (Mac) 或 Ctrl + Shift + I (Windows)
使用快捷键组合来打开命令行（command palette）：Command + Shift + P(Mac) 或 Ctrl + Shift + P (Windows)
在命令行中输入“Screen”，这时自动补齐功能会显示出一些包含 “Screen” 关键字的命令。<br/>
移动方向键到“Capture full size screenshot”并回车（或直接用鼠标点击这个选项）

### 扩展应用场景
如果并不想截取整个页面，而是截取页面中的一些元素，也可以利用开发者工具实现。<br/>
下面以截取 LinkedIn 网站中的用户身份信息为例：
进入需要截图的网站页面，打开开发者工具（方法和上面两步相同）<br/>
点击开发者工具左上角的“选取元素”按钮，在网页中点击要截图的元素<br/>
由于 HTML 父子元素的嵌套，可能选中的是需要截图元素的子元素。<br/>
这时，需要在开发者工具中对所选取的元素进行调整：由于选取的是子元素，所以只需要在“选取元素”按钮，<br/>
旁边的”Elements Tab”里边按照嵌套关系，找到合适的父元素就可以了。这时，点击选中该父元素。
打开命令行，进行截图命令（方法和上面第四步类似）。
<br/>不过需要注意这时在包含 “Screen” 关键字的命令中选取“Capture node screenshot”而非“Capture full size screenshot”
