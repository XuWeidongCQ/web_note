# #、分部分说明

## 1.虚拟DOM

* 虚拟dom是指将一个真实的dom用js对象来描述，如

  ```jsx
  //真实dom
  <div class="a" id="b">我是内容</div>
  
  //使用js对象来描述
  {
    tag:'div',        // 元素标签
    attrs:{           // 属性
      class:'a',
      id:'b'
    },
    text:'我是内容',  // 文本内容
    children:[]       // 子元素
  }
  ```

* 使用虚拟dom的好处：
  * 1.真实的dom的结构非常复杂，包含的属性远比虚拟dom使用的属性多得多，故频繁的操作dom很浪费性能
  * 2.使用虚拟dom可以只让数据发生变化的时候才更改真实的dom(因为直接手动操作js去更新DOM，可能会出现有些不需要更新的节点却意想不到的更新了)
  
  ```
  最直观的思路就是我们不要盲目的去更新视图，而是通过对比数据变化前后的状态，计算出视图中哪些地方需要更新，只更新需要更新的地方，而不需要更新的地方则不需关心，这样我们就可以尽可能少的操作DOM了。这也就是上面所说的用JS的计算性能来换取操作DOM的性能。
  ```
  
  

### 1.1 虚拟节点（只有23个属性，远小于真实DOM节点上的属性）

类型包括:

* 注释节点 isComment为true
* 文本节点 就只是一段文字
* 元素节点
* 组件节点 比如自己写的.vue组件
* 函数式组件节点
* 克隆节点 isCloned为true

```ts
//位置：src/core/vdom/vnode.js
//虚拟节点类
export default class VNode {
  constructor (
    tag?: string, //标签名
    data?: VNodeData, 
    children?: ?Array<VNode>, //包含的子节点
    text?: string, 
    elm?: Node,
    context?: Component,
    componentOptions?: VNodeComponentOptions, //组件节点的options
    asyncFactory?: Function
  ) {
    this.tag = tag //当前节点的名称，比如div
    this.data = data //当前节点上附带的数据，比如id class 
    this.children = children //当前节点的子节点,是一个数组
    this.text = text /*当前节点的文本*/
    this.elm = elm /*当前虚拟节点对应的真实dom节点*/
    this.ns = undefined
    this.context = context
    this.fnContext = undefined
    this.fnOptions = undefined
    this.fnScopeId = undefined
    this.key = data && data.key //用于唯一标识一个vnode节点，用于优化
    this.componentOptions = componentOptions
    this.componentInstance = undefined /*当前节点对应的组件的实例*/
    this.parent = undefined /*当前节点的父节点*/
    this.raw = false 
    this.isStatic = false /*静态节点标志*/
    this.isRootInsert = true
    this.isComment = false /*是否为注释节点*/
    this.isCloned = false
    this.isOnce = false
    this.asyncFactory = asyncFactory
    this.asyncMeta = undefined
    this.isAsyncPlaceholder = false
  }

  // DEPRECATED: alias for componentInstance for backwards compat.
  /* istanbul ignore next */
  get child (): Component | void {
    return this.componentInstance
  }
}
//产生一个注释节点
export const createEmptyVNode = (text: string = '') => {
  const node = new VNode()
  node.text = text
  node.isComment = true
  return node
}
//产生一个文本节点
export function createTextVNode (val: string | number) {
  return new VNode(undefined, undefined, undefined, String(val))
}
```

### 1.2 DOM-Diff(对比新旧两份`VNode`并找出差异的过程就是所谓的`DOM-Diff`过程)

* 虚拟dom的目的是为了在数据变化前后生成真实DOM对应的虚拟DOM
* 对比两份新旧VNode的差异，并修改真实DOM相应位置的算法就是DOM-Diff，这一个过程也叫patch
* oldVNode是数据变化之前的虚拟dom，组件初始化时候的dom产生的就是oldVNode
* 以新的VNode为基准，改造旧的oldVNode使之成为跟新的VNode一样，这就是patch过程要干的事

#### 1.2.1 创建节点

* 新的`VNode`中有而旧的`oldVNode`中没有，就在旧的`oldVNode`中创建

* 只有3种类型的节点能够被创建并插入到`DOM`中，它们分别是：元素节点、文本节点、注释节点。

  <img src="../../../AppData/Roaming/Typora/typora-user-images/image-20200627194613598.png" alt="image-20200627194613598" style="zoom:67%;" />

```js
  function createElm (
    vnode,
    insertedVnodeQueue,
    parentElm,
    refElm,
    nested,
    ownerArray,
    index
  ) {
    const data = vnode.data
    const children = vnode.children
    const tag = vnode.tag
    if (isDef(tag)) { //有tag说明是元素节点
      if (process.env.NODE_ENV !== 'production') {
        if (data && data.pre) {
          creatingElmInVPre++
        }
        if (isUnknownElement(vnode, creatingElmInVPre)) {
          warn(
            'Unknown custom element: <' + tag + '> - did you ' +
            'register the component correctly? For recursive components, ' +
            'make sure to provide the "name" option.',
            vnode.context
          )
        }
      }
      vnode.elm = nodeOps.createElement(tag, vnode)  //创建元素节点
      createChildren(vnode, children, insertedVnodeQueue) // 创建元素节点的子节点
      insert(parentElm, vnode.elm, refElm)       // 插入到真实DOM中
    } else if (isTrue(vnode.isComment)) {
      vnode.elm = nodeOps.createComment(vnode.text) //创建一个注释节点
      insert(parentElm, vnode.elm, refElm) //将这个节点的插入到真实的dom中
    } else {
      vnode.elm = nodeOps.createTextNode(vnode.text) //创建一个文本
      insert(parentElm, vnode.elm, refElm) //将这个节点的插入到真实的dom中
    }
  }
```

