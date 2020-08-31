参考书籍:《HTML5权威指南》

# 一、 继承关系

```
                                       Document类 --> HTMLDocument类(产生document对象的类)
                                       |
  Object类--> EventTarget类--> Node类 -->  
                                       |
                                       Element类 -->HTMLElement类 -->HTMLDivElement类(产生各种html元素标签对象的类)
                                                                     HTMLhtmlElement类
                                                                     HTMLTitleElement类
                                                                     ...
```

* 上面显示DOM中各种类之间的继承关系

# EventTarget类

```js
addEventListener: ƒ addEventListener()
dispatchEvent: ƒ dispatchEvent()
removeEventListener: ƒ removeEventListener()
```

# Node类

* 在prototype上面定义了一些表示节点类型的数字

```js
ATTRIBUTE_NODE: 2 //属性节点
CDATA_SECTION_NODE: 4
COMMENT_NODE: 8 //注释节点
DOCUMENT_FRAGMENT_NODE: 11
DOCUMENT_NODE: 9 //document节点
DOCUMENT_POSITION_CONTAINED_BY: 16
DOCUMENT_POSITION_CONTAINS: 8
DOCUMENT_POSITION_DISCONNECTED: 1
DOCUMENT_POSITION_FOLLOWING: 4
DOCUMENT_POSITION_IMPLEMENTATION_SPECIFIC: 32
DOCUMENT_POSITION_PRECEDING: 2
DOCUMENT_TYPE_NODE: 10
ELEMENT_NODE: 1 //html元素标签节点
ENTITY_NODE: 6
ENTITY_REFERENCE_NODE: 5
NOTATION_NODE: 12
PROCESSING_INSTRUCTION_NODE: 7
TEXT_NODE: 3 //文本节点
```

* 其prototype上有以下方法:

```js
appendChild: ƒ appendChild()
baseURI: (...)
childNodes: (...)
cloneNode: ƒ cloneNode()
compareDocumentPosition: ƒ compareDocumentPosition()
contains: ƒ contains() //重要
firstChild: (...)
getRootNode: ƒ getRootNode()
hasChildNodes: ƒ hasChildNodes()
insertBefore: ƒ insertBefore() //重要
isConnected: (...)
isDefaultNamespace: ƒ isDefaultNamespace()
isEqualNode: ƒ isEqualNode()
isSameNode: ƒ isSameNode()
lastChild: (...)
lookupNamespaceURI: ƒ lookupNamespaceURI()
lookupPrefix: ƒ lookupPrefix()
nextSibling: (...)
nodeName: (...) //重要
nodeType: (...) //重要
nodeValue: (...) //重要
normalize: ƒ normalize()
ownerDocument: (...)
parentElement: (...) //重要
parentNode: (...)
previousSibling: (...)
removeChild: ƒ removeChild() //重要
replaceChild: ƒ replaceChild()
textContent: (...)
```



# Document类

* 其prototype上有:

