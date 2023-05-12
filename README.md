<p align="center">
 <h1 align="center">Tìm hiểu về React Hook</h1>
</p>

## React Hook là gì?

 React Hook là các hàm JavaScript đơn giản mà chúng ta có thể sử dụng để tách phần có thể tái sử dụng khỏi một thành phần chức năng. Móc có thể có trạng thái và có thể quản lý tác dụng phụ.
 
 React cung cấp một loạt các hook tích hợp tiêu chuẩn:
 
- useState: Để quản lý các trạng thái. Trả về một giá trị có trạng thái và một hàm cập nhật để cập nhật nó.
- useEffect: Để quản lý các tác dụng phụ như lệnh gọi API, đăng ký, bộ hẹn giờ, đột biến, v.v.
- useContext: Để trả về giá trị hiện tại cho một ngữ cảnh.
- useReducer: Một useStategiải pháp thay thế giúp quản lý trạng thái phức tạp.
- useCallback: Nó trả về một phiên bản gọi lại đã ghi nhớ để giúp một thành phần con không kết xuất lại một cách không cần thiết.
- useMemo: Nó trả về một giá trị được ghi nhớ giúp tối ưu hóa hiệu suất.
- useRef: Nó trả về một đối tượng ref với một .currentthuộc tính. Đối tượng ref có thể thay đổi. Nó chủ yếu được sử dụng để truy cập một thành phần con một cách bắt buộc.
- useLayoutEffect: Nó kích hoạt ở cuối tất cả các đột biến DOM. Tốt nhất là sử dụng useEffectcàng nhiều càng tốt cái này khi useLayoutEffectlửa cháy đồng bộ.
- useDebugValue: Giúp hiển thị nhãn trong React DevTools cho các hook tùy chỉnh.

## Basic Hooks?

Trong bài viết này sẽ thảo luận về các React Hook cơ bản: useState, useEffect, và useContext