#### 1.2.2 删除节点

* 新的`VNode`中没有而旧的`oldVNode`中有，就从旧的`oldVNode`中删除。

  ```js
  function removeNode (el) {
      const parent = nodeOps.parentNode(el)// 获取父节点
      // element may have already been removed due to v-html / v-text
      if (isDef(parent)) {
        nodeOps.removeChild(parent, el)  // 调用父节点的removeChild方法
      }
  }
  ```

#### 1.2.3 ++更新节点（7步）

* 新的`VNode`和旧的`oldVNode`中都有，就以新的`VNode`为准，更新旧的`oldVNode`

* 静态节点 节点中没有{{ }}的节点

  ![image-20200627194704530](../../../AppData/Roaming/Typora/typora-user-images/image-20200627194704530.png)

```js
function patchVnode (
    oldVnode,
    vnode,
    insertedVnodeQueue,
    ownerArray,
    index,
    removeOnly
  ) {
    if (oldVnode === vnode) {//如果两个节点一样 直接退出
      return
    }

    const elm = vnode.elm = oldVnode.elm

    // vnode与oldVnode是否都是静态节点？若是，退出程序
    if (isTrue(vnode.isStatic) &&
      isTrue(oldVnode.isStatic) &&
      vnode.key === oldVnode.key &&
      (isTrue(vnode.isCloned) || isTrue(vnode.isOnce))
    ) {
      vnode.componentInstance = oldVnode.componentInstance
      return
    }


    if (isUndef(vnode.text)) {//vnode有text属性？若没有
      if (isDef(oldCh) && isDef(ch)) {// vnode的子节点与oldVnode的子节点是否都存在？
          // 若都存在，判断子节点是否相同，不同则更新子节点
        if (oldCh !== ch) updateChildren(elm, oldCh, ch, insertedVnodeQueue, removeOnly)
      } else if (isDef(ch)) {// 若只有vnode的子节点存在
        if (process.env.NODE_ENV !== 'production') {
          checkDuplicateKeys(ch)
        }
       /**
       * 判断oldVnode是否有文本？
       * 若没有，则把vnode的子节点添加到真实DOM中
       * 若有，则清空Dom中的文本，再把vnode的子节点添加到真实DOM中
       */
        if (isDef(oldVnode.text)) nodeOps.setTextContent(elm, '')
        addVnodes(elm, null, ch, 0, ch.length - 1, insertedVnodeQueue)
      } else if (isDef(oldCh)) {// 若只有oldVNode的子节点存在
        removeVnodes(oldCh, 0, oldCh.length - 1)// 清空真实DOM中的子节点
      } else if (isDef(oldVnode.text)) {// 若vnode和oldnode都没有子节点，但是oldnode中有文本
        nodeOps.setTextContent(elm, '')// 清空oldnode文本
      }
    } else if (oldVnode.text !== vnode.text) {//vnode有text属性？若有,且不相同
      nodeOps.setTextContent(elm, vnode.text)//则用vnode的text替换真实DOM的文本
    }
  }
```

## 2.模板编译

* 把用户写的html模板(这个里面含有一些非原生的HTML)进行编译就可以得到初始的VNode
* Vue会把用户在`<template></template>`标签中写的类似于原生`HTML`的内容进行编译，把原生`HTML`的内容找出来，再把非原生`HTML`找出来，经过`render`函数将模板内容生成对应的`VNode`，而`VNode`再经过前几篇文章介绍的`patch`过程从而得到将要渲染的视图中的`VNode`，最后根据`VNode`创建真实的`DOM`节点并插入到视图中， 最终完成视图的渲染更新。
* 非原生html包括 {{ }}  过滤器 指令 自定义组件标签

![image-20200627200912911](../../../AppData/Roaming/Typora/typora-user-images/image-20200627200912911.png)

```js
export const createCompiler = createCompilerCreator(function baseCompile (
  template: string,
  options: CompilerOptions
): CompiledResult {
   //1.模板解析阶段：用正则等方式解析 template 模板中的指令、class、style等数据，形成AST 
  const ast = parse(template.trim(), options)
  if (options.optimize !== false) {
  // 2.优化阶段：遍历AST，找出其中的静态节点，并打上标记；
    optimize(ast, options)
  }
  //3.代码生成阶段：将AST转换成渲染函数；
  const code = generate(ast, options)
  return {
    ast,
    render: code.render,
    staticRenderFns: code.staticRenderFns
  }
})
```

### 2.1 模板解析阶段（生成AST）

* `parse` 函数就是解析器的主函数，在`parse` 函数内调用了`parseHTML` 函数对模板字符串进行解析，
* 在`parseHTML` 函数解析模板字符串的过程中，如果遇到文本信息，就会调用文本解析器`parseText`函数进行文本解析；
* 如果遇到文本中包含过滤器，就会调用过滤器解析器`parseFilters`函数进行解析。

```js
/**
 * Convert HTML string to AST.
 */
export function parse(template, options) {
   // ...
  parseHTML(template, {
    warn,
    expectHTML: options.expectHTML,
    isUnaryTag: options.isUnaryTag,
    canBeLeftOpenTag: options.canBeLeftOpenTag,
    shouldDecodeNewlines: options.shouldDecodeNewlines,
    shouldDecodeNewlinesForHref: options.shouldDecodeNewlinesForHref,
    shouldKeepComment: options.comments,
    start (tag, attrs, unary) {//如果遇到开始标签就执行这个函数 
        //其中为调用processAttrs()用来解析该标签的属性，包括原生的，指令等
    },
    end () {//如果遇到结束标签就执行这个函数

    },
    chars (text: string) {//如果遇到文本就执行这个函数，其中可能包含插值表达式和过滤器
        //其中会调用函数parseText() parseFilters()
    },
    comment (text: string) {//如果遇到注释文本就执行这个函数

    }
  })
  return root
}
```



