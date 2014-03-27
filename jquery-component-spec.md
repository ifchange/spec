# jQuery组件开发规范

1. 文件名统一用jquery.xxxx.js命名
2. 代码模板采用兼容AMD的模式 
  * https://github.com/umdjs/umd/blob/master/jqueryPlugin.js
  * https://github.com/jquery-boilerplate/jquery-patterns/blob/master/patterns/jquery.basic.plugin-boilerplate.js

3. 文件头部加上完整注释
```
/*
 * Macro Polo
 * @fileoverview 组件简介
 * @author yanwei
 * @version v1.0.2
 * @date 2013-10-23
 */
```
4. 每个方法前加上简单的注释
5. 每个组件都必须有默认参数配置，且每个参数有详细说明
```
$.fn.tips.defaults={
    applyTo: '',   // 目标容器
    content: '',    // 内容
}
```
6. 参数中如果有事件绑定配置，必须是以on开头的驼峰命名，如onXxx
7. 确保组件返回的是jquery对象
8. 插件前缀统一用ic前缀，如$.fn.icDialog, $.fn.icTooltip
9. 非UI类组件通过$.xxx扩展上，UI类组件通过$.fn.xxx扩展
10. 版本号作为静态属性保存，如$.xxx.version, $.fn.xxx.version
11. 如果组件依赖有CSS，提供参数配置后自动加载对应的CSS
12. 对于涉及表单数据的组件，既能够根据操作生成隐藏域的值，也可以在初始化的时候根据隐藏域的值渲染组件
13. 一些建议
  * 多利用data()方法/html5的data属性来实现一些数据缓存等
  * jquery (dom)元素一律以$开始，如$input
  * 私有属性和方法一律下划线开始
  * 一些公共属性
    * this.$element：调用元素
    * this.options：组件配置


**参考资料**
* http://learn.jquery.com/plugins/
* http://learn.jquery.com/jquery-ui/widget-factory/
* https://github.com/guillaumepotier/Parsley.js/blob/master/parsley.js
