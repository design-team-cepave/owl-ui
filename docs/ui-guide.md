## 檔名/模組化命名方式

- 檔案與目錄名稱需全部採用`小寫`，可以降低 Mac 上 require `不分大小寫`問題的風險。

- 模塊裡的單字區分需用連字符 `-` 區隔開來，目錄亦同。

```js
// bad
toggleCss.js

// good
toggle-css.js
```

### 測試用例檔名
- 歸放在各自組件內 `__test__` 目錄下

- 可取名為 `module-name.spec.js`

  ​
> 為什麼不是 `module-name-spec.js`？因為會讓人誤認為成有一個叫 `module-name-spec` 的 module 而不是一個 `module-name` 的 `spec`


## React Component

- 保持與其他一致，用 `.js` 來命名
- 目錄與檔名也與`檔名/模組化命名方式`一致

為什麼不採用 airbnb 推薦的 `.jsx` ？雖然 `.jsx` 比較好區分是否為 React Component 或一般模塊，並且 webpack 也可以設定讓 `.jsx` 變為預設，但因為快網產品有跑 **server-side rendering**，`index.jsx`下 Node 端無法變成預設載入。

```js
// bad
import Footer from './footer/footer.jsx';

// bad
import Footer from './footer/index.jsx';

// good
import Footer from './footer';
```

### Compoent 創建方式, Class vs React.createClass vs stateless
目前有三種方式來創建 React Component

### Class vs React.createClass
```javascript
// bad
const Component = React.createClass({
  // ...
  render() {
    return <div>{this.state.hello}</div>;
  }
});

// good
class Component extends React.Component {
  // ...
  render() {
    return <div>{this.state.hello}</div>;
  }
}
```

### stateless
如果你的組件沒有任何 state，請採用這種方式，會讓渲染效率變高

```javascript
Component.propTypes = {}
Component.defaultProps = {}

function Component({...props}) {
  return (
    <div {...props}>

    </div>
  )
}
```
### API Props 接口設計原則

####  props.children

優先思考是否可用 `children` 達成，因為這是我們寫 HTML 最本能的用法

```javascript
// bad
render() {
  <div>
    {this.props.text}
  </div>
}

// good
render() {
  <div>
    {this.props.children}
  </div>
}
```



#### [Boolean]

採用 `is` 或 `has` 前綴

```javascript
// bad
static defaultProps = {
  disabled: true
}

// good
static defaultProps = {
  isDisabled: true
}
```

####  [Array/Counts/Number] 

盡量採用複數名詞

```javascript
// bad
static defaultProps = {
  count: 20,
  entry: []
}

// good
static defaultProps = {
  counts: 20,
  entries: []
}
```

####  [Function] 

通常為**做什麼事**， 接口上應該有`動詞`

```javascript
// bad
static defaultProps = {
  something(){}
}

// good
static defaultProps = {
  doSomething(){}
}
```

在做一件事之前或之後，可用 `before` / `after`

```javascript
static defaultProps = {
  afterClick(){}
  beforeClick(){}
}
```




## ES2015 import/export 與 require 的抉擇
### import 引入模塊

優先採用 ES2015 的 `import` 方式引入模塊，
與 `Object Mapping` 的宣告方式

```javascript
// bad
const React = require('react')
const {PropTypes, Component} = React

// good
import React, {PropTypes, Component} from 'react'
```

如遇到限制與特殊情況時可直接採用 `require`

-  只被引用一次，或不需要任何宣告存入變數的情況

```javascript
// good but a little extra
import colors from 'colors'
import moduleFunc from './module-func'
moduleFunc({})

// good
require('colors')
require('./module-func')({})
```

- 有時候我們不希望最上方就加載進來，需透過一些條件加載或是運行在函數內

```javascript
if (condition) {
  const moduleName = require('module-name')
}

func(){
  const moduleName = require('module-name')
}
```

### export 輸出模塊
一律採用 `module.exports` 或 `exports.xxx` 的方式。