```js
 function parseHTML (html, options) {
    var stack = [];
    var expectHTML = options.expectHTML;
    var isUnaryTag$$1 = options.isUnaryTag || no;
    var canBeLeftOpenTag$$1 = options.canBeLeftOpenTag || no;
    var index = 0;
    var last, lastTag;
    while (html) {//循环 直到当前的html文档为空
      last = html;
      // 确保即将 parse 的内容不是在纯文本标签里 (script,style,textarea)
      if (!lastTag || !isPlainTextElement(lastTag)) {
        var textEnd = html.indexOf('<');
        if (textEnd === 0) {//文本以'<'开头
          // 1是注释吗 <!-- 我是注释 --> 
          if (comment.test(html)) {
            
          }

          // 解析是否是条件注释 <!-- [if !IE] --> <!-- [endif] -->
          if (conditionalComment.test(html)) {
            var conditionalEnd = html.indexOf(']>');

            if (conditionalEnd >= 0) {
              advance(conditionalEnd + 2);
              continue
            }
          }

          // 解析是否是DOCTYPE <!DOCTYPE html>
          var doctypeMatch = html.match(doctype);
          if (doctypeMatch) {
            advance(doctypeMatch[0].length);
            continue
          }

          // 解析是否是结束标签 </div>
          var endTagMatch = html.match(endTag);
          if (endTagMatch) {
            var curIndex = index;
            advance(endTagMatch[0].length);
            parseEndTag(endTagMatch[1], curIndex, index);
            continue
          }

          // 匹配是否是开始标签 <div>
          var startTagMatch = parseStartTag();
          if (startTagMatch) {
            handleStartTag(startTagMatch);
            if (shouldIgnoreFirstNewline(startTagMatch.tagName, html)) {
              advance(1);
            }
            continue
          }
        }

        var text = (void 0), rest = (void 0), next = (void 0);
        if (textEnd >= 0) { // 如果html字符串不是以'<'开头,则解析文本类型
          
        }

        if (textEnd < 0) { // 如果在html字符串中没有找到'<'，表示这一段html字符串都是纯文本
          text = html;
        }

        if (options.chars && text) { // 把截取出来的text转化成textAST
          options.chars(text, index - text.length, index);
        }
      } else { //父元素为script、style、textarea时，其内部的内容全部当做纯文本处理
        var endTagLength = 0;
        var stackedTag = lastTag.toLowerCase();
        var reStackedTag = reCache[stackedTag] || (reCache[stackedTag] = new RegExp('([\\s\\S]*?)(</' + stackedTag + '[^>]*>)', 'i'));
        var rest$1 = html.replace(reStackedTag, function (all, text, endTag) {
          endTagLength = endTag.length;
          if (!isPlainTextElement(stackedTag) && stackedTag !== 'noscript') {
            text = text
              .replace(/<!\--([\s\S]*?)-->/g, '$1') // #7298
              .replace(/<!\[CDATA\[([\s\S]*?)]]>/g, '$1');
          }
          if (shouldIgnoreFirstNewline(stackedTag, text)) {
            text = text.slice(1);
          }
          if (options.chars) {
            options.chars(text);
          }
          return ''
        });
        index += html.length - rest$1.length;
        html = rest$1;
        parseEndTag(stackedTag, index - endTagLength, index);
      }

      if (html === last) {
        options.chars && options.chars(html);
        if (!stack.length && options.warn) {
          options.warn(("Mal-formatted tag at end of template: \"" + html + "\""), { start: index + html.length });
        }
        break
      }
    }

    // Clean up any remaining tags
    parseEndTag();

    function advance (n) {
      index += n;
      html = html.substring(n);
    }

    function parseStartTag () {
      var start = html.match(startTagOpen);
      if (start) {
        var match = {
          tagName: start[1],
          attrs: [],
          start: index
        };
        advance(start[0].length);
        var end, attr;
        while (!(end = html.match(startTagClose)) && (attr = html.match(dynamicArgAttribute) || html.match(attribute))) {
          attr.start = index;
          advance(attr[0].length);
          attr.end = index;
          match.attrs.push(attr);
        }
        if (end) {
          match.unarySlash = end[1];
          advance(end[0].length);
          match.end = index;
          return match
        }
      }
    }

    function handleStartTag (match) {
      var tagName = match.tagName;
      var unarySlash = match.unarySlash;

      if (expectHTML) {
        if (lastTag === 'p' && isNonPhrasingTag(tagName)) {
          parseEndTag(lastTag);
        }
        if (canBeLeftOpenTag$$1(tagName) && lastTag === tagName) {
          parseEndTag(tagName);
        }
      }

      var unary = isUnaryTag$$1(tagName) || !!unarySlash;

      var l = match.attrs.length;
      var attrs = new Array(l);
      for (var i = 0; i < l; i++) {
        var args = match.attrs[i];
        var value = args[3] || args[4] || args[5] || '';
        var shouldDecodeNewlines = tagName === 'a' && args[1] === 'href'
          ? options.shouldDecodeNewlinesForHref
          : options.shouldDecodeNewlines;
        attrs[i] = {
          name: args[1],
          value: decodeAttr(value, shouldDecodeNewlines)
        };
        if (options.outputSourceRange) {
          attrs[i].start = args.start + args[0].match(/^\s*/).length;
          attrs[i].end = args.end;
        }
      }

      if (!unary) {
        stack.push({ tag: tagName, lowerCasedTag: tagName.toLowerCase(), attrs: attrs, start: match.start, end: match.end });
        lastTag = tagName;
      }

      if (options.start) {
        options.start(tagName, attrs, unary, match.start, match.end);
      }
    }

    function parseEndTag (tagName, start, end) {
      var pos, lowerCasedTagName;
      if (start == null) { start = index; }
      if (end == null) { end = index; }

      // Find the closest opened tag of the same type
      if (tagName) {
        lowerCasedTagName = tagName.toLowerCase();
        for (pos = stack.length - 1; pos >= 0; pos--) {
          if (stack[pos].lowerCasedTag === lowerCasedTagName) {
            break
          }
        }
      } else {
        // If no tag name is provided, clean shop
        pos = 0;
      }

      if (pos >= 0) {
        // Close all the open elements, up the stack
        for (var i = stack.length - 1; i >= pos; i--) {
          if (i > pos || !tagName &&
            options.warn
          ) {
            options.warn(
              ("tag <" + (stack[i].tag) + "> has no matching end tag."),
              { start: stack[i].start, end: stack[i].end }
            );
          }
          if (options.end) {
            options.end(stack[i].tag, start, end);
          }
        }

        // Remove the open elements from the stack
        stack.length = pos;
        lastTag = pos && stack[pos - 1].tag;
      } else if (lowerCasedTagName === 'br') {
        if (options.start) {
          options.start(tagName, [], true, start, end);
        }
      } else if (lowerCasedTagName === 'p') {
        if (options.start) {
          options.start(tagName, [], false, start, end);
        }
        if (options.end) {
          options.end(tagName, start, end);
        }
      }
    }
  }
```

