# util

**util.inherits**

使用 inherits 从一个基类继承原型方法,接受两个参数：子类和基类；

   _util.inherits(subClass, superClass);_
    
    function Super(name) {
        // super class code
    }
    
    Super.prototype = {
        constructor: Super,
        
        getName: function() {
            return this.name;
        },
        
        setName: function(newName) {
            this.name = newName;
            return this;
        }
    }
    
    function Sub() {
        // sub class code
    }
    
    util.inherits(Sub, Super);
    
    var subInstance = new Sub();
    
    typeof subInstance.getName === 'function'    // true
    typeof subInstance.setName === 'function'    // true

-------------------------------------------------------------------------------

**util.mixin** 

使用 mixin 可以把一个 Object 上的属性复制到另外一个 Object 上，接受2个或以上的参数

当只有 2 个参数时，把源 Object 上的所有属性都复制到目标 Object 上；

当参数大于 2 个时，从第三个参数开始是指定的要从源 Object 上复制的 key；

_util.mixin(target, source [,key1, key2, ..., keyn]);_

    var target = {};
    var source = {
        a: 10,
        b: 100,
        c: 1000
    }
    
    mixin(target, source, 'b', 'c');
    console.log(target);    // {b:100, c:1000}
    
    mixin(target, source);
    console.log(target)    // {a: 10, b: 100, c: 1000}
     
    
-------------------------------------------------------------------------------


   
**util.extend**

对 jQuery.extend 的引用；
    
    
    
-------------------------------------------------------------------------------

**util.argumentsToArray** 

把 *arguments* 对象转换为数组

只在函数内部使用，接受一个函数运行时的  *arguments* 对象作为参数，两个可选参数：起始位置和结束位置 用来对转换后的数组进行裁切

_util.argumentsToArray(arguments [,start, end]);_
    
    function fn() {
        var args1 = util.argumentsToArray(arguments);
        var args2 = util.argumentsToArray(arguments, 1);
        
        console.log(args1);
        console.log(args1);
    }
    
    fn(1, 2, 3, 4);
    
    Output:
    [1, 2, 3, 4]
    [2, 3, 4]
    
    
    
    
    
    
---------------------------------------------------------------------------------

**util.isFunction**

判断是否是函数 

---------------------------------------------------------------------------------    
    
**util.isObject**

判断是否是 Object

---------------------------------------------------------------------------------

**util.isArray**

判断是否是数组


----------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------------
--------------------------------------------------------
# Events

事件基础类

**Events.prototype.addListener / Events.prototype.on**:添加一个事件监听
    
    var ev = new Events();
    
    function handler() {
        console.log('eventName was fired,', arguments); 
    }
    
    ev.on('eventName', handler);
    
**Events.prototype.emit**:触发一个事件
    
    ev.emit('evnetName', agrument1, ..., argumentsn);    //  eventName was fired,[argument1, ..., argument] 
    
**Events.prototype.removeListener**:移除一个事件监听
    
    ev.removeListener('eventName', handler);
    ev.emit('eventName')        // nothing happen
    
**Events.prototype.removeAllListeners**:移除所有监听器,事件名可选，如果为空，则移除所有事件，非空则只移除该事件下的监听
    
    ev.removeAllListeners([type]);
    
**Events.prototype.once**：监听一次
    
    function once() {
        console.log('I will called once');
    }
    ev.once('once', once);
    
    ev.emit('once');    // I will be called once;
    ev.emit('once');    // nothing to output


# Widget

wiget 是一个工厂方法，根据传入的配置用来返回每个组件的构造函数；接受两个参数：组件名、组件配置

由widget 返回的构造函数，其 prototype 上已经以下几个通用接口：
    
   * render():组件渲染
   * get(attribureName):返回组件某个属性值
   * set(key, value):设置组件某个属性
   * destroy():销毁组件
   
构造函数本身还有个 eachHTML 方法，用来遍历页面上指定类名的元素，自动生成组件