```js
URL: (...)
activeElement: (...)
adoptNode: ƒ adoptNode()
adoptedStyleSheets: (...)
alinkColor: (...)
all: (...)
anchors: (...)
append: ƒ append()
applets: (...)
baseURI: (...)
bgColor: (...)
body: (...)
captureEvents: ƒ captureEvents()
caretRangeFromPoint: ƒ caretRangeFromPoint()
characterSet: (...)
charset: (...)
childElementCount: [Exception: TypeError: Illegal invocation at Document.invokeGetter (<anonymous>:1:142)]
childNodes: (...)
children: [Exception: TypeError: Illegal invocation at Document.invokeGetter (<anonymous>:1:142)]
clear: ƒ clear()
close: ƒ close()
compatMode: (...)
contentType: (...)
cookie: (...)
createAttribute: ƒ createAttribute()
createAttributeNS: ƒ createAttributeNS()
createCDATASection: ƒ createCDATASection()
createComment: ƒ createComment()
createDocumentFragment: ƒ createDocumentFragment()
createElement: ƒ createElement()
createElementNS: ƒ createElementNS()
createEvent: ƒ createEvent()
createExpression: ƒ createExpression()
createNSResolver: ƒ createNSResolver()
createNodeIterator: ƒ createNodeIterator()
createProcessingInstruction: ƒ createProcessingInstruction()
createRange: ƒ createRange()
createTextNode: ƒ createTextNode()
createTreeWalker: ƒ createTreeWalker()
currentScript: (...)
defaultView: (...)
designMode: (...)
dir: (...)
doctype: (...)
documentElement: (...)
documentURI: (...)
domain: (...)
elementFromPoint: ƒ elementFromPoint()
elementsFromPoint: ƒ elementsFromPoint()
embeds: (...)
evaluate: ƒ evaluate()
execCommand: ƒ execCommand()
exitFullscreen: ƒ exitFullscreen()
exitPictureInPicture: ƒ ()
exitPointerLock: ƒ exitPointerLock()
featurePolicy: (...)
fgColor: (...)
firstChild: (...)
firstElementChild: (...)
fonts: (...)
forms: (...)
fullscreen: (...)
fullscreenElement: (...)
fullscreenEnabled: (...)
getElementById: ƒ getElementById()
getElementsByClassName: ƒ getElementsByClassName()
getElementsByName: ƒ getElementsByName()
getElementsByTagName: ƒ getElementsByTagName()
getElementsByTagNameNS: ƒ getElementsByTagNameNS()
getSelection: ƒ getSelection()
hasFocus: ƒ hasFocus()
head: (...)
hidden: (...)
images: (...)
implementation: (...)
importNode: ƒ importNode()
inputEncoding: (...)
isConnected: (...)
lastChild: (...)
lastElementChild: (...)
lastModified: (...)
linkColor: (...)
links: (...)
nextSibling: (...)
nodeName: (...)
nodeType: (...)
nodeValue: (...)
onabort: (...)
onanimationend: (...)
onanimationiteration: (...)
onanimationstart: (...)
onauxclick: (...)
onbeforecopy: (...)
onbeforecut: (...)
onbeforepaste: (...)
onblur: (...)
oncancel: (...)
oncanplay: (...)
oncanplaythrough: (...)
onchange: (...)
onclick: (...)
onclose: (...)
oncontextmenu: (...)
oncopy: (...)
oncuechange: (...)
oncut: (...)
ondblclick: (...)
ondrag: (...)
ondragend: (...)
ondragenter: (...)
ondragleave: (...)
ondragover: (...)
ondragstart: (...)
ondrop: (...)
ondurationchange: (...)
onemptied: (...)
onended: (...)
onerror: (...)
onfocus: (...)
onformdata: (...)
onfreeze: (...)
onfullscreenchange: (...)
onfullscreenerror: (...)
ongotpointercapture: (...)
oninput: (...)
oninvalid: (...)
onkeydown: (...)
onkeypress: (...)
onkeyup: (...)
onload: (...)
onloadeddata: (...)
onloadedmetadata: (...)
onloadstart: (...)
onlostpointercapture: (...)
onmousedown: (...)
onmouseenter: (...)
onmouseleave: (...)
onmousemove: (...)
onmouseout: (...)
onmouseover: (...)
onmouseup: (...)
onmousewheel: (...)
onpaste: (...)
onpause: (...)
onplay: (...)
onplaying: (...)
onpointercancel: (...)
onpointerdown: (...)
onpointerenter: (...)
onpointerleave: (...)
onpointerlockchange: (...)
onpointerlockerror: (...)
onpointermove: (...)
onpointerout: (...)
onpointerover: (...)
onpointerrawupdate: (...)
onpointerup: (...)
onprogress: (...)
onratechange: (...)
onreadystatechange: (...)
onreset: (...)
onresize: (...)
onresume: (...)
onscroll: (...)
onsearch: (...)
onsecuritypolicyviolation: (...)
onseeked: (...)
onseeking: (...)
onselect: (...)
onselectionchange: (...)
onselectstart: (...)
onstalled: (...)
onsubmit: (...)
onsuspend: (...)
ontimeupdate: (...)
ontoggle: (...)
ontransitionend: (...)
onvisibilitychange: (...)
onvolumechange: (...)
onwaiting: (...)
onwebkitanimationend: (...)
onwebkitanimationiteration: (...)
onwebkitanimationstart: (...)
onwebkitfullscreenchange: (...)
onwebkitfullscreenerror: (...)
onwebkittransitionend: (...)
onwheel: (...)
open: ƒ open()
ownerDocument: (...)
parentElement: (...)
parentNode: (...)
pictureInPictureElement: (...)
pictureInPictureEnabled: (...)
plugins: (...)
pointerLockElement: (...)
prepend: ƒ prepend()
previousSibling: (...)
queryCommandEnabled: ƒ queryCommandEnabled()
queryCommandIndeterm: ƒ queryCommandIndeterm()
queryCommandState: ƒ queryCommandState()
queryCommandSupported: ƒ queryCommandSupported()
queryCommandValue: ƒ queryCommandValue()
querySelector: ƒ querySelector()
querySelectorAll: ƒ querySelectorAll()
readyState: (...)
referrer: (...)
releaseEvents: ƒ releaseEvents()
rootElement: (...)
scripts: (...)
scrollingElement: (...)
styleSheets: (...)
textContent: (...)
title: (...)
visibilityState: (...)
vlinkColor: (...)
wasDiscarded: (...)
webkitCancelFullScreen: ƒ webkitCancelFullScreen()
webkitCurrentFullScreenElement: (...)
webkitExitFullscreen: ƒ webkitExitFullscreen()
webkitFullscreenElement: (...)
webkitFullscreenEnabled: (...)
webkitHidden: (...)
webkitIsFullScreen: (...)
webkitVisibilityState: (...)
write: ƒ write()
writeln: ƒ writeln()
xmlEncoding: [Exception: TypeError: Illegal invocation at Document.invokeGetter (<anonymous>:1:142)]
xmlStandalone: [Exception: TypeError: Illegal invocation at Document.invokeGetter (<anonymous>:1:142)]
xmlVersion: [Exception: TypeError: Illegal invocation at Document.invokeGetter (<anonymous>:1:142)]
Symbol(Symbol.unscopables): {append: true, fullscreen:
```

# HTMLDocument类

* 其prototype上有:

