



### Props的解构传值

定义了接口以后可以用解构传值

interface DelButtonProps {

​    /** 按钮类型*/

​    type?: 'actionButton' | 'text' | 'outlined' | 'contained';

​    /** 根据某一字段值来确认是否禁用删除按钮*/

​    operator?: DelButtonOperator;

​    /** 当前项数据 */

​    item?: any;

​    /** 删除*/

​    onDelItem: (id?: string) => void;

​    /** 弹框中的删除的文案*/

​    delText?: string | JSX.Element;

​    /** 删除按钮文案*/

​    delBtnText?: string | JSX.Element;

​    /** 是否禁用*/

​    disabled?: boolean;

​    /** 样式*/

​    sx?: any;

}



​    const {

​        type = 'actionButton',

​        operator,

​        item,

​        onDelItem,

​        delText,

​        delBtnText,

​        ...others

​    } = props;

### 数据请求

![image-20230629110556506](./react笔记.assets/image-20230629110556506.png)

​    // 组件封装好了方法，可以用下面这段代替上面定义的三个数据请求的语句

### 路由

![image-20230629013318007](./react笔记.assets/image-20230629013318007.png)

使用Switch包裹以后每次只会匹配渲染一个页面，可以解决页面叠加的问题

当前面所有的页面都不匹配的时候会自动匹配最后一个页面

![image-20230629013610903](./react笔记.assets/image-20230629013610903.png)

在route中使用component传递组件的时候，react-router会默认传递：history、location、match这三个数据

在相应的组件文件里打印相应组件的props可以获取到这些数据

![image-20230629014111809](./react笔记.assets/image-20230629014111809.png)

match的结果并不总是与URL一致，

match中可以查看isexact来判断路径与URL是否完全匹配

match中的params可以读取URL中的参数

![image-20230629014332650](./react笔记.assets/image-20230629014332650.png)



![image-20230629014402113](./react笔记.assets/image-20230629014402113.png)

获取参数：props.match.params

![image-20230629014614614](./react笔记.assets/image-20230629014614614.png)

需要给组件传递泛型，否则props.match.params中match会报错

![image-20230629014741361](./react笔记.assets/image-20230629014741361.png)

根据需要，还要再说明props的类型

![image-20230629015515477](./react笔记.assets/image-20230629015515477.png)

使用URL传递参数需要完全匹配（URL与）路径 

继承了以后可以再进行增加

![image-20230629020448008](./react笔记.assets/image-20230629020448008.png)



![image-20230629020755038](./react笔记.assets/image-20230629020755038.png)

push的参数就是接下来要进入的页面URL

useHistory；useLocation; useParams; useRouteMatch

![image-20230629021216155](./react笔记.assets/image-20230629021216155.png)

link组件优势：1节省代码量，不用手动操作导航栈；2支持右键操作（新页面中打开），可以改善用户体验

![image-20230629021525718](./react笔记.assets/image-20230629021525718.png)

![image-20230629021825140](./react笔记.assets/image-20230629021825140.png)

### redux










### 新建组件并传参调用



### ![image-20230628140723575](./react笔记.assets/image-20230628140723575.png)



如上

### 状态管理方法

下面是使用React状态管理方法的一些示例：

1. 使用组件的state属性。

jsx

Copy

```
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={handleClick}>Increment</button>
    </div>
  );
}
```

在这个例子中，Counter组件使用useState()钩子来创建一个名为"count"的状态变量，并将其初始值设置为0。每次用户单击按钮时，handleClick()函数会调用setCount()方法来更新计数器的值。

1. 使用Redux。

jsx

Copy

```
import { createStore } from 'redux';
import { Provider, connect } from 'react-redux';

// 定义reducer函数
function counterReducer(state = { count: 0 }, action) {
  switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 };
    case 'DECREMENT':
      return { count: state.count - 1 };
    default:
      return state;
  }
}

// 创建store对象
const store = createStore(counterReducer);

// 定义Counter组件
function Counter(props) {
  function handleIncrement() {
    props.dispatch({ type: 'INCREMENT' });
  }

  function handleDecrement() {
    props.dispatch({ type: 'DECREMENT' });
  }

  return (
    <div>
      <p>Count: {props.count}</p>
      <button onClick={handleIncrement}>Increment</button>
      <button onClick={handleDecrement}>Decrement</button>
    </div>
  );
}

// 将Counter组件连接到store
const ConnectedCounter = connect(state => ({ count: state.count }))(Counter);

// 渲染应用程序
ReactDOM.render(
  <Provider store={store}>
    <ConnectedCounter />
  </Provider>,
  document.getElementById('root')
);
```

