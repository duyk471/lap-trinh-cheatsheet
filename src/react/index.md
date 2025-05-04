# Bắt đầu

## Khởi tạo dự án

Đọc thêm trong [Creating a React App](https://react.dev/learn/creating-a-react-app)

### Creating a React App

```
npx create-react-router@latest
```

Cái này sẽ tự động setup Framework cho các ông làm việc. Tôi cũng không hiểu lắm và vì cũng mới học nên tôi sẽ chọn "Build a React app from Scratch".

### Build a React app from Scratch
Đọc thêm trong [Build a React app from Scratch](https://react.dev/learn/build-a-react-app-from-scratch
). Chọn Vite vì thấy mấy ông dev cũng dùng và không biết gì về Parcel với Rsbuild.

```
npm create vite@latest my-app -- --template react
```

### Chọn cái nào
Theo Documentation thì: 

> If you want to build a new app or website with React, we recommend starting with a framework (Chính là cái _Creating a React App_ ở trên).

Và thêm nữa là:

> Starting from scratch is an easy way to get started using React, but a major tradeoff to be aware of is that going this route is often the same as building your own adhoc framework. As your requirements evolve, you may need to solve more framework-like problems that our recommended frameworks already have well developed and supported solutions for.

## Một số thiết lập khác
### Adding TypeScript to an existing React project

```
npm install @types/react @types/react-dom
```
The following compiler options need to be set in your `tsconfig.json`:

1.  `dom` must be included in [`lib`](https://www.typescriptlang.org/tsconfig/#lib) (Note: If no `lib` option is specified, `dom` is included by default).
2.  [`jsx`](https://www.typescriptlang.org/tsconfig/#jsx) must be set to one of the valid options. `preserve` should suffice for most applications.

###

Diểm khác biệt khi sử dụng thêm TypeScript thì là... có thể thêm kiểu dữ liệu vào kiểu như này:

```jsx
function MyButton({ title }: { title: string }) {
  return (
    <button>{title}</button>
  );
}

export default function MyApp() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton title="I'm a button" />
    </div>
  );
}
``` 

### useState

```jsx
// Infer the type as "boolean"
const [enabled, setEnabled] = useState(false);
// Trong trường hợp mà cần phải khai báo thêm kiểu thì
type Status = "idle" | "loading" | "success" | "error";
const [status, setStatus] = useState<Status>("idle");
```
# Khởi động chút React

Theo như trang hướng dẫn nói thì cái trang này sẽ bao gồm khoảng 80% những gì mà bạn sẽ dùng trong React hàng ngày. Gọi bài này là dịch thì *vừa đúng vừa không đúng* vì mình đã thêm bớt khá nhiều sao cho hợp với cái hiểu của mình. Nhưng cấu trúc từng phần về cơ bản vẫn vậy.

Mình nghĩ là các nguyên lý cơ bản sẽ không dễ bị thay đổi quá nhanh cho nên là chắc để vậy cũng oke.

### Tạo và nest các components

Về cơ bản thì ứng dụng React toàn là *components* (A component is a piece of the UI (user interface) that has its own logic and appearance. A component can be as small as a button, or as large as an entire page). React components về cơ bản là JavaScript functions, nhưng trả về Markup:

```js
function MyButton() {
  return (<button>I'm a button</button>);
}
// Đã khai báo `MyButton` rồi nên có thể dùng trong Component khác, kiểu như này:
export default function MyApp() {
  return (<div><h1>Welcome to my app</h1><MyButton /></div>);
}
```

Nhớ là `<MyButton />` bắt đầu bằng chữ cái in hoa để nhận diện component, còn in thường là cho HTML tag. 

### Viết markup bằng JSX

Cú pháp markup được dùng ở đây là *JSX* (Dù nó là *optional*, nhưng mấy ông dev bảo chẳng ai khổ dâm mà không dùng *JSX* cả)

JSX thì cứng nhắc hơn HTML. Ví dụ như cần phải đóng tag `<br />`. Và cũng chỉ được trả một JSX tag. Hoặc là gói trong một *shared parent*, kiểu như `<div>...</div>` hoặc `<>...</>` 

```
function AboutPage() {return (<><h1>About</h1><p>Hello there.<br />How do you do?</p></>);}
```

If you have a lot of HTML to port to JSX, you can use an [online converter](https://transform.tools/html-to-jsx)

### Thêm chút kiểu cách (styles)

Trong React thì khai báo CSS class bằng `className`. Hoạt động tương đương thuộc tính [`class`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/class) của HTML:

```html
<img className="avatar" />
```

Rồi viết quy tắc CSS sang một tệp khác

```css
/* In your CSS */
.avatar {border-radius: 50%;}
```

Thêm CSS thì tương tự như bình thường, có thể là thêm tag [`<link>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link) vào tệp HTML.

### Hiện data

Sử dụng cặp ngoặc nhọn để thêm chút JavaScript vào `{}` ví dụ như `user.name`:

```
return (<h1>{user.name}</h1>);
```

Cũng có thể làm tương tự trong các thuộc tính của "tag" JSX. Ví dụ như `src={user.imageUrl}` sẽ đọc biến `user.imageUrl` trong JavaScript, và đưa vào thuộc tính `src`:

```
return (<imgclassName="avatar"src={user.imageUrl}/>);
```

<!-- In the above example, `style={{}}` is not a special syntax, but a regular `{}` object inside the `style={ }` JSX curly braces. You can use the `style` attribute when your styles depend on JavaScript variables. -->

### Conditional rendering

Về cơ bản thì dùng như viết JavaScript, như đoạn code dưới đây:

```js
let doAn;
if (duTienMua) {
  doAn = <BanhMi />;
} else {
  doAn = <LamGiCoGiMaAn />; 
}
return (
  <div>
    {doAn}
  </div>
);
```
Hoặc đơn giản hơn thì dùng Elvis operator
```
<div>{duTienMua ? (<BanhMi />) : (<LamGiCoGiMaAn />)}</div>
// Nếu không cần `else` thì gọn nữa
<div>{duTienMua && <BanhMi />}</div>
```

### Rendering lists

Dùng các chức năng của bên JS [`for` loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for) hoặc [array `map()` function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) để render list components.


```js
const products = [
  { title: 'Cabbage', id: 1 },
  { title: 'Garlic', id: 2 },
  { title: 'Apple', id: 3 }
];
// Inside your component, use the `map()` function to transform an array of products into an array of `<li>` items:
const listItems = products.map(
  product =>
    <li key={product.id}>{product.title}</li>
);
return (<ul>{listItems}</ul>);

```

Để ý là `<li>` có thuộc tính `key` sử dụng để nhận diện giữa các Item với nhau.

### Responding to events

Khai báo *event handler* function trong component:

```js
function MyButton() {
  function handleClick() {
    alert('Mày bấm cái nút này xem');
  }

  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );
}
```

`onClick={handleClick}` không sử dụng dấu `()` nha, không phải là `onClick={handleClick()}` vì chúng ta đang coi `handleClick()` là một argument của `onClick` chứ không phải để gọi. 

### Cập nhật màn hình

Often, you'll want your component to "remember" some information and display it. For example, maybe you want to count the number of times a button is clicked. To do this, add *state* to your component.

Đầu tiên hãy import [`useState`](https://react.dev/reference/react/useState).

```
import { useState } from 'react';
```

Now you can declare a *state variable* inside your component:

```js
function MyButton() {
  const [count, setCount] = useState(0);
  // ...
```

Bạn sẽ nhận được hai thứ từ `useState`: state (trạng thái) hiện tại (`count`), và function để update cái state đấy (`setCount`). Theo convention (Cách mà mọi người thường làm, mà nó tiện và đúng nhá) `[something, setSomething]`.

Xem thêm phần ví dụ chạy phần Code cho phần này trong [Quick Start](https://react.dev/learn#responding-to-events)

### Dùng Hooks

Functions bắt đầu bằng `use` được gọi *Hooks*, ví dụ như `useState`. Đọc danh sách các đồ đã có sẵn[API reference.](https://react.dev/reference/react), hoặc tự tạo cũng được.

Hooks are more restrictive than other functions. You can only call Hooks *at the top* of your components (or other Hooks). If you want to use `useState` in a condition or a loop, extract a new component and put it there.

Sẽ cần nói thêm về Hook, nhưng chắc để sau.

### Chia sẻ data giữa các components

Thường thì như bạn có thể đã biết, mỗi một Component đều có cho mình một State riêng. Nhưng đôi khi, bạn cần các components share data và luôn cập nhật (giống nhau).# Bàn về UI trong React

## Your First Component
Trong biển code React chẳng có gì ngoài Components, có thể nói vậy.

> Components are one of the core concepts of React.

```jsx
// The `export default` prefix is a standard JavaScript syntax (not specific to React). It lets you mark the main function in a file so that you can later import it from other files.
// React components are regular JavaScript functions, but **their names must start with a capital letter** or they won't work!
export default function Profile() {
  // But if your markup isn’t all on the same line as the return keyword, you must wrap it in a pair of parentheses:
  // Without parentheses, any code on the lines after return will be ignored!
  return (
    <img
      src="https://i.imgur.com/MK3eW3Am.jpg"
      alt="Katherine Johnson"
    />
  )
}
```

Gọi component ra kiểu như thế này

```jsx
export default function Gallery() {
  return (
    <section>
      <h1>Amazing scientists</h1>
      <Profile />
    </section>
  );
}
```

Nó sẽ trả ra như này (Note: Mình thấy bình thường họ hay trả `<>{}</>` hoặc dùng `<div>`, nếu trả `<section>` thì HTML sẽ _semantic_)

```html
<section>
  <h1>Amazing scientists</h1>
  <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
  <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
  <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
</section>
```

Components can render other components (so you can keep multiple components in the same file), but you must never nest their definitions:

```jsx
// Như này là sai vl
export default function Gallery() {
  // 🔴 Never define a component inside another component!
  function Profile() {
    // ...
  }
  // ...
}

// Mà phải như này (Cũng không tối ưu lắm)
export default function Gallery() {
  return (
    <section>
      <Profile />
    </section>
  )
}

function Profile() {
  // ...
}
```

## Importing and Exporting Components

### The root component file

These currently live in a root component file, named `App.js` in this example. Depending on your setup, your root component could be in another file, though

Nếu đây là tệp `App.jsx` 

```jsx
function Profile() {
  return (
    <img
      src="https://i.imgur.com/MK3eW3As.jpg"
      alt="Katherine Johnson"
    />
  );
}

export default function Gallery() {
  return (
    <section>
      <Profile />
    </section>
  );
}

```

Giả sử tệp trên được ném sang một tệp khác `Gallery.jsx` chẳng hạn, thì muốn sử dụng `<Gallery />` ở tệp khác thì cần `import` vào:

```jsx
import Gallery from './Gallery.js';
// Như này cũng được, mà nên như trên hơn: import Gallery from './Gallery';
export default function App() {
  return (
    <Gallery />
  );
}
```

### Exporting and importing multiple components from the same file
A file can only have one default export, but it can have numerous named exports!

When you write a *default* import, you can put any name you want after `import`. For example, you could write `import Banana from './Button.js'` instead and it would still provide you with the same default export.

| Syntax | Export statement | Import statement |
| --- | --- | --- |
| Default | `export default function Button() {}` | `import Button from './Button.js';` |
| Named | `export function Button() {}` | `import { Button } from './Button.js';` |

Ví như trong tệp `Gallery.js` ở trên

```jsx
export function Profile() {
  // ...
}
```

```jsx
import { Profile } from './Gallery.js';
// Finally, render <Profile /> from the App component:
export default function App() {
  return <Profile />;
}
```

## JSX

### The Rules of JSX
- Return a single root element: Kiểu `<div></div>`
- camelCase: stroke-width you use strokeWidth. Since class is a reserved word, in React you write className instead, named after the corresponding DOM property (**Pitfall**: For historical reasons, aria-* and data-* attributes are written as in HTML with dashes.)

### JavaScript in JSX with Curly Braces
Kiểu như này

```jsx
export default function Avatar() {
  const avatar = 'https://i.imgur.com/7vQD0fPs.jpg';
  const description = 'Gregorio Y. Zara';
  return (
    <img
      className="avatar"
      src={avatar}
      alt={description}
    />
  );
}
```

You can only use curly braces in two ways inside JSX:

1.  **As text** directly inside a JSX tag: `<h1>{name}'s To Do List</h1>` works, but `<{tag}>Gregorio Y. Zara's To Do List</{tag}>` will not.
2.  **As attributes** immediately following the `=` sign: `src={avatar}` will read the `avatar` variable, but `src="{avatar}"` will pass the string `"{avatar}"`.

Using “double curlies”: CSS and other objects in JSX: Can even pass objects in JSX. Objects are also denoted with curly braces, like `{ name: "Hedy Lamarr", inventions: 5 }`. To pass a JS object in JSX: `person={{ name: "Hedy Lamarr", inventions: 5 }}`.

Inline `style` properties are written in camelCase. For example, HTML `<ul style="background-color: black">` would be written as `<ul style={{ backgroundColor: 'black' }}>` in your component.

```jsx
export default function TodoList() {
  return (
    // Là cái này này
    <ul style={{
      backgroundColor: 'black',
      color: 'pink'
    }}>
      <li>Improve the videophone</li>
      <li>Prepare aeronautics lectures</li>
      <li>Work on the alcohol-fuelled engine</li>
    </ul>
  );
}

```

```jsx
const person = {
  name: 'Gregorio Y. Zara',
  theme: {
    backgroundColor: 'black',
    color: 'pink'
  }
};


export default function TodoList() {
  return (
    <div style={person.theme}>
      <h1>{person.name}'s Todos</h1>
      <img
        className="avatar"
        src="https://i.imgur.com/7vQD0fPs.jpg"
        alt="Gregorio Y. Zara"
      />
      <ul>
        <li>Improve the videophone</li>
        <li>Prepare aeronautics lectures</li>
        <li>Work on the alcohol-fuelled engine</li>
      </ul>
    </div>
  );
}
```
Tips: Kết hợp String vào với nhau cũng oke: `src={baseUrl + person.imageId + person.imageSize + '.jpg'}`

## Passing Props to a Component

Props are the information that you pass to a JSX tag. For example, `className`, `src`, `alt`, `width`, and `height` are some of the props you can pass to an `<img>`:


```jsx
export default function Profile() {
  return (
    // Adding the props to the component looks like this
    <Avatar
      person={{ name: 'Lin Lanying', imageId: '1bX5QH6' }}
      size={100}
    />
  );
}

function Avatar({ person, size }) {  // person and size are available here
}
// Or setting the default value for the props' propert(ies)

// function Avatar({ person, size = 100 }) {
//   // ...
// }
```
### ### Pitfall

**Don't miss the pair of `{` and `}` curlies** inside of `(` and `)` when declaring props:

```jsx
function Avatar({ person, size }) {  
  // ...
}
```

This syntax is called ["destructuring"](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Unpacking_fields_from_objects_passed_as_a_function_parameter) and is equivalent to reading properties from a function parameter:

```jsx
function Avatar(props) {  
  let person = props.person;  
  let size = props.size;  
  // ...
}
```
Mà dùng cái spread syntax này hạn chế thôi nha

###

```jsx
function Profile(props) {
  return (
    <div className="card">
      <Avatar props={...props} />
    </div>
  );
}
```

fact không vui: props là immutable, vậy nên chuyển qua cái `setState`

Thay vì tạo Class (?) hmm
```jsx

function Profile({
  imageId,
  name,
  profession,
  awards,
  discovery,
  imageSize = 70
}) {
  return (
    <section className="profile">
      <h2>{name}</h2>
      <img
        className="avatar"
        src={getImageUrl(imageId)}
        alt={name}
        width={imageSize}
        height={imageSize}
      />
      <ul>
        <li><b>Profession:</b> {profession}</li>
        <li>
          <b>Awards: {awards.length} </b>
          ({awards.join(', ')})
        </li>
        <li>
          <b>Discovered: </b>
          {discovery}
        </li>
      </ul>
    </section>
  );
}

```


## Điều kiện các thứ

```jsx
if (isPacked) {
  return <li className="item">{name} ✅</li>;
}
return <li className="item">{name}</li>;

// or

if (isPacked) {
  return (
  <li className="item">
    {isPacked ? name + ' ✅' : name}
  </li>
);
}
```
## Render list

```jsx
const people = [
  'Creola Katherine Johnson: mathematician',
  'Mario José Molina-Pasquel Henríquez: chemist',
  'Mohammad Abdus Salam: physicist',
  'Percy Lavon Julian: chemist',
  'Subrahmanyan Chandrasekhar: astrophysicist'
];

export default function List() {
  const listItems = people.map(person =>
    <li>{person}</li>
  );
  return <ul>{listItems}</ul>;
}

```

will display a warning

> Warning: Each child in a list should have a unique “key” prop.

giai phap: them id vao

```jsx
<li key={person.id}>...</li>
```

### Pitfall không biết thứ bao nhiêu
Arrow functions implicitly return the expression right after `=>`, so you didn't need a `return` statement:

```
const listItems = chemists.map(person =>  <li>...</li> // Implicit return!);
```

However, **you must write `return` explicitly if your `=>` is followed by a `{` curly brace!**

```
const listItems = chemists.map(person => { // Curly brace  return <li>...</li>;});
```

## Keeping Components Pure

### Về cơ bản là FP

Fact 1: Detecting impure calculations with StrictMode (Trong Main.jsx hay gì đó sẽ có cái này):

```
<StrictMode>
  <App />
</StrictMode>
```

### Local mutation: Your component’s little secret
However, it’s completely fine to change variables and objects that you’ve just created while rendering. In this example, you create an [] array, assign it to a cups variable, and then push a dozen cups into it

## Understanding Your UI as a Tree

Đọc không thấy hiểu lắm nên tạm thời bỏ qua nha.# Adding Interactivity
In React, data that changes over time is called *state*.

## Responding to events

### Adding event handlers

You defined the `handleClick` function and then [passed it as a prop](https://react.dev/learn/passing-props-to-a-component) to `<button>`. `handleClick` is an **event handler.** Event handler functions:

-   Are usually defined *inside* your components.
-   Have names that start with `handle`, followed by the name of the event.

By convention, it is common to name event handlers as `handle` followed by the event name. You'll often see `onClick={handleClick}`, `onMouseEnter={handleMouseEnter}`, and so on.

```jsx
export default function Button() {
  function handleClick() {
    alert('You clicked me!');
  }

  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );
}
```

Hoặc

```jsx

<button onClick={function handleClick() {
  alert('You clicked me!');
}}>

// Or, more concisely, using an arrow function:

<button onClick={() => {
  alert('You clicked me!');
}}>
````

**Pitfall**

Functions passed to event handlers must be passed, not called. For example:

| passing a function (correct) | calling a function (incorrect) |
| --- | --- |
| `<button onClick={handleClick}>` | `<button onClick={handleClick()}>` |

The difference is subtle. In the first example, the `handleClick` function is passed as an `onClick` event handler. This tells React to remember it and only call your function when the user clicks the button.

In the second example, the `()` at the end of `handleClick()` fires the function *immediately* during [rendering](https://react.dev/learn/render-and-commit), without any clicks. This is because JavaScript inside the [JSX `{` and `}`](https://react.dev/learn/javascript-in-jsx-with-curly-braces) executes right away. When you write code inline, the same pitfall presents itself in a different way:

| passing a function (correct) | calling a function (incorrect) |
| --- | --- |
| `<button onClick={() => alert('...')}>` | `<button onClick={alert('...')}>` |


### Passing event handlers as props

Kiểu như này, tạo thêm một cái Component tên là `Button` rồi thêm onClick làm prop.

```jsx
function Button({ onClick, children }) {
  return (
    <button onClick={onClick}>
      {children}
    </button>
  );
}

function PlayButton({ movieName }) {
  function handlePlayClick() {
    alert(`Playing ${movieName}!`);
  }

  return (
    <Button onClick={handlePlayClick}>
      Play "{movieName}"
    </Button>
  );
}
```

### Naming event handler props
Built-in components like `<button>` and `<div>` only support [browser event names](https://react.dev/reference/react-dom/components/common#common-props) like `onClick`. However, when you're building your own components, you can name their event handler props any way that you like.

By convention, event handler props should start with `on`, followed by a capital letter. Kiểu thế này:

```jsx
function Button({ onSmash, children }) {
  return (
    <button onClick={onSmash}>
      {children}
    </button>
  );
}

export default function App() {
  return (
    <div>
      <Button onSmash={() => alert('Playing!')}>
        Play Movie
      </Button>
      <Button onSmash={() => alert('Uploading!')}>
        Upload Image
      </Button>
    </div>
  );
}

```

### Event propagation

Event handlers will also catch events from any children your component might have. We say that an event “bubbles” or “propagates” up the tree: it starts with where the event happened, and then goes up the tree.

Tức là, event con khi được chạy xong thì nó cũng sẽ nhảy lên event cha để chạy nốt, đọc code nha

```jsx
export default function Toolbar() {
  return (
    <div className="Toolbar" onClick={() => {
      alert('You clicked on the toolbar!');
    }}>
      <button onClick={() => alert('Playing!')}>
        Play Movie
      </button>
      <button onClick={() => alert('Uploading!')}>
        Upload Image
      </button>
    </div>
  );
}
```

Giả sử bạn bấm nút 'Playing' thì đoạn alert 'Playing!' đó sẽ được chạy trước (component con), rồi sau đó alert 'You clicked on the toolbar!' sẽ được chạy.

Hoặc: That event object also lets you stop the propagation. If you want to prevent an event from reaching parent components, you need to call `e.stopPropagation()` like this `Button` component does:

Ở phần trên thì thay `<button>` thành component `<Button>` rồi sửa lại thành thế này:

```jsx
function Button({ onClick, children }) {
  return (
    <button onClick={e => {
      e.stopPropagation();
      onClick();
    }}>
      {children}
    </button>
  );
}
```

Ở cái toolbar ở trên nó sẽ thành thế này:

```jsx
<Button onClick={() => alert('Playing!')}>
    Play Movie
</Button>
<Button onClick={() => alert('Uploading!')}>
Upload Image
</Button>
```

Không liên quan: Events may have unwanted default browser behavior. Call `e.preventDefault()` to prevent that.

## State: A Component's Memory

```js
const [index, setIndex] = useState(0);
```
### Meet your first Hook

In React, `useState`, as well as any other function starting with "`use`", is called a Hook.

*Hooks* are special functions that are only available while React is [rendering](https://react.dev/learn/render-and-commit#step-1-trigger-a-render) (which we'll get into in more detail on the next page). They let you "hook into" different React features.

> Hooks — functions starting with use—can only be called at the top level of your components or your own Hooks. You can’t call Hooks inside conditions, loops, or other nested functions. Hooks are functions, but it’s helpful to think of them as unconditional declarations about your component’s needs.

The convention is to name this pair like `const [something, setSomething]`. You could name it anything you like, but conventions make things easier to understand across projects.

### State is isolated and private


## Render and Commit

Viết về cách hoạt động cơ bản của React, kiểu như này

### Step 1: Triggering a render

1.  It's the component's **initial render.**
2.  The component's (or one of its ancestors') **state has been updated.**

Đây là một, còn hai là cái set-Gì-Gì-Đó như kiểu setNumber, sẽ khiến React render lại:

```jsx
import Image from './Image.js';
import { createRoot } from 'react-dom/client';

const root = createRoot(document.getElementById('root'));
root.render(<Image />);
```

### Step 2: React renders your components

-   **On initial render,** React will call the root component.
-   **For subsequent renders,** React will call the function component whose state update triggered the render.


## State as a Snapshot

State hoạt động không giống như một biến bình thường trong JS.

```jsx
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(number + 1);
        setNumber(number + 1);
        setNumber(number + 1);
      }}>+3</button>
    </>
  )
}
```

Có thể nhiều người sẽ nghĩ sau mỗi lần bấm thì nó sẽ là +3, nhưng nó chỉ +1 thôi =)).

**Setting state only changes it for the *next* render.** During the first render, `number` was `0`. This is why, in *that render's* `onClick` handler, the value of `number` is still `0` even after `setNumber(number + 1)` was called.

Hay trong ví dụ này

```jsx
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(number + 5);
        alert(number);
      }}>+5</button>
    </>
  )
}
```

Kết quả vẫn sẽ là:

```jsx
setNumber(0 + 5);
setTimeout(() => {
  alert(0);
}, 3000);
```

Bất ngờ chưa, cái alert vẫn sẽ hiện giá trị 0 thay vì 5.

> **A state variable's value never changes within a render,** even if its event handler's code is asynchronous. Inside *that render's* `onClick`, the value of `number` continues to be `0` even after `setNumber(number + 5)` was called. Its value was "fixed" when React "took the snapshot" of the UI by calling your component.


## Queueing a Series of State Updates

Ở cái ví dụ trên:

```jsx
  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(n => n + 1);
        setNumber(n => n + 1);
        setNumber(n => n + 1);
      }}>+3</button>
    </>
  )
```

## Updating Objects in State

### Treat state as read-only

In other words, you should treat any JavaScript object that you put into state as read-only.

To actually trigger a re-render in this case, create a new object and pass it to the state setting function:

```jsx
const [position, setPosition] = useState({
  x: 0,
  y: 0
});
onPointerMove={e => {
  setPosition({
    x: e.clientX,
    y: e.clientY
  });
}}
```

```jsx
setPerson({
  ...person, // Copy other fields
  artwork: { // but replace the artwork
    ...person.artwork, // with the same one
    city: 'New Delhi' // but in New Delhi!
  }
});
```


## Updating Arrays in State


| Functions | avoid (mutates the array) | prefer (returns a new array) |
| --- | --- | --- |
| adding | `push`, `unshift` | `concat`, `[...arr]` spread syntax ([example](https://react.dev/learn/updating-arrays-in-state#adding-to-an-array)) |
| removing | `pop`, `shift`, `splice` | `filter`, `slice` ([example](https://react.dev/learn/updating-arrays-in-state#removing-from-an-array)) |
| replacing | `splice`, `arr[i] = ...` assignment | `map` ([example](https://react.dev/learn/updating-arrays-in-state#replacing-items-in-an-array)) |
| sorting | `reverse`, `sort` | copy the array first ([example](https://react.dev/learn/updating-arrays-in-state#making-other-changes-to-an-array)) |

Alternatively, you can [use Immer](https://react.dev/learn/updating-arrays-in-state#write-concise-update-logic-with-immer) which lets you use methods from both columns.


Instead, create a new array which contains the existing items and a new item at the end. There are multiple ways to do this, but the easiest one is to use the ... array spread syntax:

```jsx
setArtists( // Replace the state
  [ // with a new array
    ...artists, // that contains all the old items
    { id: nextId++, name: name } // and one new item at the end
  ]
);
// or

setArtists([
  { id: nextId++, name: name },
  ...artists // Put old items at the end
]);
```

Delete

```jsx
setArtists(
  artists.filter(a => a.id !== artist.id)
);
```

Here, artists.filter(a => a.id !== artist.id) means “create an array that consists of those artists whose IDs are different from artist.id”.

In other words, each artist’s “Delete” button will filter that artist out of the array, and then request a re-render with the resulting array. Note that filter does not modify the original array.

Thay đổi cho một vài hoặc tất cả các item trong array bằng `map()`
```js
function handleClick() {
  const nextShapes = shapes.map(shape => {
    if (shape.type === 'square') {
      // No change
      return shape;
    } else {
      // Return a new circle 50px below
      return {
        ...shape,
        y: shape.y + 50,
      };
    }
  });
  // Re-render with the new array
  setShapes(nextShapes);
}
```

Replace

```js

const [counters, setCounters] = useState(
  initialCounters
);

function handleIncrementClick(index) {
  const nextCounters = counters.map((c, i) => {
    if (i === index) {
      // Increment the clicked counter
      return c + 1;
    } else {
      // The rest haven't changed
      return c;
    }
  });
  setCounters(nextCounters);
}
```

Insert ở giữa

```jsx
function handleClick() {
  const insertAt = 1; // Could be any index
  const nextArtists = [
    // Items before the insertion point:
    ...artists.slice(0, insertAt),
    // New item:
    { id: nextId++, name: name },
    // Items after the insertion point:
    ...artists.slice(insertAt)
  ];
  setArtists(nextArtists);
  // Để khởi tạo lại cái hộp ghi tên thành rỗng
  setName('');
}

```

Reverse hay sort các thứ

```js
const [list, setList] = useState(initialList);

function handleClick() {
  const nextList = [...list];
  nextList.reverse();
  setList(nextList);
}
```

Here, you use the `[...list]` spread syntax to create a copy of the original array first. Now that you have a copy, you can use mutating methods like `nextList.reverse()` or `nextList.sort()`, or even assign individual items with `nextList[0] = "something"`.

Đến đây thì dừng lại việc học phần 3 và 4 vì nó được đánh dấu là trung cấp, mình sẽ sửa lại sau.