```js
URL: (...)
activeElement: (...)
adoptedStyleSheets: (...)
alinkColor: (...)
all: (...)
anchors: (...)
applets: (...)
baseURI: (...)
bgColor: (...)
body: (...)
characterSet: (...)
charset: (...)
childElementCount: (...)
childNodes: (...)
children: (...)
compatMode: (...)
contentType: (...)
cookie: (...)
currentScript: (...)
defaultView: (...)
designMode: (...)
dir: (...)
doctype: (...)
documentElement: (...)
documentURI: (...)
domain: (...)
embeds: (...)
featurePolicy: (...)
fgColor: (...)
firstChild: (...)
firstElementChild: (...)
fonts: (...)
forms: (...)
fullscreen: (...)
fullscreenElement: (...)
fullscreenEnabled: (...)
head: (...)
hidden: (...)
images: (...)
implementation: (...)
inputEncoding: (...)
isConnected: (...)
lastChild: (...)
lastElementChild: (...)
lastModified: (...)
linkColor: (...)
links: (...)
nextSibling: (...)
nodeName: (...)
nodeType: (...)
nodeValue: (...)
onabort: (...)
onanimationend: (...)
onanimationiteration: (...)
onanimationstart: (...)
onauxclick: (...)
onbeforecopy: (...)
onbeforecut: (...)
onbeforepaste: (...)
onblur: (...)
oncancel: (...)
oncanplay: (...)
oncanplaythrough: (...)
onchange: (...)
onclick: (...)
onclose: (...)
oncontextmenu: (...)
oncopy: (...)
oncuechange: (...)
oncut: (...)
ondblclick: (...)
ondrag: (...)
ondragend: (...)
ondragenter: (...)
ondragleave: (...)
ondragover: (...)
ondragstart: (...)
ondrop: (...)
ondurationchange: (...)
onemptied: (...)
onended: (...)
onerror: (...)
onfocus: (...)
onformdata: (...)
onfreeze: (...)
onfullscreenchange: (...)
onfullscreenerror: (...)
ongotpointercapture: (...)
oninput: (...)
oninvalid: (...)
onkeydown: (...)
onkeypress: (...)
onkeyup: (...)
onload: (...)
onloadeddata: (...)
onloadedmetadata: (...)
onloadstart: (...)
onlostpointercapture: (...)
onmousedown: (...)
onmouseenter: (...)
onmouseleave: (...)
onmousemove: (...)
onmouseout: (...)
onmouseover: (...)
onmouseup: (...)
onmousewheel: (...)
onpaste: (...)
onpause: (...)
onplay: (...)
onplaying: (...)
onpointercancel: (...)
onpointerdown: (...)
onpointerenter: (...)
onpointerleave: (...)
onpointerlockchange: (...)
onpointerlockerror: (...)
onpointermove: (...)
onpointerout: (...)
onpointerover: (...)
onpointerrawupdate: (...)
onpointerup: (...)
onprogress: (...)
onratechange: (...)
onreadystatechange: (...)
onreset: (...)
onresize: (...)
onresume: (...)
onscroll: (...)
onsearch: (...)
onsecuritypolicyviolation: (...)
onseeked: (...)
onseeking: (...)
onselect: (...)
onselectionchange: (...)
onselectstart: (...)
onstalled: (...)
onsubmit: (...)
onsuspend: (...)
ontimeupdate: (...)
ontoggle: (...)
ontransitionend: (...)
onvisibilitychange: (...)
onvolumechange: (...)
onwaiting: (...)
onwebkitanimationend: (...)
onwebkitanimationiteration: (...)
onwebkitanimationstart: (...)
onwebkitfullscreenchange: (...)
onwebkitfullscreenerror: (...)
onwebkittransitionend: (...)
onwheel: (...)
ownerDocument: (...)
parentElement: (...)
parentNode: (...)
pictureInPictureElement: (...)
pictureInPictureEnabled: (...)
plugins: (...)
pointerLockElement: (...)
previousSibling: (...)
readyState: (...)
referrer: (...)
rootElement: (...)
scripts: (...)
scrollingElement: (...)
styleSheets: (...)
textContent: (...)
title: (...)
visibilityState: (...)
vlinkColor: (...)
wasDiscarded: (...)
webkitCurrentFullScreenElement: (...)
webkitFullscreenElement: (...)
webkitFullscreenEnabled: (...)
webkitHidden: (...)
webkitIsFullScreen: (...)
webkitVisibilityState: (...)
xmlEncoding: (...)
xmlStandalone: (...)
xmlVersion: (...)
```

# Element类

