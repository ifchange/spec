# 自己开发组件规范


## 目录结构
    
    /static/public/js            //公共资源 js 根目录
      |--ic-widgetname           //组件根目录，命名规则：ic-组件名
      |   |--HISTORY.md          //更新记录，必须
      |   |--README.md           //组件说明，必须
      |   |--docs                //详细文档，必须
      |   |--examples            //使用实例，必须
      |   |--tests               //测试用例，目前可选
      |   |--src                 //源码目录，必须
      |   |   |--widgetname.js
      |   |   |--widgetname.css
      |   |   |--template        //模版目录，可选
      |   |   |   |--widgetname.html
      |   |   |--img             //图片资源，可选
      |   |   |   |--icon.png




所有组件均使用模块化的开发方式，使用 **seajs** 做为模块加载器，组件的代码结构如下：
    
    /**
     * 组件介绍，可以同 README.md
     */
     
    ;;define(function(require, exports, module){
        // 加载 widget 构造方法
        var widget = require('widget');
        
        // 组件名，英文单词或中文全拼，小写
        var NAME = 'tab'
        
        // Tab 是个构造函数，首字母大写
        var Tab ＝ module.exports = widget(NAME, {
            // 组件代码
            // 优先定义组件属性
            // 定义组件方法
        });
    });
    
 * **所有自己开发的组件，都应该保持如上结构，且严格遵守约定的编码规范，不同的应该只是组件的内部代码；**
 
 * **组件开发完成，在 docs 目录下必须有完整的文档，包含 属性列表，方法列表，使用说明等；**
 
 * **每次组件更新，必须在 HISTORY.md 中描述更新细节；**
 
 * **最好有组件使用的 DEMO；**
 
 * **条件允许时，请对组件进行测试，包含性能、兼容性等。**