在这个例子中，使用Redux创建一个名为"counterReducer"的reducer函数，它用于处理计数器的状态变化。创建一个名为"store"的store对象来管理状态。使用connect()函数将Counter组件连接到store。在Counter组件中，使用dispatch()方法来发送一个名为"INCREMENT"或"DECREMENT"的操作，以更新计数器的值。

1. 使用上下文。

jsx

Copy

```
import React, { createContext, useContext, useState } from 'react';

// 创建上下文对象
const CountContext = createContext();

// 创建Counter组件
function Counter() {
  const [count, setCount] = useState(0);

  function handleIncrement() {
    setCount(count + 1);
  }

  function handleDecrement() {
    setCount(count - 1);
  }

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={handleIncrement}>Increment</button>
      <button onClick={handleDecrement}>Decrement</button>
    </div>
  );
}

// 创建App组件
function App() {
  return (
    <CountContext.Provider value={0}>
      <Counter />
    </CountContext.Provider>
  );
}

// 在Counter组件中使用上下文
function Counter() {
  const count = useContext(CountContext);

  function handleIncrement() {
    // ...
  }

  function handleDecrement() {
    // ...
  }

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={handleIncrement}>Increment</button>
      <button onClick={handleDecrement}>Decrement</button>
    </div>
  );
}

// 渲染应用程序
ReactDOM.render(<App />, document.getElementById('root'));
```

在这个例子中，使用createContext()函数创建一个名为"CountContext"的上下文对象。在App组件中，使用CountContext.Provider组件来提供上下文数据。在Counter组件中，使用useContext()钩子来访问上下文数据。

总之，React的状态管理可以使用组件的state属性、Redux、上下文等方式来实现。选择适当的状态管理方案取决于应用程序的规模和复杂度。

### react知识点

React 是一个流行的 JavaScript 库，用于构建用户界面。以下是 React 的一些重要知识点：

1. JSX：JSX 是一种允许你在 JavaScript 中编写类似 HTML 的语法的语言扩展。它使得编写 React 组件变得更加简单和直观。
2. 组件：React 中的组件是可重用的代码块，用于构建用户界面。组件可以是函数组件或类组件，它们接受输入属性 `props`，并返回用于渲染界面的 JSX 元素。
3. 状态：React 中的状态是组件内部管理的数据。通过使用 `useState`钩子函数，组件可以在运行时修改它们的状态，从而更新界面。
4. 生命周期：React 组件具有生命周期方法，这些方法在组件的不同阶段被调用。例如，`componentDidMount` 方法在组件挂载后调用，`componentWillUnmount` 方法在组件卸载前调用。
5. 渲染：React 使用虚拟 DOM（Virtual DOM）来管理界面更新。当组件的状态发生变化时，React 会重新计算虚拟 DOM 中的差异，并更新实际 DOM 中的部分内容，从而提高性能。
6. 路由：React 应用程序通常使用第三方库（例如 React Router）来实现客户端路由。这些库提供了一种将 URL 映射到组件的方式，使得用户可以在不刷新页面的情况下导航到不同的页面。
7. 状态管理：当 React 应用程序变得复杂时，通常需要使用状态管理库（例如 Redux）来管理全局状态。这些库提供了一种将组件之间共享的状态集中管理的方式。
8. 高阶组件：高阶组件是一个函数，它接收一个组件作为参数，并返回一个新的组件。它可以用来在组件之间共享逻辑或增强组件的功能。
9. 钩子函数：React 中的钩子函数（例如 `useState`、`useEffect`、`useContext` 等）是一种用于在函数组件中添加状态和副作用的方式。它们使得编写函数组件变得更加灵活和强大。

以上是 React 的一些重要知识点，掌握它们可以帮助你更好地使用 React 构建用户界面。

### 父组件怎么通过props将数据传递给子组件

父组件可以通过props将数据传递给子组件。在父组件中，可以在子组件的标签中添加属性，并将数据作为属性的值传递给子组件。