```js
after: ƒ after()
animate: ƒ animate()
append: ƒ append()
ariaAtomic: (...)
ariaAutoComplete: (...)
ariaBusy: (...)
ariaChecked: (...)
ariaColCount: (...)
ariaColIndex: (...)
ariaColSpan: (...)
ariaCurrent: (...)
ariaDescription: (...)
ariaDisabled: (...)
ariaExpanded: (...)
ariaHasPopup: (...)
ariaHidden: (...)
ariaKeyShortcuts: (...)
ariaLabel: (...)
ariaLevel: (...)
ariaLive: (...)
ariaModal: (...)
ariaMultiLine: (...)
ariaMultiSelectable: (...)
ariaOrientation: (...)
ariaPlaceholder: (...)
ariaPosInSet: (...)
ariaPressed: (...)
ariaReadOnly: (...)
ariaRelevant: (...)
ariaRequired: (...)
ariaRoleDescription: (...)
ariaRowCount: (...)
ariaRowIndex: (...)
ariaRowSpan: (...)
ariaSelected: (...)
ariaSort: (...)
ariaValueMax: (...)
ariaValueMin: (...)
ariaValueNow: (...)
ariaValueText: (...)
assignedSlot: (...)
attachShadow: ƒ attachShadow()
attributeStyleMap: (...)
attributes: (...)
baseURI: (...)
before: ƒ before()
childElementCount: (...) 
childNodes: (...)
children: (...) //重要
classList: (...) //重要
className: (...) //重要
clientHeight: (...)
clientLeft: (...)
clientTop: (...)
clientWidth: (...)
closest: ƒ closest()
computedStyleMap: ƒ computedStyleMap()
elementTiming: (...)
firstChild: (...)
firstElementChild: (...) //重要
getAttribute: ƒ getAttribute() //重要
getAttributeNS: ƒ getAttributeNS()
getAttributeNames: ƒ getAttributeNames()
getAttributeNode: ƒ getAttributeNode()
getAttributeNodeNS: ƒ getAttributeNodeNS()
getBoundingClientRect: ƒ getBoundingClientRect()
getClientRects: ƒ getClientRects()
getElementsByClassName: ƒ getElementsByClassName()
getElementsByTagName: ƒ getElementsByTagName()
getElementsByTagNameNS: ƒ getElementsByTagNameNS()
hasAttribute: ƒ hasAttribute() //重要
hasAttributeNS: ƒ hasAttributeNS()
hasAttributes: ƒ hasAttributes()
hasPointerCapture: ƒ hasPointerCapture()
id: (...)
innerHTML: (...) //重要
insertAdjacentElement: ƒ insertAdjacentElement()
insertAdjacentHTML: ƒ insertAdjacentHTML()
insertAdjacentText: ƒ insertAdjacentText()
isConnected: (...)
lastChild: (...)
lastElementChild: (...)
localName: (...)
matches: ƒ matches()
namespaceURI: (...)
nextElementSibling: (...)
nextSibling: (...)
nodeName: (...)
nodeType: (...)
nodeValue: (...)
onbeforecopy: (...)
onbeforecut: (...)
onbeforepaste: (...)
onbeforexrselect: (...)
onfullscreenchange: (...)
onfullscreenerror: (...)
onsearch: (...)
onwebkitfullscreenchange: (...)
onwebkitfullscreenerror: (...)
outerHTML: (...)
ownerDocument: (...)
parentElement: (...)
parentNode: (...)
part: (...)
prefix: (...)
prepend: ƒ prepend()
previousElementSibling: (...)
previousSibling: (...)
querySelector: ƒ querySelector()
querySelectorAll: ƒ querySelectorAll()
releasePointerCapture: ƒ releasePointerCapture()
remove: ƒ remove()
removeAttribute: ƒ removeAttribute()
removeAttributeNS: ƒ removeAttributeNS()
removeAttributeNode: ƒ removeAttributeNode()
replaceWith: ƒ replaceWith()
requestFullscreen: ƒ requestFullscreen()
requestPointerLock: ƒ requestPointerLock()
scroll: ƒ scroll()
scrollBy: ƒ scrollBy()
scrollHeight: (...)
scrollIntoView: ƒ scrollIntoView()
scrollIntoViewIfNeeded: ƒ scrollIntoViewIfNeeded()
scrollLeft: (...)
scrollTo: ƒ scrollTo()
scrollTop: (...)
scrollWidth: (...)
setAttribute: ƒ setAttribute()
setAttributeNS: ƒ setAttributeNS()
setAttributeNode: ƒ setAttributeNode()
setAttributeNodeNS: ƒ setAttributeNodeNS()
setPointerCapture: ƒ setPointerCapture()
shadowRoot: (...)
slot: (...)
tagName: (...)
textContent: (...)
toggleAttribute: ƒ toggleAttribute()
webkitMatchesSelector: ƒ webkitMatchesSelector()
webkitRequestFullScreen: ƒ webkitRequestFullScreen()
webkitRequestFullscreen: ƒ webkitRequestFullscreen()
Symbol(Symbol.unscopables): {after: true, append: true,
```

# HTMLElement类