### 2.2 优化阶段 (优化AST的结构)

* 在`AST`中找出所有静态节点并打上标记（静态节点是指那些节点中没有插值表达式的）
* 在`AST`中找出所有静态根节点并打上标记
* 目的为了提高虚拟`DOM`中`patch`过程的性能。在优化阶段将所有静态节点都打上标记，这样在`patch`过程中就可以跳过对比这些节点。

```js
export function optimize (root: ?ASTElement, options: CompilerOptions) {
  if (!root) return
  isStaticKey = genStaticKeysCached(options.staticKeys || '')
  isPlatformReservedTag = options.isReservedTag || no
  // 标记静态节点
  markStatic(root)
  // 标记静态根节点
  markStaticRoots(root, false)
}
```

```jsx
//一个模板
<div id="NLRX">
    <p>Hello {{name}}</p>
</div>

//经过模板解析和优化得到的ast如下:
ast = {
    'type': 1,
    'tag': 'div',
    'attrsList': [
        {
            'name':'id',
            'value':'NLRX',
        }
    ],
    'attrsMap': {
      'id': 'NLRX',
    },
    'static':false,
    'parent': undefined,
    'plain': false,
    'children': [{
      'type': 1,
      'tag': 'p',
      'plain': false,
      'static':false,
      'children': [
        {
            'type': 2,
            'expression': '"Hello "+_s(name)',
            'text': 'Hello {{name}}',
            'static':false,
        }
      ]
    }]
  }
```



### 2.3 代码生成阶段（使用系统的Function,输入AST产生一个render函数,调用render函数产生虚拟DOM）

* 根据模板对应的抽象语法树`AST`生成一个render函数，通过调用这个函数就可以得到模板对应的虚拟`DOM`。
* 简单的来说就是`Vue`只要调用了`render`函数，就可以把模板转换成对应的虚拟`DOM`.
* 用户可以在`Vue`组件选项中手写一个`render`选项，其值对应一个函数，那这个函数就是`render`函数，当用户手写了`render`函数时，那么`Vue`在挂载该组件的时候就会调用用户手写的这个`render`函数。(一般根组件会写)
* 用户没有写的话(普通组件),那这个时候`Vue`就要自己根据模板内容生成一个`render`函数供组件挂载的时候调用。

```js
export function generate (ast,option) {
  const state = new CodegenState(options)
  //如果ast不为空,就调用genElement(ast, state)函数创建VNode，为空则创建一个空的元素型div的VNode
  const code = ast ? genElement(ast, state) : '_c("div")' 
  return {
    render: `with(this){return ${code}}`, //产生一个渲染所需的函数字符串，如下例
    staticRenderFns: state.staticRenderFns
  }
}
```

```js

`
with(this){
    return _c( // _c()是一个内部函数，用于创建一个元素节点
        'div',
        {
            attrs:{"id":"NLRX"},
        }
        [
            _c('p'),
            [
                _v("Hello "+_s(name)) //_v()是一个内部函数，用于创建一个文本节点，_c()用于创建
            ]
        ])
}
`  
function createFunction (code, errors) {
  try {
    return new Function(code) //通过字符串产生一个函数
  } catch (err) {
    errors.push({ err, code })
    return noop
  }
}
//通过createFunction将函数字符串转换成真正的函数，赋给组件中的render选项，从而就是render函数了
res.render = createFunction(compiled.render, fnGenErrors) 
```



```js
const code = generate(ast, options)
```

### 2.4 总结（调用$mount()方法可以把虚拟DOM挂载到页面上）

* 模板编译最终目的就是：把用户所写的模板转化成供`Vue`实例在挂载时可调用的`render`函数。
* vue在进行实例挂载的时候，会调用$mount()方法，该方法内部就会去实例的options中寻找render函数

```js
//选择挂载元素
function query (el) {
  if (typeof el === 'string') {
    var selected = document.querySelector(el);
    if (!selected) {
      warn(
        'Cannot find element: ' + el
      );
      return document.createElement('div')
    }
    return selected
  } else {
    return el
  }
}


var mount = Vue.prototype.$mount; //缓存vue运行版本的的$mount()函数
Vue.prototype.$mount = function (el){
    el = el && query(el); //拿到el指向的DOM元素
  const options = this.$options
  // 如果用户没有手写render函数
  if (!options.render) {
    // 获取模板，先尝试获取内部模板，如果获取不到则获取外部模板
    let template = options.template
    if (template) {//如果存在template(.vue文件存在) 
        
        //对template的合法性进行检查
        
    } else if(el){//如果存在el属性,就直接代理el指定的HTML模板
      template = getOuterHTML(el)
    }
    //用户没有手写就根据模板创建一个render函数
    const { render, staticRenderFns } = compileToFunctions(template, {
      shouldDecodeNewlines,
      shouldDecodeNewlinesForHref,
      delimiters: options.delimiters,
      comments: options.comments
    }, this)
    options.render = render
    options.staticRenderFns = staticRenderFns
  }
  return mount.call(this, el, hydrating) //执行运行版本的$mount()函数
}
```