![image](https://github.com/thangdtph27626/react-hook.github.io/assets/109157942/3c217ad7-e2a7-4003-a1f6-d1c347df30e5)

### 1: useState

Hàm useState trả về 1 mảng 2 phần tử, phần tử đầu tiên là để khởi tạo state, phần tử thứ 2 là hàm để cập nhật state. Tham số truyền vào hàm useState là giá trị khởi tạo của state.

```
import logo from './logo.svg';
import './App.css';
import React, { useState } from 'react';

function Test() {
  const [state, setState] = useState(0);
  const handleChange = () => {
  setState(state + 1)
  }
    return (
         <div>
          {state}
           <button  onClick={handleChange} >state</button>
         </div>
    );
}

function App() {

  return (
    <div className="App">
     <Test ></Test>
    </div>
  );
}

export default App;
```

 + state: Biến state đếm 
 + setState: hàm cập nhật giá trị của state
 + useState(0): khởi tạo giá trị của state bắt đầu từ 0
 
 - Cập nhật biến trạng thái: Mỗi khi state được cập nhật thì Component sẽ re-render (function được chạy lại và giao diện được cập nhật lại theo state). Cần chú ý là không được thay đổi trực tiếp biến state (immutable) mà phải cập nhật thông qua hàm cập nhật state.

```
  const handleChange = () => {
     setState(state + 1) // hàm cập nhật state 
  }
```

### 2: useEffect

useEffect được kích hoạt sau khi quá trình render của React component hoàn tất. Nó sẽ được gọi và thực hiện tính toán các hành động bên trong nó trong một callback.
 
 - Cú pháp khai báo: 
 
```
useEffect(callback, dependencies);
```

- useEffect chấp nhận 2 đối số:
  + Callback: sẽ được gọi trong useEffect sau khi return thực thi nhiệm vụ kết xuất giao diện của nó. 
  + Dependencies: Là một mảng chứa các đối số mà useEffect sẽ phụ thuộc vào đó để thực thi

#### Các dependencies trong các trường hợp:
   
##### 1: Không cung cấp
 - Trong trường hợp bạn không cung cấp bất kỳ đôi số nào. useEffect sẽ được gọi thực thi các tính toán bên trong nó môi khi thành phần render.
   + Thực hiện callback mỗi khi componet mounted 
   + Gọi callback mỗi khi componet re-render 
   + Gọi callback mỗi khi thêm element vào dom 
   
```
   function Test() {
  const [count, setCount] = useState(0)
  useEffect(() => {
		console.log("hello");
	})
    return (
         <div>
           <button onClick={e => setCount(count + 1)} >state</button>
         </div>
    );
}

function App() {

  return (
    <div className="App">
     <Test ></Test>
    </div>
  );
}
```
   
##### 2: Một mảng trống []

- Khi bạn truyền một mảng trống vào, nó sẽ chỉ thực thi một lần duy nhất sau khi thành phần đó render lần đầu tiên
 + Chỉ gọi callback một lần  khi componet mounted 

```
function Test() {
  const [count, setCount] = useState(0)
  useEffect(() => {
		console.log("hello");
	},[])
    return (
         <div>
           <button onClick={e => setCount(count + 1)} >state</button>
         </div>
    );
}
```

##### 3. Khi truyền các Props, State
 - Khi dependencies là các props, state bên trong một mảng . React useEffect sẽ dựa vào giá trị props, state. 
  + Thực hiện call back mỗi khi props, state được thay đổi khác với  giá trị trước đó 

```
function Test() {
  const [count, setCount] = useState(0)
  useEffect(() => { // useEffect callback được gọi khi state thay đổi so với giá trị trước đó
		console.log("hello");
	},[count])
    return (
         <div>
           <button onClick={e => setCount(count + 1)} >state</button>
         </div>
    );
}

```

### useContext

- React Context là một cách để quản lý trạng thái trên toàn cầu.
- Nó có thể được sử dụng cùng với useState Hook để chia sẻ trạng thái giữa các thành phần lồng vào nhau dễ dàng hơn so với useState một mình.


```
import React from "react";
import ReactDOM from "react-dom";

// tạo một Context
const NumberContext = React.createContext();
// It returns an object with 2 values:
// { Provider, Consumer }

function App() {
  // Sử dụng  Provider để cung cấp một giá trị có sẵn cho tất cả
  // children và grandchildren
  return (
    <NumberContext.Provider value={42}>
      <div>
        <Display />
      </div>
    </NumberContext.Provider>
  );
}

function Display() {
  // Sử dụng Consumer để lấy giá trị từ ngữ cảnh
  // Lưu ý rằng thành phần này không nhận được bất kỳ props!
  return (
    <NumberContext.Consumer>
      {value => <div>The answer is {value}.</div>}
    </NumberContext.Consumer>
  );
}
```
## Bổ sung

### useReducer 
- useReducer là một hàm có 2 tham số là (state, action) và trả về new state sau khi thực hiện một action

```
import './App.css';
import React, { useEffect, useReducer, useState } from 'react';


const initialState = { width: 15 };

const reducer = (state, action) => {
  switch (action) {
    case 'plus':
      return { width: state.width + 15 }
    case 'minus':
      return { width: Math.max(state.width - 15, 2) }
    default:
      throw new Error("what's going on?" )
  }
}


function Test() {
  const [state, dispatch] = useReducer(reducer, initialState)
  return <>
    <div style={{ background: 'teal', height: '30px', width: state.width }}></div>
    <div style={{marginTop: '3rem'}}>
        <button onClick={() => dispatch('plus')}>Increase bar size</button>
        <button onClick={() => dispatch('minus')}>Decrease bar size</button>
    </div>
    </>
}

function App() {

  return (
    <div className="App">
     <Test ></Test>
    </div>
  );
}

export default App;
```

- useReducercó thể được sử dụng thay thế cho useState. Đó là lý tưởng cho logic trạng thái phức tạp khi có sự phụ thuộc vào các giá trị trạng thái trước đó hoặc nhiều giá trị phụ của trạng thái.
ví dụ:

```
const initialState = { width: 15 }; 

const reducer = (state, newState) => ({
  ...state,
  width: newState.width
})
const [state, setState] = useReducer(reducer, initialState)
```

### useMemo

useMemo giúp ta kiểm soát việc được render dư thừa của các component con tránh thực hiện lặp đi lặp lại các hoạt động có khả năng tốn kém cho đến khi cần thiết.

> Hãy cẩn thận khi sử dụng phương pháp này vì bất kỳ biến thay đổi nào được xác định trong hàm đều không ảnh hưởng đến hành vi của  useMemo

```

function App(){
  const [age, setAge] = useState(99)
  const handleClick = () => setAge(age + 1)
  const someValue = { value: "someValue" }
  const doSomething = () => {
    return someValue
  }

  return (
    <div>
      <Age age={age} handleClick={handleClick}/>
      <Instructions doSomething={doSomething} />
    </div>
  )
}
export default App;

function Age({ age, handleClick }){
return (
  <div>
    <div style={{ border: '2px', background: "papayawhip", padding: "1rem" }}>
      Today I am {age} Years of Age
    </div>
    <pre> - click the button below 👇 </pre>
    <button onClick={handleClick}>Get older! </button>
  </div>
)
}

const Instructions = React.memo((props) => {
return (
  <div style={{ background: 'black', color: 'yellow', padding: "1rem" }}>
    <p>Follow the instructions above as closely as possible</p>
  </div>
)
})
```

### useRef

- useRef trả về một đối tượng "ref". Các giá trị được truy cập từ  thuộc tính của đối tượng được trả về
- Ngoài việc chỉ giữ các tham chiếu DOM, đối tượng “ref” có thể giữ bất kỳ giá trị nào. 
- Lưu giữ một biến có thể mutate

```
onst AccessDOM = () => {
  const textAreaEl = useRef(null);
  const handleBtnClick = () => {
    textAreaEl.current.value =
    "The is the story of your life. You are an human being, and you're on a website about React Hooks";
    textAreaEl.current.focus();
  };
  return (
    <section style={{ textAlign: "center" }}>
      <div>
        <button onClick={handleBtnClick}>Focus and Populate Text Field</button>
      </div>
      <label
        htmlFor="story"
        style={{
          display: "block",
          background: "olive",
          margin: "1em",
          padding: "1em"
        }}
      >
        The input box below will be focused and populated with some text
        (imperatively) upon clicking the button above.
      </label>
      <textarea ref={textAreaEl} id="story" rows="5" cols="33" />
    </section>
  );
};
```

### useCallback

- useCallback có nhiệm vụ tương tự như useMemo nhưng khác ở chỗ function truyền vào useMemo bắt buộc phải ở trong quá trình render trong khi đối với useCallback đó lại là function callback của 1 event 

```
function App(){
  const [age, setAge] = useState(99)
  const handleClick = () => setAge(age + 1)
  const someValue = "someValue"
  const doSomething = () => {
    return someValue
  }

  return (
    <div>
      <Age age={age} handleClick={handleClick}/>
      <Instructions doSomething={doSomething} />
    </div>
  )
}
export default App;

function Age({ age, handleClick }){
return (
  <div>
    <div style={{ border: '2px', background: "papayawhip", padding: "1rem" }}>
      Today I am {age} Years of Age
    </div>
    <pre> - click the button below 👇 </pre>
    <button onClick={handleClick}>Get older! </button>
  </div>
)
}

const Instructions = React.memo((props) => {
return (
  <div style={{ background: 'black', color: 'yellow', padding: "1rem" }}>
    <p>Follow the instructions above as closely as possible</p>
  </div>
)
})
```

## Kểt Luận
Bạn có thể tìm hiểu thêm về 1 số hook khác [tại đây](https://react.dev/reference/react)