例如，如果有一个父组件和一个子组件，父组件要将一个名为"message"的字符串传递给子组件，可以这样做：

jsx

Copy

```
// ParentComponent.js

import React from 'react';
import ChildComponent from './ChildComponent';

function ParentComponent(){
  const message = "Hello, World!";

  return (
    <div>
      <ChildComponent message={message} />
    </div>
  );
}
```

在这个例子中，父组件中定义了一个名为"message"的字符串，并通过子组件的props将其传递给子组件。子组件可以通过this.props.message来访问这个属性。

jsx

Copy

```
// ChildComponent.js

import React from 'react';

function ChildComponent(props){
  return <p>{props.message}</p>;
}
```

在子组件中，可以通过props来访问父组件传递的数据。在这个例子中，子组件会渲染一个p标签，显示父组件传递的"message"字符串。

总之，父组件可以通过props将数据传递给子组件，子组件可以通过props来访问这些数据。这种单向数据流是React中的一个核心概念，使得组件之间的数据传递变得非常简单和可预测。

### 给组件用解析传值

在React中，子组件通过props从父组件接收数据。在子组件中，可以使用解构赋值来方便地使用这些props。

例如，假设有一个父组件和一个子组件，父组件向子组件传递了一个名为"message"的字符串和一个名为"count"的数字，可以这样使用解构赋值来访问这些props：

jsx

Copy

```
// ParentComponent.js

import React from 'react';
import ChildComponent from './ChildComponent';

function ParentComponent(){
  const message = "Hello, World!";
  const count = 10;

  return (
    <div>
      <ChildComponent message={message} count={count} />
    </div>
  );
}
```

在子组件中，可以使用解构赋值来访问这些props。

jsx

Copy

```
// ChildComponent.js

import React from 'react';

function ChildComponent({message, count}){
  return (
    <div>
      <p>{message}</p>
      <p>Count: {count}</p>
    </div>
  );
}
```

在这个例子中，子组件使用了解构赋值来访问父组件传递的"message"和"count"属性。在子组件中，可以直接使用这些属性，不需要通过"this.props"来访问它们。

总之，使用解构赋值可以使子组件中的props更容易使用和阅读，同时还可以减少代码量。



### styled

const titleStyle = {
    fontSize: '14px'
};
<Box sx={titleStyle}>请问您是否确定创建理论模型？</Box>


export const TertiaryBtn = ({ sx, ...props }: ButtonProps) => {
    return (
        <Button
            sx={{
                color: (t) => t.palette.text.primary,
                border: '1px solid #C9CDD4',
                ...sx
            }}
            {...props}
        />
    );
};
const BaseBtn = styled(TertiaryBtn)({
    height: 28,
    minHeight: 28,
    padding: '4px 16px',
    fontWeight: 'normal'
});

mui styled如何使用
MUI（Material-UI）是一种基于React的UI组件库，提供了一组丰富的美观、易于使用的组件和样式。在MUI中，可以使用styled()函数来创建自定义样式的组件。
使用styled()函数创建组件的基本语法如下：
jsx
Copy
import { styled } from '@mui/material/styles';

const MyComponent = styled(ComponentName)({
  // CSS样式
});
其中，ComponentName是要自定义样式的MUI组件名称，{}中的内容是CSS样式规则。通过这种方式，可以在不修改原始组件代码的情况下，轻松地自定义组件的样式。
例如，要创建一个自定义的Button组件，可以按照以下方式使用styled()函数：
jsx
Copy
import { styled } from '@mui/material/styles';
import Button from '@mui/material/Button';

const MyButton = styled(Button)({
  backgroundColor: 'red',
  color: 'white',
  padding: '10px 20px',
  borderRadius: '5px',
  '&:hover': {
    backgroundColor: 'darkred'
  }
});

function App() {
  return (
    <div>
      <Button variant="contained">默认按钮</Button>
      <MyButton variant="contained">自定义按钮</MyButton>
    </div>
  );
}
在上面的代码中，使用styled()函数创建了一个名为MyButton的自定义Button组件，覆盖了原始Button组件的样式。在MyButton的样式中，设置了背景颜色、文本颜色、内边距、边框半径和悬停效果等样式。最后，在App组件中，可以使用MyButton组件来替代原始的Button组件。



### css 知识点