## 3.变化侦测++

### 3.1 Object的变化侦测

#### 3.1.1 Object.defineProperty`方法

```js

let car = {}
let val = 3000
Object.defineProperty(car, 'price', {
  enumerable: true,
  configurable: true,
  get(){//当这样使用的时候,car['price']的时候会触发这个函数
    console.log('price属性被读取了')
    return val //返回价格
  },
  set(newVal){
    console.log('price属性被修改了')
    val = newVal
  }
})

```

#### 3.1.2 定义一个观察者类

```js
//传入一个需要被观察的数据对象和值 比如defineReactive(car,'price')
function defineReactive (obj,key,val) {
  // 如果只传了obj和key，那么val = obj[key]
  if (arguments.length === 2) {
    val = obj[key]
  }
    
  if(typeof val === 'object'){
      new Observer(val)
  }
    
  Object.defineProperty(obj, key, {
    enumerable: true,
    configurable: true,
    get(){
      dep.depend()    // 在getter中收集依赖
      return val;
    },
    set(newVal){
      if(val === newVal){
          return
      }
      val = newVal;
      dep.notify()   // 在setter中通知依赖更新
    }
  })
}

//该类的目的是把一个数据对象中的数据设置set和get
export class Observer {
    //这里的value是一个对象字面量
  constructor (value) {
    this.value = value
    // 给value新增一个__ob__属性，值为该value的Observer实例
    // 相当于为value打上标记，表示它已经被转化成响应式了，避免重复操作
    def(value,'__ob__',this)
    if (Array.isArray(value)) {
      // 当value为数组时的逻辑，数组需要单独处理
      // ...
    } else {
      this.walk(value)//调用Observer类的walk方法 给value设置set和get
    }
  }

    //循环给数据对象的每一个属性都设置set和get
  walk (obj: Object) {
    const keys = Object.keys(obj)
    for (let i = 0; i < keys.length; i++) {
      defineReactive(obj, keys[i])
    }
  }
}

//比如 这样，car的两个属性都变得可观测了。
let car = new Observer({
  'brand':'BMW',
  'price':3000
})
```

#### 3.1.3 依赖收集

* 当数据发生变化时，我们去通知视图更新就好了
* 我们把"谁用到了这个数据"称为"谁依赖了这个数据"
* 给每一个数据都设置一个依赖数组，如果这个数据发生了变化，就通知这个数据中每一个都去更新自己
* 总结一句话就是：**在getter中收集依赖，在setter中通知依赖更新**。

```js
//该类的目的是一个数据建立一个依赖数组
export default class Dep {
  constructor () {
    this.subs = []
  }

  addSub (sub) {
    this.subs.push(sub)
  }
  // 删除一个依赖
  removeSub (sub) {
    remove(this.subs, sub)
  }
  // 添加一个依赖
  depend () {
    if (window.target) {
      this.addSub(window.target)
    }
  }
  // 通知所有依赖更新
  notify () {
    const subs = this.subs.slice()
    for (let i = 0, l = subs.length; i < l; i++) {
      subs[i].update()//调用该依赖的update方法
    }
  }
}
```

#### 3.1.4 依赖是什么

* 虽然我们一直在说”谁用到了这个数据谁就是依赖“，但是这仅仅是在口语层面上，那么反应在代码上该如何来描述这个”谁“呢？
* 其实在`Vue`中还实现了一个叫做`Watcher`的类，而`Watcher`类的实例就是我们上面所说的那个"谁"。换句话说就是：谁用到了数据，谁就是依赖，我们就为谁创建一个`Watcher`实例。
* 在之后数据变化时，我们不直接去通知依赖更新，而是通知依赖对应的`Watch`实例，由`Watcher`实例去通知真正的视图。

```js

/**
 * Parse simple path.
 * 把一个形如'data.a.b.c'的字符串路径所表示的值，从真实的data对象中取出来
 * 例如：
 * data = {a:{b:{c:2}}}
 * parsePath('a.b.c')(data)  // 2
 */
const bailRE = /[^\w.$]/
function parsePath (path) { //一个辅助函数
  if (bailRE.test(path)) {
    return
  }
  const segments = path.split('.')
  return function (obj) {
    for (let i = 0; i < segments.length; i++) {
      if (!obj) return
      obj = obj[segments[i]]
    }
    return obj
  }
}

//该类的目的是构建一个依赖，输出(当前的组件实例，依赖的数据，数据变化时候需要执行的回调函数)
export default class Watcher {
  constructor (vm,expOrFn,cb) {
    this.vm = vm;
    this.cb = cb; //当数据变化的时候需要执行的函数
    this.getter = parsePath(expOrFn)
    this.value = this.get()//调用一下get()方法，触发数据的getter，将一个依赖添加到该数据的依赖数组中
  }
  get () {
    window.target = this;
    const vm = this.vm
    let value = this.getter.call(vm, vm) //读取这个值，触发getter
    window.target = undefined;
    return value
  }
  update () { //触发数据的setter时候，会调用该方法执行里面的回调函数
    const oldValue = this.value
    this.value = this.get()
    this.cb.call(this.vm, this.value, oldValue) //执行这个回调函数
  }
}
```