```js
accessKey: (...)
ariaAtomic: (...)
ariaAutoComplete: (...)
ariaBusy: (...)
ariaChecked: (...)
ariaColCount: (...)
ariaColIndex: (...)
ariaColSpan: (...)
ariaCurrent: (...)
ariaDescription: (...)
ariaDisabled: (...)
ariaExpanded: (...)
ariaHasPopup: (...)
ariaHidden: (...)
ariaKeyShortcuts: (...)
ariaLabel: (...)
ariaLevel: (...)
ariaLive: (...)
ariaModal: (...)
ariaMultiLine: (...)
ariaMultiSelectable: (...)
ariaOrientation: (...)
ariaPlaceholder: (...)
ariaPosInSet: (...)
ariaPressed: (...)
ariaReadOnly: (...)
ariaRelevant: (...)
ariaRequired: (...)
ariaRoleDescription: (...)
ariaRowCount: (...)
ariaRowIndex: (...)
ariaRowSpan: (...)
ariaSelected: (...)
ariaSort: (...)
ariaValueMax: (...)
ariaValueMin: (...)
ariaValueNow: (...)
ariaValueText: (...)
assignedSlot: (...)
attachInternals: ƒ attachInternals()
attributeStyleMap: (...)
attributes: (...)
autocapitalize: (...)
autofocus: (...)
baseURI: (...)
blur: ƒ blur()
childElementCount: (...)
childNodes: (...)
children: (...)
classList: (...)
className: (...)
click: ƒ click()
clientHeight: (...)
clientLeft: (...)
clientTop: (...)
clientWidth: (...)
contentEditable: (...)
dataset: (...)
dir: (...)
draggable: (...)
elementTiming: (...)
enterKeyHint: (...)
firstChild: (...)
firstElementChild: (...)
focus: ƒ focus()
hidden: (...)
id: (...)
innerHTML: (...)
innerText: (...)
inputMode: (...)
isConnected: (...)
isContentEditable: (...)
lang: (...)
lastChild: (...)
lastElementChild: (...)
localName: (...)
namespaceURI: (...)
nextElementSibling: (...)
nextSibling: (...)
nodeName: (...)
nodeType: (...)
nodeValue: (...)
nonce: (...)
offsetHeight: (...)
offsetLeft: (...)
offsetParent: (...)
offsetTop: (...)
offsetWidth: (...)
onabort: (...)
onanimationend: (...)
onanimationiteration: (...)
onanimationstart: (...)
onauxclick: (...)
onbeforecopy: (...)
onbeforecut: (...)
onbeforepaste: (...)
onbeforexrselect: (...)
onblur: (...)
oncancel: (...)
oncanplay: (...)
oncanplaythrough: (...)
onchange: (...)
onclick: (...)
onclose: (...)
oncontextmenu: (...)
oncopy: (...)
oncuechange: (...)
oncut: (...)
ondblclick: (...)
ondrag: (...)
ondragend: (...)
ondragenter: (...)
ondragleave: (...)
ondragover: (...)
ondragstart: (...)
ondrop: (...)
ondurationchange: (...)
onemptied: (...)
onended: (...)
onerror: (...)
onfocus: (...)
onformdata: (...)
onfullscreenchange: (...)
onfullscreenerror: (...)
ongotpointercapture: (...)
oninput: (...)
oninvalid: (...)
onkeydown: (...)
onkeypress: (...)
onkeyup: (...)
onload: (...)
onloadeddata: (...)
onloadedmetadata: (...)
onloadstart: (...)
onlostpointercapture: (...)
onmousedown: (...)
onmouseenter: (...)
onmouseleave: (...)
onmousemove: (...)
onmouseout: (...)
onmouseover: (...)
onmouseup: (...)
onmousewheel: (...)
onpaste: (...)
onpause: (...)
onplay: (...)
onplaying: (...)
onpointercancel: (...)
onpointerdown: (...)
onpointerenter: (...)
onpointerleave: (...)
onpointermove: (...)
onpointerout: (...)
onpointerover: (...)
onpointerrawupdate: (...)
onpointerup: (...)
onprogress: (...)
onratechange: (...)
onreset: (...)
onresize: (...)
onscroll: (...)
onsearch: (...)
onseeked: (...)
onseeking: (...)
onselect: (...)
onselectionchange: (...)
onselectstart: (...)
onstalled: (...)
onsubmit: (...)
onsuspend: (...)
ontimeupdate: (...)
ontoggle: (...)
ontransitionend: (...)
onvolumechange: (...)
onwaiting: (...)
onwebkitanimationend: (...)
onwebkitanimationiteration: (...)
onwebkitanimationstart: (...)
onwebkitfullscreenchange: (...)
onwebkitfullscreenerror: (...)
onwebkittransitionend: (...)
onwheel: (...)
outerHTML: (...)
outerText: (...)
ownerDocument: (...)
parentElement: (...)
parentNode: (...)
part: (...)
prefix: (...)
previousElementSibling: (...)
previousSibling: (...)
scrollHeight: (...)
scrollLeft: (...)
scrollTop: (...)
scrollWidth: (...)
shadowRoot: (...)
slot: (...)
spellcheck: (...)
style: (...)
tabIndex: (...)
tagName: (...) //重要
textContent: (...)
title: (...)
translate: (...)
```



# HTMLDivElement类