CSS（层叠样式表）是一种用于

描述网页样式和布局的语言。以下是CSS的一些核心知识点：
选择器：选择器用于选择要应用样式的HTML元素。常见的选择器包括标签选择器、类选择器、ID选择器、属性选择器、后代选择器和伪类选择器等。
盒模型：盒模型是指HTML元素在页面中占据的空间，包括内容、内边距、边框和外边距。在CSS中，可以使用box-sizing属性来控制盒模型的大小计算方式。
样式属性：CSS中有许多样式属性，用于控制文本、颜色、背景、布局、动画等方面的样式。常见的样式属性包括color、font、background、margin、padding、display、position和animation等。
媒体查询：媒体查询用于根据设备屏幕大小、分辨率和方向等条件来应用不同的样式。在CSS中，可以使用@media规则来定义媒体查询。
浮动和定位：浮动和定位是CSS中常用的布局技术。浮动可以让元素脱离文档流并与其他元素进行布局，而定位可以让元素相对于其父元素或文档进行定位。
响应式布局：响应式布局是指根据设备屏幕大小和方向等条件来自适应调整页面布局和样式的技术。在CSS中，可以使用媒体查询和弹性布局等技术来实现响应式布局。
CSS预处理器：CSS预处理器是一种可以增强CSS编写体验的工具，例如Sass和Less。它们提供了变量、函数、嵌套和继承等功能，可以帮助开发人员更轻松地编写和维护CSS代码。



### 必须在父元素中添加设置以后才能生效的css属性

有一些CSS属性需要在父元素中设置才能生效，这些属性包括：

1. display属性：如果子元素的display属性值为table-cell、table-row、table-caption、inline-block、flex、inline-flex等，则要求父元素的display属性的值为table、inline-table、table-row-group、table-header-group、table-footer-group、table-row、table-column-group、table-column、flex、inline-flex等。
2. position属性：如果子元素的position属性值为absolute、fixed或sticky，则要求父元素的position属性的值不为static。
3. float属性：如果子元素的float属性值为left或right，则要求父元素的overflow属性的值不为visible。
4. clear属性：如果子元素的clear属性值为left或right，则要求在该子元素前面的所有浮动元素都已被清除。
5. vertical-align属性：如果子元素的vertical-align属性值为baseline、middle、sub、super或text-top，则要求父元素的display属性的值为table-cell。
6. z-index属性：如果子元素的z-index属性值不为auto，则要求父元素的position属性的值不为static。

需要注意的是，并非所有的浏览器都会严格遵守这些规则，因此在使用这些属性时，最好进行充分的测试，以确保在所有浏览器中都能正确地呈现样式。

### assets文件夹的作用

assets 文件夹通常用于存放项目中使用的静态资源，例如图片、字体、样式表、JavaScript文件等。这些资源通常不会被修改，也不会包含任何业务逻辑代码，但是它们是项目运行和呈现所必需的。
在许多前端框架和工具中，assets 文件夹通常位于项目的根目录下，并且会被编译和打包到最终的发布文件中。这些静态资源可以通过相对路径或绝对路径引用，例如在HTML文件中使用 <img> 标签引用图片文件，或在CSS文件中使用 url() 函数引用背景图。
将静态资源存放在 assets 文件夹中可以使项目的目录结构更加清晰，易于维护和管理。同时，由于这些资源通常较大，单独打包和压缩这些资源可以减少最终发布文件的大小，加快项目的加载速度。



### 循环渲染

![image-20230625000758994](./react笔记.assets/image-20230625000758994.png)

react要求循环渲染组件要有唯一id

### 函数式组件

![image-20230625000621294](./react笔记.assets/image-20230625000621294.png)


### 消息传递

![image-20230624155904927](./react笔记.assets/image-20230624155904927.png)

![image-20230624155839897](./react笔记.assets/image-20230624155839897.png)

### hooks是一种处理消息的方法

hooks的一个例子（对比）

类组件：

![image-20230624160448501](./react笔记.assets/image-20230624160448501.png)

函数组件：

![image-20230624160703269](./react笔记.assets/image-20230624160703269.png)

定义一个说明了类型的state

![image-20230624161117778](./react笔记.assets/image-20230624161117778.png)

### useEffect异步请求数据