#### 3.1.5 不足之处

虽然我们通过`Object.defineProperty`方法实现了对`object`数据的可观测，但是这个方法仅仅只能观测到`object`数据的取值及设置值，当我们向`object`数据里添加一对新的`key/value`或删除一对已有的`key/value`时，它是无法观测到的，导致当我们对`object`数据添加或删除值时，无法通知依赖，无法驱动视图进行响应式更新。

当然，`Vue`也注意到了这一点，为了解决这一问题，`Vue`增加了两个全局API:`Vue.set`和`Vue.delete`，这两个API的实现原理将会在后面学习全局API的时候说到

### 3.2 Array的变化侦测

<img src="../../../AppData/Roaming/Typora/typora-user-images/image-20200708173102981.png" alt="image-20200708173102981" style="zoom:80%;" />

#### 3.2.1 不足之处

对于数组变化侦测是通过拦截器实现的，也就是说只要是通过数组原型上的方法对数组进行操作就都可以侦测到，但是别忘了，我们在日常开发中，还可以通过数组的下标来操作数据，如下：

```js
let arr = [1,2,3]
arr[0] = 5;       // 通过数组下标修改数组中的数据
arr.length = 0    // 通过修改数组长度清空数组
```

而使用上述例子中的操作方式来修改数组是无法侦测到的。 同样，`Vue`也注意到了这个问题， 为了解决这一问题，`Vue`增加了两个全局API:`Vue.set`和`Vue.delete`

# 一、目录结构

* 这个是从GitHub上clone下来的，对应的vue版本为2.6.11

```
dist                            #项目构建后的文件，使用vue都是引入这个里面的
examples                        #利用vue的一些案例
flow                            #flow的类型声明文件
packages
script                          #项目构建相关的脚本和配置文件
src
----complier                    #与模板编译相关的代码[重点]
----core                        #通用的、与运行平台无关的运行时代码
--------components              	#vue内置的组件的代码 如keep-alive
--------global-api              	#全局api代码
--------instance                	#产生vue的实例的构造函数和相应的原型方法[重点]
--------observer                	#实现变侦测的代码[重点]
--------util                    	#构建vue实例所需要的一些辅助函数，比如发出警告并在控制台打印，标准化options等
--------vdom                    	#实现虚拟dom的代码[重点]                    
--------config.js               	#产生vue实例上面的一些初始options
--------index.js               		#将Vue导出，以可以在全局中使用
----platforms                   #在特定平台上可以使用代码 如weex
----server                      #服务端渲染相关的代码
----sfc                         #单文件组件的解析代码 .vue文件
----shared                      #项目公用的工具代码 和一些vue中使用的常量[重要]
test
types
```

## 1.1 说明

* flow类型声明是为了解决js由于是弱语言的缺点，使用这个工具就可以强制限定数据的类型，在需要使用flow的文件的头部添加//@flow就可以在这个文件中执行flow类型，vue使用了很多这个，例如

  ```js
  //函数中num1，nums2必须是数字，返回值也必须是数字
  function add(num1:number, num2:number) :number {
    return num1 + num2;
  }
  
  //foo可以是字符串也可以是null或者undefined
  var foo:?string = null;
  
  //限定对象字面量 a必须是字符串 b必须是数字,c必须是数组,d必须是Foo构造出来的实例
  var obj : {a : string, b : number, c : Array<string>, d : Foo} = {
    a : "kongzhi",
    b : 1,
    c : ["kongzhi111", "kongzhi222"],
    d : new Foo("hello", 1)
  }
  ```

* shared文件夹下面包含了一些工具代码，如

  ```js
  //这是函数是执行了flow类型检查的，所以和平常的JS不一样
  export function isUndef (v: any): boolean %checks {
    return v === undefined || v === null
  }
  
  export function isDef (v: any): boolean %checks {
    return v !== undefined && v !== null
  }
  
  export function isTrue (v: any): boolean %checks {
    return v === true
  }
  
  export function isFalse (v: any): boolean %checks {
    return v === false
  }
  
  export function makeMap (
    str: string,
    expectsLowerCase?: boolean
  ): (key: string) => true | void {
    const map = Object.create(null)
    const list: Array<string> = str.split(',')
    for (let i = 0; i < list.length; i++) {
      map[list[i]] = true
    }
    return expectsLowerCase
      ? val => map[val.toLowerCase()]
      : val => map[val]
  }
  //检查是不是一个对象 数组 {}也是对象
  export function isObject (obj: mixed): boolean %checks {
    return obj !== null && typeof obj === 'object'
  }
  //检查是不是{}
  export function isPlainObject (obj: any): boolean {
    return _toString.call(obj) === '[object Object]'
  }
  ```

## 1.2 入口

* 入口位于instance/index.js

```js
import { initMixin } from './init'
import { stateMixin } from './state'
import { renderMixin } from './render'
import { eventsMixin } from './events'
import { lifecycleMixin } from './lifecycle'
import { warn } from '../util/index'

//options为构建vue实例的对象，就是{props:  data:  methods: ...}
function Vue (options) {
  if (process.env.NODE_ENV !== 'production' &&
    !(this instanceof Vue)
  ) {
    warn('Vue is a constructor and should be called with the `new` keyword')
  }
  this._init(options)
}

initMixin(Vue) //在vue的prototype添加_init()方法
stateMixin(Vue) //在vue的prototype添加$data $props $watch() $set() $delete()方法
eventsMixin(Vue) //在vue的prototype添加$emit() $off() $on() $once()方法
lifecycleMixin(Vue) //在vue的prototype添加$forceUpdate() $destroy() 
renderMixin(Vue) //在vue的prototype添加$nextTick()

export default Vue
```

# 二、vue实例建立的步骤（从上到下）