```js
accessKey: (...)
align: (...)
ariaAtomic: (...)
ariaAutoComplete: (...)
ariaBusy: (...)
ariaChecked: (...)
ariaColCount: (...)
ariaColIndex: (...)
ariaColSpan: (...)
ariaCurrent: (...)
ariaDescription: (...)
ariaDisabled: (...)
ariaExpanded: (...)
ariaHasPopup: (...)
ariaHidden: (...)
ariaKeyShortcuts: (...)
ariaLabel: (...)
ariaLevel: (...)
ariaLive: (...)
ariaModal: (...)
ariaMultiLine: (...)
ariaMultiSelectable: (...)
ariaOrientation: (...)
ariaPlaceholder: (...)
ariaPosInSet: (...)
ariaPressed: (...)
ariaReadOnly: (...)
ariaRelevant: (...)
ariaRequired: (...)
ariaRoleDescription: (...)
ariaRowCount: (...)
ariaRowIndex: (...)
ariaRowSpan: (...)
ariaSelected: (...)
ariaSort: (...)
ariaValueMax: (...)
ariaValueMin: (...)
ariaValueNow: (...)
ariaValueText: (...)
assignedSlot: (...)
attributeStyleMap: (...)
attributes: (...)
autocapitalize: (...)
autofocus: (...)
baseURI: (...)
childElementCount: (...)
childNodes: (...)
children: (...)
classList: (...)
className: (...)
clientHeight: (...)
clientLeft: (...)
clientTop: (...)
clientWidth: (...)
contentEditable: (...)
dataset: (...)
dir: (...)
draggable: (...)
elementTiming: (...)
enterKeyHint: (...)
firstChild: (...)
firstElementChild: (...)
hidden: (...)
id: (...)
innerHTML: (...)
innerText: (...)
inputMode: (...)
isConnected: (...)
isContentEditable: (...)
lang: (...)
lastChild: (...)
lastElementChild: (...)
localName: (...)
namespaceURI: (...)
nextElementSibling: (...)
nextSibling: (...)
nodeName: (...)
nodeType: (...)
nodeValue: (...)
nonce: (...)
offsetHeight: (...)
offsetLeft: (...)
offsetParent: (...)
offsetTop: (...)
offsetWidth: (...)
onabort: (...)
onanimationend: (...)
onanimationiteration: (...)
onanimationstart: (...)
onauxclick: (...)
onbeforecopy: (...)
onbeforecut: (...)
onbeforepaste: (...)
onbeforexrselect: (...)
onblur: (...)
oncancel: (...)
oncanplay: (...)
oncanplaythrough: (...)
onchange: (...)
onclick: (...)
onclose: (...)
oncontextmenu: (...)
oncopy: (...)
oncuechange: (...)
oncut: (...)
ondblclick: (...)
ondrag: (...)
ondragend: (...)
ondragenter: (...)
ondragleave: (...)
ondragover: (...)
ondragstart: (...)
ondrop: (...)
ondurationchange: (...)
onemptied: (...)
onended: (...)
onerror: (...)
onfocus: (...)
onformdata: (...)
onfullscreenchange: (...)
onfullscreenerror: (...)
ongotpointercapture: (...)
oninput: (...)
oninvalid: (...)
onkeydown: (...)
onkeypress: (...)
onkeyup: (...)
onload: (...)
onloadeddata: (...)
onloadedmetadata: (...)
onloadstart: (...)
onlostpointercapture: (...)
onmousedown: (...)
onmouseenter: (...)
onmouseleave: (...)
onmousemove: (...)
onmouseout: (...)
onmouseover: (...)
onmouseup: (...)
onmousewheel: (...)
onpaste: (...)
onpause: (...)
onplay: (...)
onplaying: (...)
onpointercancel: (...)
onpointerdown: (...)
onpointerenter: (...)
onpointerleave: (...)
onpointermove: (...)
onpointerout: (...)
onpointerover: (...)
onpointerrawupdate: (...)
onpointerup: (...)
onprogress: (...)
onratechange: (...)
onreset: (...)
onresize: (...)
onscroll: (...)
onsearch: (...)
onseeked: (...)
onseeking: (...)
onselect: (...)
onselectionchange: (...)
onselectstart: (...)
onstalled: (...)
onsubmit: (...)
onsuspend: (...)
ontimeupdate: (...)
ontoggle: (...)
ontransitionend: (...)
onvolumechange: (...)
onwaiting: (...)
onwebkitanimationend: (...)
onwebkitanimationiteration: (...)
onwebkitanimationstart: (...)
onwebkitfullscreenchange: (...)
onwebkitfullscreenerror: (...)
onwebkittransitionend: (...)
onwheel: (...)
outerHTML: (...)
outerText: (...)
ownerDocument: (...)
parentElement: (...)
parentNode: (...)
part: (...)
prefix: (...)
previousElementSibling: (...)
previousSibling: (...)
scrollHeight: (...)
scrollLeft: (...)
scrollTop: (...)
scrollWidth: (...)
shadowRoot: (...)
slot: (...)
spellcheck: (...)
style: (...)
tabIndex: (...)
tagName: //标签名
textContent: (...)
title: (...)
translate: (...)
```

# 二、document对象 ++ 由HTMLDocument类构造产生

## 2.1 使用元数据

* 该部分会向用户提供关于文档的信息（书536页）

| 属性         | 说明                                          | 返回值                |
| ------------ | --------------------------------------------- | --------------------- |
| characterSet | 返回文档的字符集编码[只读]                    | 字符串                |
| charset      | 获取或设置文档的字符集编码                    | 字符串                |
| cookie       | 设置或者获取当前文档的cookie                  | 字符串                |
| domain       | 获取或者设置当前文档的域名                    | 字符串                |
| location     | 获取当前页面的url                             | Location对象          |
| readyState   | 返回当前文档的加载状态[只读]                  | 字符串 比如'complete' |
| referer      | 返回链接到当前文档的文档URL，就是HTTP标头的值 | 字符串                |
| title        | 设置或者返回当前文档的标题                    |                       |
|              |                                               |                       |

### 2.1.1 Location对象（提供更粒度的文档地址信息）

| 属性            | 说明                              | 返回                                         |
| --------------- | --------------------------------- | -------------------------------------------- |
| protocol        | 获取或者设置当前文档的url协议部分 | 字符串 如https:                              |
| host            | 返回主机和端口                    | 字符串 如space.bilibili.com                  |
| href            | 返回当前文档的url                 | 字符串 如https://space.bilibili.com/43536/#/ |
| hostname        | 返回主机名                        | 字符串 如space.bilibili.com                  |
| port            | 返回端口号                        | 字符串 如8080                                |
| pathname        | 返回当前文档的路径部分            | 字符串 如/43536/                             |
| hash            | 返回当前文档的锚部分              | 字符串 如\#/                                 |
| assign(url)     | 导航到指定的url                   | 无 这是一个函数                              |
| reload()        | 重新加载文档                      | 无 这是一个函数                              |
| resolveURL(url) | 将相对URL解析成绝对URL            | 字符串 这是一个函数                          |

### 2.1.2 document.readyState