使用useEffect来调接口拿数据（如果useEffect的第二个参数为空，组件将会在每一次更新以后执行前面的回调函数）

![image-20230624161413098](./react笔记.assets/image-20230624161413098.png)

useEffetc的回调函数不能是异步的

![image-20230624163227229](./react笔记.assets/image-20230624163227229-7595549.png)

这样会的返回值会是一个promise，既不是函数也不是none，所以会报错。

如果要在useEffetc里使用async、await的异步请求，需要把异步函数封装起来

![image-20230624163359830](./react笔记.assets/image-20230624163359830.png)

错误处理

![image-20230624164029932](./react笔记.assets/image-20230624164029932.png)



### async、await的用法

async和await是ES2017（也称为ES8）中引入的用于处理异步操作的关键字。

async关键字被用于定义一个函数，并使其返回一个Promise对象。在该函数中可以使用await关键字来等待异步操作完成，然后获取其结果。以下是async和await的基本用法：

javascript

Copy

```
async function myAsyncFunction() {
  //执行异步操作，如发起网络请求或读取文件
  const result = await myAsyncOperation();
  //在异步操作完成后，在此处使用结果
  console.log(result);
}

//myAsyncFunction返回一个Promise对象，可以使用then()和catch()方法来处理它的结果或错误
myAsyncFunction().then(() => {
  console.log("Async operation completed successfully");
}).catch((error) => {
  console.error(`Async operation failed with error: ${error}`);
});
```

在上面的代码中，myAsyncFunction是一个异步函数，它使用await关键字等待myAsyncOperation异步操作完成，并获取其结果。myAsyncFunction返回一个Promise对象，可以使用then()和catch()方法来处理其结果或错误。

需要注意的是，只有在async函数中才能使用await关键字。await关键字只能用于返回Promise对象的函数，或者是一个返回值为Promise对象的表达式。如果await关键字用于一个非Promise对象或者没有返回值的表达式，那么它将被自动包装为一个解决了的Promise对象。

另外，使用async和await可以让异步代码更易于理解和编写，因为它们使异步代码看起来更像是同步代码。但是，要注意避免使用过多的异步操作，以避免阻塞事件循环并影响应用程序的性能。



### JSX中使用逻辑判断

用{}花括号包裹的内容会被求值，否则不会被视为变量



### typescript

![image-20230624164738931](./react笔记.assets/image-20230624164738931.png)

定义一个符合接口要求的变量

​    const [theoryId, setTheoryId] = useState<number>();

​    const [protoId, setProtoId] = useState<number>();

### 高阶组件（HOC）

高阶组件用with开头（类似钩子以use开头）

![image-20230624165326482](./react笔记.assets/image-20230624165326482.png)

![image-20230624165749630](./react笔记.assets/image-20230624165749630.png)

![image-20230624170102697](./react笔记.assets/image-20230624170102697.png)

![image-20230624170129097](./react笔记.assets/image-20230624170129097.png)



![image-20230624170308010](./react笔记.assets/image-20230624170308010.png)

![image-20230624170335047](./react笔记.assets/image-20230624170335047.png)

![image-20230624170524784](./react笔记.assets/image-20230624170524784.png)

![image-20230624170551115](./react笔记.assets/image-20230624170551115.png)

![image-20230624170643659](./react笔记.assets/image-20230624170643659.png)



### 在onClick中调用一个函数：

onClick={() =>addTocart(id, name )}

![image-20230624170236599](./react笔记.assets/image-20230624170236599.png)



### store(redux)

![image-20230625003420453](./react笔记.assets/image-20230625003420453.png)

### 在URL中添加参数

![image-20230625005846492](./react笔记.assets/image-20230625005846492.png)

获取参数

![image-20230625010202755](./react笔记.assets/image-20230625010202755.png)

![image-20230625010508969](./react笔记.assets/image-20230625010508969.png)

![image-20230625010546186](./react笔记.assets/image-20230625010546186.png)

### redux仓库使用方法

 ![image-20230627172717604](./react笔记.assets/image-20230627172717604.png)



![image-20230627173019737](./react笔记.assets/image-20230627173019737.png)

仓库数据片需要在store/index里面导入，然后再注册

![image-20230627173224618](./react笔记.assets/image-20230627173224618.png)





#  疑问：

layout好像不具备route属性，为什么可以和route一起放在Switch里