```js
export function initMixin{
  Vue.prototype._init = function (options) {
    const vm = this
    if (options && options._isComponent) {
      initInternalComponent(vm, options)
    } else {
      vm.$options = mergeOptions(
         //将默认的一些options,比如components,directives,filters,这也是为什么可以直接使用一些内置指令和组件的原因
        resolveConstructorOptions(vm.constructor),
        options || {}, //自己写的options，就是.vue文件中的scrpit中的
        vm
      )
    }
    // expose real self
    vm._self = vm
    initLifecycle(vm)
    initEvents(vm)
    initRender(vm)
    callHook(vm, 'beforeCreate')
    initInjections(vm) // resolve injections before data/props
    initState(vm)
    initProvide(vm) // resolve provide after data/props
    callHook(vm, 'created')
      //如果用户传入了el选项 则调用$mount函数进入模板编译与挂载阶段
      //如果没有传入el选项，则不进入下一个生命周期阶段，需要用户手动执行vm.$mount方法才进入下一个生命周期阶段。
    if (vm.$options.el) {
      vm.$mount(vm.$options.el)
    }
  }
}
```

## 2.1 [初始化阶段] 初始化生命周期相关的属性 initLifecycle(vm)

* 在实例上初始化一些属性和相应的值

```js
export function initLifecycle (vm: Component) {
  const options = vm.$options
//定位到实例的父组件实例，并且这个组件不是抽象组件，抽象组件的options.abstract=true，比如component,transition,transition-group,keep-alive,slot这些不会被渲染成真实的dom节点，是抽象的节点
  let parent = options.parent
  if (parent && !options.abstract) {
    while (parent.$options.abstract && parent.$parent) {
      parent = parent.$parent
    }
    parent.$children.push(vm)
  }
  //在实例上初始化一些属性和相应的值
  vm.$parent = parent
  vm.$root = parent ? parent.$root : vm

  vm.$children = []
  vm.$refs = {}

  vm._watcher = null
  vm._inactive = null
  vm._directInactive = false
  vm._isMounted = false
  vm._isDestroyed = false
  vm._isBeingDestroyed = false
}
```

## 2.2 [初始化阶段] 初始化实例的事件系统 initEvents(vm)

* 初始化实例的事件系统，当我们在父组件中使用子组件时可以给子组件上注册一些事件，这些事件即包括使用`v-on`或`@`注册的自定义事件，也包括注册的浏览器原生事件（需要加 `.native` 修饰符），如下：
* **实例初始化阶段调用的初始化事件函数`initEvents`实际上初始化的是父组件在模板中使用v-on或@注册的监听子组件内触发的事件。**

```html
<child @select="selectHandler" 	@click.native="clickHandler"></child>
```

```js
export function initEvents (vm: Component) {
  vm._events = Object.create(null) //在vm上新建一个_events属性，用来存储该组件上的所有的事件
  vm._hasHookEvent = false
  //获取父组件在该组件上设置的事件监听器
  const listeners = vm.$options._parentListeners
  if (listeners) {
     //如果父组件给该组件添加了事件监听，那么将父组件向子组件注册的事件注册到子组件的实例中
    updateComponentListeners(vm, listeners)
  }
}
```

父组件既可以给子组件上绑定自定义事件，也可以绑定浏览器原生事件。这两种事件有着不同的处理时机，浏览器原生事件是由父组件处理，而自定义事件是在子组件初始化的时候由父组件传给子组件，再由子组件注册到实例的事件系统中。



## 2.3 [初始化阶段] 初始化渲染 initRender(vm)

```js
export function initRender (vm: Component) {
  vm._vnode = null // the root of the child tree
  vm._staticTrees = null // v-once cached trees
  const options = vm.$options
  const parentVnode = vm.$vnode = options._parentVnode // the placeholder node in parent tree
  const renderContext = parentVnode && parentVnode.context
  vm.$slots = resolveSlots(options._renderChildren, renderContext)
  vm.$scopedSlots = emptyObject
  // bind the createElement fn to this instance
  // so that we get proper render context inside it.
  // args order: tag, data, children, normalizationType, alwaysNormalize
  // internal version is used by render functions compiled from templates
  vm._c = (a, b, c, d) => createElement(vm, a, b, c, d, false)
  // normalization is always applied for the public version, used in
  // user-written render functions.
  vm.$createElement = (a, b, c, d) => createElement(vm, a, b, c, d, true)

  // $attrs & $listeners are exposed for easier HOC creation.
  // they need to be reactive so that HOCs using them are always updated
  const parentData = parentVnode && parentVnode.data

  /* istanbul ignore else */
  if (process.env.NODE_ENV !== 'production') {
    defineReactive(vm, '$attrs', parentData && parentData.attrs || emptyObject, () => {
      !isUpdatingChildComponent && warn(`$attrs is readonly.`, vm)
    }, true)
    defineReactive(vm, '$listeners', options._parentListeners || emptyObject, () => {
      !isUpdatingChildComponent && warn(`$listeners is readonly.`, vm)
    }, true)
  } else {
    defineReactive(vm, '$attrs', parentData && parentData.attrs || emptyObject, null, true)
    defineReactive(vm, '$listeners', options._parentListeners || emptyObject, null, true)
  }
}
```

## 2.4 [初始化阶段] 触发钩子函数 callHook(vm, 'beforeCreate')

## 2.5 [初始化阶段] 初始化Injections initInjections(vm)

* 该函数是用来初始化实例中的`inject`选项的

## 2.6 [初始化阶段] 初始化状态 initState(vm) ++

* 初始化_watchers数组，用来存放该实例上的所有的侦听器，包括响应式默认加入的和自己手动在watch选项中添加的

