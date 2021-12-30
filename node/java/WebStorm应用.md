### webstorm设置技巧

#### 1.取消勾选安全保存时间

​		这两项取消勾选，否则影响热更新，不能及时将修改的内容反应在浏览器上。

![img](\img\webp) 

#### 2.常用开发工具窗口

开发过程中，最常用的工具窗口有以下几个：

- Project 记录项目的层级结构；（快捷键 Alt + 1）
- Structure  记录当前文件内部的层级结构，方便快速定位到某个方法；（快捷键 Alt + 7）
- Npm 使用 npm 构建的工程，Npm 窗口会记录 package.json 里的脚本信息，一般用于快速启动项目；快捷键 （Ctrl + E）
- TODO 项目中难免会预留 TODO 标记用于日后完善，该窗口可以快速定位到哪个文件的哪一行预留了 TODO 标记。（快捷键 Alt + 6）

 ![img](\img\webp1) 

####  3.快捷键 —— 最常用的快捷键最佳应在 10 个以内

`Ctrl + Shift + R` —— 快速定位到文件并跳转

`Ctrl + Shift + F` —— 全局搜索文件内某个字符串 （Webstorm 默认快捷键，eclipse 中是 `Ctrl + H`）

`Ctrl + Shift + U` —— 大小写切换

`Ctrl + E` —— 打开最近操作过的文件

`Ctrl + Alt + L` —— 格式化代码（与 QQ 快捷键冲突，自定修改 QQ 快捷键）

`Ctrl + Y` —— 删除光标所在行

`Ctrl + Alt + S`—— 打开设置窗口

#### 4.关闭 node_modules 校验

 在 node 项目中存在 node_modules 目录，每次打开 webstrom 时会校验文件，同样也会校验 node_modules 中的内容，这会浪费很多时间。 

 ![img](\img\webp2)

#### 5.取消勾选不常用的插件

​		 webstorm 中可以集成很多插件，这些插件也会影响运行速度，有的插件你可能压根都没听过，更不会使用，可以取消勾选。 

- #### ![img](\img\webp3) 