| 值          | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| loading     | 浏览器正在加载和处理此文档                                   |
| interactive | 文档已经被解析，当时浏览器还在加载其中的链接资源(图片，媒体资源等) |
| complete    | 文档已经被解析，所有资源加载完毕                             |

### 2.1.3 cookie 一段键值对组成的字符串，用分号链接

```
一个例子(B站)
"_uuid=99FB959C-EBF1-B4C3-8E49-5BD7936D590E68840infoc; buvid3=9CFB548E-5897-4EC1-AFEC-636FBBA6D1B4190981infoc; LIVE_BUVID=AUTO5315740750708152; sid=j11dhm1q; CURRENT_FNVAL=16; stardustvideo=1; rpdid=|(J~JYku||~k0J'ul~R|~lR|l; CURRENT_QUALITY=80; laboratory=1-1; DedeUserID=10470086; DedeUserID__ckMd5=b9f8c7577696bd49; bili_jct=35edc9517ad4c4f603916419298c9f9a; bp_article_offset_10470086=402173573598380284; Hm_lvt_8a6e55dbd2870f0f5bc9194cddf32a02=1594031918; bp_t_offset_10470086=409964107399185546; bp_video_offset_10470086=410611204355465826; PVID=1"

这里只显示name和value属性
```

| 属性     | 说明                                                 |
| -------- | ---------------------------------------------------- |
| name     | 名称                                                 |
| value    | 值                                                   |
| path     | 当前cookie所在的文档的路径 比如/                     |
| domain   | 当前cookie所在的域名 比如 .bilibili.com              |
| max-age  | 有效期，以秒从创建的时候算                           |
| expire   | 有效期，绝对时间以GMT格式 如2020-11-13T04:57:22.702Z |
| secure   | 是否只有在HTTPS链接的时候才发送cookie                |
| HttpOnly | 是否只能用于HTTP请求，不能用js操作                   |

## 2.2 获取元素 ++

### 2.2.1 使用document属性获取元素

```js
document.activeElement
```

| 属性           | 说明                                        | 返回           |
| -------------- | ------------------------------------------- | -------------- |
| activeElement  | 返回一个代表当前带有键盘焦点元素的对象      | HTMLElement    |
| body           | 返回body元素                                | HTMLElement    |
| embeds plugins | 返回所有的embed元素                         | HTMLCollection |
| forms          | 返回所有的form元素                          | HTMLCollection |
| head           | 返回head元素                                | HTMLElement    |
| images         | 返回所有的img元素                           | HTMLCollection |
| links          | 返回文档中所有包含href属性的a元素和area元素 | HTMLCollection |
| scripts        | 返回所有script元素                          | HTMLCollection |

### 2.2.2 使用document上的方法获取元素

| 方法                     | 说明                      | 返回           | 能否在HTMLElement上使用 |
| ------------------------ | ------------------------- | -------------- | ----------------------- |
| getElementById()         | 以元素Id属性获取          | HTMLElement    | 否                      |
| getElementsByClassName() | 以元素class属性获取       | HTMLCollection | 是                      |
| getElementsByName()      | 以元素name属性获取        | HTMLCollection | 是                      |
| getElementsTagName()     | 以元素标签名获取          | HTMLCollection | 是                      |
| querySelector()          | 以CSS选择器获取第一个元素 | HTMLElement    | 是                      |
| querySelectorAll()       | 以CSS选择器获取           | HTMLCollection | 是                      |

### 2.2.3 层级关系

* 这些属性或者方法 所有的元素都有

| 属性 方法              | 说明                            | 返回           |
| ---------------------- | ------------------------------- | -------------- |
| childNodes             | 返回子节点组                    | HTMLCollection |
| children               | 放回子元素组(所有nodeType为1的) | HTMLCollection |
| firstChild             | 返回第一个子节点                | HTMLElement    |
| firstElementChild      |                                 | HTMLElement    |
| hasChildNodes()        | 有子元素就返回true              | 布尔值         |
| lastChild              | 返回最后一个子节点              | HTMLElement    |
| lastElementChild       |                                 | HTMLElement    |
| nextSibling            | 返回当前元素之后的兄弟节点      | HTMLElement    |
| nextElementSibling     |                                 | HTMLElement    |
| parentNode             | 返回父节点                      | HTMLElement    |
| previousSibling        | 返回当前元素之前的兄弟节点      | HTMLElement    |
| previousElementSibling |                                 | HTMLElement    |

# 三、节点类型

<img src="../../../AppData/Roaming/Typora/typora-user-images/image-20200711160911930.png" alt="image-20200711160911930" style="zoom:80%;" />

| nodeType | 说明     | 举例          |
| -------- | -------- | ------------- |
| 1        | 元素节点 | <div></div>   |
| 2        | 属性节点 | 上图中的红色  |
| 3        | 文本节点 | 上图中的蓝色  |
| 8        | 注释节点 | <!-- 注释 --> |

# 四、HTMLElement类的常用方法和属性++

## 4.1 常用属性