下面是创建一个Tab组件时具体的调用实例：
    
    ;;define(function(require, exports, module){
    var widget = require('./widget');
    
    // 设置组件的名称
    var NAME = 'tab';

    var $ = require('jquery');
    
    // 生成构造函数
    var Tab = module.exports = widget(NAME, {
        // 配置属性
        // 组件默认属性包含：
        // id: 组件id，默认 null
        // parentNode：父亲节点，默认 body
        // template： 组件模板，默认 ''
        // module：组件数据，默认 null
        
        attrs: {
            triggers: {
                value: '.tg-item',
                readOnly: true
            },

            panels: {
                value: '.ctn-item',
                readOnly: true
            },

            addbtn: {
                value: '.add-btn',
                readOnly: true
            },

            closebtn: {
                value: '.close',
                readOnly: true
            },

            activeIndex: {
                value: 0             
            }
        },

        // 配置组件鼠标键盘等事件
        // 事件配置采用 action@selector的形式
        // selector 可以有两种方式：jq选择器和用 {} 包含的属性名
        // 第二种时，会取该属性的值作为选择器
        // 如下面的 click@{triggers}, 真实的选择器就是 triggers 属性的值
        // 后面的值可以是 函数、字符串、由字符串或函数组成的数组，数组主要是用来绑定多个事件的
        // 当为字符串或数组中有字符串时，该字符串应该是该组件的一个方法名
        // 就像下面的例子，_switchPanels 等都是组件的一个方法
        events: {
            'click@{triggers}': '_switchPanels',
            'click@{addbtn}': ['_add'],
            'click@{closebtn}': ['_close'],
            'dblclick@{triggers}': ['_close'],
            // 也可以直接是个函数
            'click@{panels}': function() {
                // do something
            }
        },

        _onInit: function() {
            // 组件初始化完成时被调用，可选
            this.set('activeIndex', this.get('activeIndex'));
        },
        
        _onChange: function() {
            // 一旦有组件属性发生改变都会被调用
            // 一次改多个属性只会被调用一次
        },
        
        _onRender: function(){
            // 组件渲染完成时被调用
        },

        // 组件内部逻辑，这些方法都将被挂载在构造函数的 prototype 上
        // 组件内部可以为每个属性配置一个_onChange + 属性名(首字母需大写)的事件
        // 当该属性被修改时，将会调用该事件，如下面的 _onChangeActiveIndex 
        // ChangeActive事件
        _onChangeActiveIndex: function(key, newValue, oldValue) {
            var triggers = $(this.get('triggers'), this.element);
            var panels = $(this.get('panels'), this.element);

            triggers.removeClass('active');
            panels.removeClass('active');

            triggers.eq(newValue).addClass('active');
            panels.eq(newValue).addClass('active');
        },
        
        // 删除一个面板
        _close: function(el, e) {
            var triggers = $(this.get('triggers'), this.element);
            var panels = $(this.get('panels'), this.element);
            var closebtn = $(this.get('closebtn'), this.element);
            var index = closebtn.index(el);
            var size = this.size();

            e.stopPropagation();

            if (size === 1) {
                this.destroy();
            } else {
                triggers.eq(index).remove();
                panels.eq(index).remove();

                var activeIndex = this.get('activeIndex');
                if (index < activeIndex) {
                    this.set('activeIndex', activeIndex - 1);
                } else if (index === activeIndex) {
                    var newIndex = index ? index - 1 : index + 1;
                    this.set('activeIndex', newIndex);
                }
            }
        },

        // 添加一个面板
        _add: function() {
            var counter = this.size() + 1;
            this.add('Tab' + counter, 'Content' + counter);
        },

        // 切换面板
        _switchPanels: function(el, e) {
            var index = $(this.get('triggers'), this.element).index(el);
            this.set('activeIndex', index);
        },

        // 对外接口 获取tab长度
        size: function() {
            return $(this.get('triggers'), this.element).length;
        },

        // 对外接口 添加一个标签和面板
        add: function(title, content) {
            var triggers = $(this.get('triggers'), this.element);
            var panels = $(this.get('panels'), this.element);
            var newLi = triggers.eq(0).clone();
            var newDiv = panels.eq(0).clone();

            newLi.html(newLi.html().replace(/Tab\d+/g, title)).appendTo(triggers.parent());
            newDiv.html(content).appendTo(panels.parent());

            this.set('activeIndex', this.size() - 1);
        }
    });
 
手动创建一个Tab实例的代码：
    
    // 所有配置，除了至少需要 element 和 template 其中一个，其他都是可选
    var tab = Tab({
        // 设置要绑定的 DOM 节点
        element: '#J-tab',
        
        // 设置组件的HTML模板
        template: '',
        
        // 可选，设置 activeIndex 属性的初始值
        // 注意：如果某个属性组件内部没有预先定义，则这里不能设置初始值
        // 所有属性都应该先在组件开发过程中预先定义好，不是所有实例都具有的属性，默认值可以给空值
        activeIndex: 0,
        
        // 当节点是从模版生成时，该配置决定在初始化完成后是否理解把组件html片段插入到页面中
        // 但该配置不是组件本身的一个属性，仅仅作为配置存在
        // 默认值 true
        autoRender: false,
        
        onInit: function(){
            // 可选
            // 该实例初始化完成时被调用
            // 如果明确返回 false，则 组件内部定义的 _onInit 将不被调用
        },
        onChange: function(){
            // 可选
            // 该实例某个属性被修改时被调用
            // 如果明确返回 false，则 组件内部定义的 _onChange 将不被调用
        },
        onRender: function(){
            // 可选
            // 该实例渲染完成时被调用
            // 如果明确返回 false， 则组件内部的 _onRender 将不被调用
        },
        onChangeActiveIndex: function(){
            // 可选
            // 针对该实例的某个具体的属性的change事件，只对该实例有效
            // 如果明确返回 false，则组件内部定义的下划线开头的对应事件将不被调用
        }
    });


Tab.eachHTML()调用：
    
    // 如果不需要做其他配置，直接调用即可
    // 也可以传入一个 Object 参数，该参数和手动创建实例时传入的参数是类似的
    // 但比手动创建时多了一个 classNamePrefix 项
    // 该项用来生成查找组件时需要用到的选择器，默认值为 widget，对应页面上的 class值为  widget-tab
    // 即 前缀-组件名
    Tab.eachHTML();

    // 自定义 classNamePrefix
    Tab.eachHTML({
        // 查找页面上 widgetPrefix-tab 的元素
        classNamePrefix: 'widgetPrefix'
    });
    
    // eachHTML 还可以根据需要把每个组件实例的配置写在html中，
    // 只需要把配置的 json 字符串写在 classNamePrefix-tab 所在的元素的 data-tab-config 属性上即可
    // 且在使用 eachHTML 时，此类型的配置的优先级是最高的
    
    
    <div class="widget-tab" data-tab-config='{"activeIndex":2}'>
        <ul class="triggers">
            <li class="tg-item active">Tab1</li>
            <li class="tg-item">Tab2</li>
        </ul>
        <div class="contents">
            <div class="ctn-item active">content1</div>
            <div class="ctn-item">content2</div>
        </div>
    </div>
    