```js
export function initState (vm: Component) {
  vm._watchers = [] //初始化_watchers数组，用来存放该实例上的所有的侦听器
  const opts = vm.$options
  //如果存在props需要先进行处理 initProps()进行规则检查 把数据变成响应式
  if (opts.props) initProps(vm, opts.props)
    
  //如果存在methods,initMethods()检查methods是否符合vue的规则
  if (opts.methods) initMethods(vm, opts.methods)
    
  if (opts.data) {
    initData(vm) //有data选项,就设置为响应式
  } else {
    observe(vm._data = {}, true /* asRootData */)
  }
  
  //computed属性规则检查 设置为响应式  
  if (opts.computed) initComputed(vm, opts.computed)
  
  //初始化侦听器  
  if (opts.watch && opts.watch !== nativeWatch) {
    initWatch(vm, opts.watch)
  }
}
```

## 2.7 [初始化阶段] 触发钩子函数 callHook(vm,'created')

## 2.8 [模板编译阶段] 模板编译阶段(只有完整版本的才用这个阶段)

* 完整版和只包含运行的版本

  ```js
  // 需要编译器的版本
  new Vue({
    template: '<div>{{ hi }}</div>'
  })
  
  // 不需要编译器 这个时候用户手写了render函数的内容
  new Vue({
    render (h) {
      return h('div', this.hi)
    }
  })
  ```

  ```js
  //选择挂载元素
  function query (el) {
    if (typeof el === 'string') {
      var selected = document.querySelector(el);
      if (!selected) {
        warn(
          'Cannot find element: ' + el
        );
        return document.createElement('div')
      }
      return selected
    } else {
      return el
    }
  }
  
  var mount = Vue.prototype.$mount; //缓存vue运行版本的的$mount()函数
  Vue.prototype.$mount = function (el){
      el = el && query(el); //拿到el指向的DOM元素
    const options = this.$options
    // 如果用户没有手写render函数
    if (!options.render) {
      // 获取模板，先尝试获取内部模板，如果获取不到则获取外部模板
      let template = options.template
      if (template) {//如果存在template(.vue文件存在) 
          
          //对template的合法性进行检查
          
      } else if(el){//如果存在el属性,就直接代理el指定的HTML模板
        template = getOuterHTML(el)
      }
      //用户没有手写就根据模板创建一个render函数
      const { render, staticRenderFns } = compileToFunctions(template, {
        shouldDecodeNewlines,
        shouldDecodeNewlinesForHref,
        delimiters: options.delimiters,
        comments: options.comments
      }, this)
      options.render = render
      options.staticRenderFns = staticRenderFns
    }
    return mount.call(this, el, hydrating) //执行运行版本的$mount()函数
  }
  ```

* 在初始化阶段各项工作做完之后vue会调用`vm.$mount`方法，该方法的调用标志着初始化阶段的结束和进入下一个阶段
* 如果options没有el选项，则需要手动调用`vm.$mount`方法

## 2.9 [模板编译阶段] 触发钩子函数callHook(vm, 'beforeMount')

## 2.10 [模板编译阶段] 挂载 [两个步骤]

* 第一部分是将模板渲染到视图上
* 第二部分是开启对模板中数据（状态）的监控

```js
Vue.prototype.$mount = function (el,hydrating) {
  el = el && inBrowser ? query(el) : undefined;
  return mountComponent(this, el, hydrating)
};

export function mountComponent (vm,el,hydrating) {
    vm.$el = el
    if (!vm.$options.render) { //不存在render函数,默认创建一个
        vm.$options.render = createEmptyVNode
    }
    callHook(vm, 'beforeMount')

    let updateComponent

    updateComponent = () => {//声明一个函数，就会把最新的模板内容渲染到视图页面
        vm._update(vm._render(), hydrating)
    }
    //新建一个监听器，对这个视图中的数据进行监听，同时执行updateComponent函数
    new Watcher(vm, updateComponent, noop, {
        before () {
            if (vm._isMounted) {
                callHook(vm, 'beforeUpdate')
            }
        }
    }, true /* isRenderWatcher */)
    hydrating = false

    if (vm.$vnode == null) {
        vm._isMounted = true
        callHook(vm, 'mounted')
    }
    return vm
}
```

## 2.11 [模板编译阶段] 触发钩子函数callHook(vm, 'mounted')

## 2.12 [销毁阶段] 销毁

* 在该阶段所做的主要工作是将当前的`Vue`实例从其父级实例中删除
* 消当前实例上的所有依赖追踪并且移除实例上的所有事件监听器

```js
Vue.prototype.$destroy = function () {
  const vm: Component = this
  if (vm._isBeingDestroyed) {
    return
  }
  callHook(vm, 'beforeDestroy') //触发钩子函数
  vm._isBeingDestroyed = true
  // 将自己从父组件删除
  const parent = vm.$parent
  if (parent && !parent._isBeingDestroyed && !vm.$options.abstract) {
    remove(parent.$children, vm)
  }
  // 移除监听器
  if (vm._watcher) {
    vm._watcher.teardown()
  }
  let i = vm._watchers.length
  while (i--) {
    vm._watchers[i].teardown()
  }
  // remove reference from data ob
  // frozen object may not have observer.
  if (vm._data.__ob__) {
    vm._data.__ob__.vmCount--
  }
  // call the last hook...
  vm._isDestroyed = true
  // invoke destroy hooks on current rendered tree
  vm.__patch__(vm._vnode, null)
  // fire destroyed hook
  callHook(vm, 'destroyed') //触发钩子函数
  // turn off all instance listeners.
  vm.$off()
  // remove __vue__ reference
  if (vm.$el) {
    vm.$el.__vue__ = null
  }
  // release circular reference (##6759)
  if (vm.$vnode) {
    vm.$vnode.parent = null
  }
}
```

# 三、总结

![image-20200724204215445](../../../AppData/Roaming/Typora/typora-user-images/image-20200724204215445.png)