| 属性                     | 说明                             | 返回                                                         |
| ------------------------ | -------------------------------- | ------------------------------------------------------------ |
| checked                  | 获取或设置checked属性是否存在    | 布尔值                                                       |
| classList                | 获取或者设置元素的类列表         | DOMTokenList                                                 |
| className++              | 获取或设置元素的class            | 字符串                                                       |
| dir                      | 获取或设置dir属性                | 字符串                                                       |
| disabled                 | 获取或设置disabled属性是否存在   | 布尔值                                                       |
| hidden                   | 获取或设置hidden属性是否存在     | 布尔值                                                       |
| id                       |                                  | 字符串                                                       |
| lang                     | 获取或设置lang属性               |                                                              |
| spellcheck               | 获取或设置spellcheck属性是否存在 | 字符串                                                       |
| tabIndex                 | 获取或设置tabIndex属性           | 数字                                                         |
| tagName++                | 返回标签名（大写）               | 字符串                                                       |
| title                    | 获取或设置title属性              | 字符串                                                       |
| attributes               | 返回应用到元素上的属性           | DOMNodeMap 如{0: id, 1: class, id: id, class: class, length: 2} |
| dataset++                | 返回以data-开头的属性            | DOMStringMap 如{fruit: "apple", orange: "orange"}            |
| getAttribute()           | 返回指定属性的值 [只读]          | 字符串                                                       |
| hasAttribute()           | 检查是否带有指定的属性           | 布尔值                                                       |
| removeAttribute()        | 从元素上移除指定的属性           | 无                                                           |
| setAttribute('id','box') | 设置一个属性和相应的值           | 无                                                           |

```jsx
<div data-apple='apple' data-orange='orange' id='box'></div>

let div = document.getElementById('box')
div.dataset // {fruit: "apple", orange: "orange"}
```

* DOMTokenList类

| 属性或者方法 | 说明                                   | 返回   |
| ------------ | -------------------------------------- | ------ |
| add()        | 给元素添加一个类                       | 无     |
| contains()   | 检查元素是否包含特定类                 | 布尔值 |
| length       | 返回元素上面类的数量                   | 数字   |
| remove       | 从元素上移除指定的类                   | 无     |
| toggle()     | 如果元素上存在这个类就删除，反之就添加 | 布尔值 |

```js
let div = document.getElementById('box')
div.classList.add('container')
```

## 4.2 元素的增删改

![image-20200711212813409](../../../AppData/Roaming/Typora/typora-user-images/image-20200711212813409.png)

| 属性 方法                             | 说明                                          | 返回                                          |
| ------------------------------------- | --------------------------------------------- | --------------------------------------------- |
| appendChild(HTMLElement)              | 在元素的末尾添加一个子元素                    | HTMLElement                                   |
| cloneNode(HTMLElement)                | 复制一个元素                                  | HTMLElement                                   |
| compareDocumentPosition(HTMLElement)  | 判断一个元素相对于另一个元素的位置(dom树上的) | 数字                                          |
| innerHTML                             | 获取或者设置元素的内容(包含html标签)          | 字符串 如"主页"                               |
| innerText                             | 获取或者设置元素的内容(不包含html标签)        | 字符串                                        |
| insertBefore(newElem,childElem)       | 在第二个元素之前插入第一个元素                | HTMLElement                                   |
| isEqualNode(HTMLElement)              | 判断指定元素是否与当前元素内容一样            | 布尔值                                        |
| isSameNode(HTMLElement)               | 判断是否是同一个节点                          | 布尔值                                        |
| outerHTML                             | 获取或设置某个元素的HTML和内容                | 字符串 如"\<span class="n-text">主页\</span>" |
| removeChild(HTMLElement)              | 删除指定的元素                                | HTMLElement                                   |
| replaceChild(HTMLElement,HTMLElement) | 以第二个元素替换第一个元素                    | HTMLElement                                   |

# 五、元素的专有属性

* 这些属性是元素特用的，不能共享

## 5.1 HTMLInputElement元素

```
1.value属性 
```

# 六、DOM中的各种尺寸、位置+++

## 6.1 整篇文档

![image-20200722153433672](../../../AppData/Roaming/Typora/typora-user-images/image-20200722153433672.png)

## 6.2 某一个元素

![image-20200722153514184](../../../AppData/Roaming/Typora/typora-user-images/image-20200722153514184.png)

## 6.3 鼠标的坐标

![image-20200722153550895](../../../AppData/Roaming/Typora/typora-user-images/image-20200722153550895.png)

# 七、DOM的常用操作

```
javascript 原生方法对dom节点的操作包括：访问（查找）、创建、添加、删除、替换、插入、复制、移动等
```

```js
//查找节点
document.getElementById("id");// 通过id查找，返回唯一的节点
document.getElementsByClassName("class");// 通过class查找，返回值为nodeList类型
document.getElementsByTagName("div");// 通过标签名查找，返回值为nodeList类型

//创建节点
document.createDocumentFragment();//创建内存文档碎片
document.createElement();//创建元素
document.createTextNode();//创建文本节点

//添加节点
var ele = document.getElementById("my_div");
var oldEle = document.createElement("p");
var newEle=document.createElement("div");
ele.appendChild(oldEle);

//删除节点
ele.removeChild(oldEle);

//替换节点
ele.replaceChild(newEle,oldEle);

//插入节点
ele.insertBefore(oldEle,newEle);//在newEle之前插入 oldEle节点

//复制节点
var cEle = oldEle.cloneNode(true);//深度复制，复制节点下面所有的子节点
cEle = oldEle.cloneNode(false);//只复制当前节点，不复制子节点

//移动节点
var cloneEle = oldEle.cloneNode(true);//被移动的节点
document.removeChild(oldEle);//删除原节点
document.insertBefore(cloneEle,newEle);//插入到目标节点之前
```

