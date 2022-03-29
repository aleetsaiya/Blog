---
title: Mastering React 課程筆記
date: 2021-06-14 18:42:24
---

## 建立 React Project
```Bash
create-react-app myapp
cd myapp
npm start
```
## 使用 bootstrap
```bash
npm install bootstrap
```
index.js
```javascript
import 'bootstrap/dist/css/bootstrap.min.css';
```
## 名詞介紹

### JSX
參考 : [Day02-React.js基本介紹(JSX)](https://ithelp.ithome.com.tw/articles/10216945)

JSX 就如同把 HTML 利用 Javascript 表示出來的語法糖 ( `Syntactic sugar` )。

ex.
```javascript
const element = <h1>Hello World</h1>;
```

使用 JSX 時需要注意的是，如果要使用變數的話需要加一個大括號 `{}`
```javascript
const message = 'Hello World';
const element = <h1>{message}</h1>
```

而如果遇到多行 HTML 的情況下，要用一個括號把他們刮起來 `()`
```javascript
const message = (
    <div>
        <h1>Title</h1>
        <p>content</p>
    </div>
)
```

在 JSX 裡，class 要寫成 `className`
```javascript
const message = <div className="title">Title</div>
```

### Babel 
> 引用至 [React 生態系（Ecosystem）入門簡介](https://github.com/kdchang/reactjs101/blob/master/Ch01/react-ecosystem-introduction.md)

由於並非所有瀏覽器都支援 ES6+ 語法，所以透過 Babel 這個 JavaScript 編譯器（可以想成是翻譯機或是翻譯蒟篛）可以讓你的 ES6+ 、JSX 等程式碼轉換成瀏覽器可以看得懂的語法。通常會在資料夾的 root 位置加入 .babelrc 進行轉譯規則 preset 和引用外掛（plugin）的設定。

## Component
一個網頁是由許多 Component 組成，每個 Component 檔在影片中都為 .jsx 副檔名，後來爬文後發現似乎使用 .js 或是 .jsx 都是可以的。

hello.jsx
```javascript
import React, { Component } from 'react';

class Hello extends Component {
    state = {

    }
    render() {
        return <h1>Hello World</h1>;
    }
}

export default Hello;
```

index.js
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import 'bootstrap/dist/css/bootstrap.css';
import Hello from './components/hello';

ReactDOM.render(<Hello />, document.querySelector('#root'));
```

其中 `state` 負責存每個 Component 的變數，而 `render` 會負責回傳需要顯示出的畫面。接下來再將 Component 由 `ReactDom.render()` 顯示在網頁上。(第一個參數為 component, 第二個參數為 html selector)

### Arrow Function
在 Component 中如果 function 中會用到 this 的話，建議使用 arrow function，不然 this 不會指向物件本身。
```javascript
sample() = () => {
    console.log(this);
}
```

### 更新 `state` 的值
要更新 `state` 的值的話，如果寫
```javascript
state = {
    count: 0,
}

this.state.count ++;
```
這樣是沒有用的。要使用 `setState` 來更新值
```javascript
this.setState({ count: this.state.count + 1 });
```

## Composing Components
> 組合多個 Components 至一個 Component 中 ( Component tree )

### Pass Data
在我們組合多個 Components 時，會一種強況是需要從上層 Components 傳送值到下層 Components ( Parent to Child )

ex. 我們要把值從 Counters.jsx 傳到 Counter.jsx

```javascript
// in counters.jsx
render() {
    return <Counter key={counter.id} value={counter.value}/>
}
```
這時候下層接收值的方式就是 `this.props`。( props 的意思就是 properties 的意思 )

```javascript
// in counter.jsx
state = {
    value: this.props.value //接收 counter.value
}
```

*課程中推薦 Debug 工具* : [React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi)

### Raise and Handle Event
因為每個 Component 裡的 `state` 都為 private 的 ( 只有 Component 本身可以訪問 )，所以當我們要從 Child 裡更新 Parent 值的時候會無法更新。像是 Counters 創造了 4 個 Counter，而 Counter 無法在 counter.jsx 內刪除自己本身，只能在 counters.jsx 中刪除。

這時我們可以透過 Parent 將負責更新的 function 傳給 Child，這樣會變成 Child 呼叫更新 function 而更新的動作依然在 Parent 內。

```javascript
// in counters.jsx ( Parent )
// 傳送 handleDelete
handleDelete = (counterId) => {
    const counters = this.state.counters.filter(c => c.id !== counterId);
    this.setState({ counters: counters });
}
render() {
    return (
        <div>
            {this.state.counters.map(counter => 
            <Counter  
            onDelete={this.handleDelete} 
            key={counter.id} 
            value={counter.value}
            id={counter.id}/>)}
        </div>
    );
}
```
```javascript
// in counter.js ( Child )
// 呼叫 handleDelete
<button  onClick={() => this.props.onDelete(this.props.id)} className="btn btn-danger btn-sm btn-danger m-2">Delete</button>
```

### Functional Components
如果一個 Component 沒有使用到 state 以及其它 function，就只包含 render return 的話，可以改使用 function 來代表一個 Component 而非 class。

```javascript
const NavBar = (props) => {
    return (...);
};

export default NavBar;
```

### Lifecycle Hooks
> 分別為 Mount, Update, Unmounting

+ Mount Phase
When instance created and insert to the DOM。
呼叫: `constructor` > `render` (recursive) > `componentDidMount`

+ Update Phase
When the state of the props of the component get changed。
呼叫: `render` > `componentDidMount`

+ Unmounting Phase
When the component remove from the DOM。
呼叫:  `componentWillUnmount`

## Routing
這邊使用 `react-router-dom`
```bash
npm install react-router-dom
```
### BrowserRouter
將 Component 用 `<BrowserRouter/>` 包起來
```javascript
ReactDOM.render(<BrowserRouter><App /></BrowserRouter>, document.getElementById('root'));
```

### Route
`<Route/>` 會根據不同的網址而呈現不同的東西(Component)。`<Switch/>` 可以使 `<Route/>` 不會同時出現多個 Route Component 的情況。`exact` 用來確保網址完全與設定的 path 相同。
```javascript
class App extends Component {
  render() {
    return (
      <div>
        <NavBar />
        <div className='content'>
          <Switch>
            <Route path='/products' component={Products} />
            <Route path='/posts' component={Posts} />
            <Route path='/admin' component={Dashboard} />
            <Route path='/' exact component={Home} />
          </Switch>
        </div>
      </div>
    );
  }
}
```

### Link
而要跳轉網頁(更改網址)的話可以使用`<Link/>`。使用`<Link/>` 代替 `<a/>` 可以讓頁面不會重新整理，要注意的是原本的 `href` attribute 要改成 `to`。
```javascript
const NavBar = () => {
  return (
    <ul>
      <li> <Link to="/"> Home </Link> </li>
      <li> <Link to="/products"> Products </Link> </li>
      <li> <Link to="/posts/2018/06"> Posts </Link> </li>
      <li> <Link to="/admin"> Admin </Link> </li>
    </ul>
  );
};
```

### Redirect
`<Redirect/>` 可以設定如果使用者到特定網址時，要重新導向哪一個網址。
```javascript
<Redirect from='/' to='/movies'/> //從 home 導向到 movies
<Redirect to='/not-found'/> //如果 <Route/> 都沒符合的話，最後寫這行可以讓不符合的網址都導向 not-found
```

### Url Data
在 `<Route/>` 設定網址時，可以設定變數然後傳送值。
```javascript
<Route path='movies/:id' component={MovieForm}/>
```

in movieForm.jsx
```javascript
const MovieForm({ match }) => {
  // 對應到上面的 /:id
  return <h1>Movie Form {match.params.id} </h1>
}
```

## Form
為了讓 `input` 變數裡的值跟 `state` 裡的變數相連再一起，要先把 `<input>` 裡的 `value` 更改設定，讓 input 欄位裡顯示的值是根據 state 裡的值來顯示。
```javascript
state = {
  account: {
    username: "",
    password: ""
  }
}

<input value={this.state.account.username} />
<input value={this.state.account.password} />
```

`value` 設定完之後，要設定 `onChange` 追蹤使用者輸入好讓我們更改 `state` 裡的值。而為了使 hangleChange 知道要更改的 target，將兩個 input 設定 `name` attribute 來辨別。

順序:
1. 執行 render >> 第一次render ( input欄位為空 )
2. 輸入字符 a >> 因為輸入 a，所以此時的 e.currentTarget.value 為 'a' ，呼叫 handleChange。( input欄位還是空的 )
3. 執行 handleChange >> 呼叫 setState 更改 state 裡的值，呼叫 render
4. 執行 render >> 因為重新 render 了一次，所以此時 input 欄位為 state 裡的值 'a'

```javascript
handleChange = e => {
    const account = {...this.state.account};
    account[e.currentTarget.name] = e.currentTarget.value;
    this.setState({ account });
}

<input value={this.state.account.username} name='username' onChange={this.handleChange}/>
<input value={this.state.account.password} name='password' onChange={this.handleChange}/>
```

## Backend 
> 使用 `Json Placeholder` 來獲得後端資料

因為 React 只是一個負責處理 UI 的 Library ，所以相較於其他的 Framework，React 可以使用自己喜歡的 api 來送出 http requests。

常見的 API :
+ Fetch API
+ jQuery AJAX
+ Axios

影片中 Mosh 介紹如何使用 axios。

### Promise
參考: [卡斯伯的 Blog- Promise](https://wcc723.github.io/development/2020/02/16/all-new-promise/#%E9%97%9C%E6%96%BC-Promise-%E5%B8%B8%E8%A6%8B%E5%95%8F%E9%A1%8C)
當我們使用 `axios` 對 backend 送出 asyn requests 時，我們會獲得一個 `Promise`，此時的 promise 還是 pending 階段 (事件已經運行中，尚未取得結果)，運行結束後會有兩種結果 resolved / rejected，分別對應 `promise.then()` 以及 `promise.catch()` ，而如果不想用這種方式寫的話可以使用 `await` 來獲得 Promise 的值。

```javascript
async componentDidMount() {
  // pending > resolved (success) OR rejected (failure)
  const promise = axios.get('https://jsonplaceholder.typicode.com/posts');
  const response = await promise;
  this.setState({ posts: response.data });
}
```

### post 
axios 中使用 `post` 的方法跟 `get` 差不多，只要再多加 `post body` 就好。

```javascript
// 增加一個 post data 
handleAdd = async () => {
  const promise = axios.post(apiEndpoint, {title: 'a', body: 'yo'});
  const { data: post } = await promise;

  const posts = [post, ...this.state.posts];
  this.setState({ posts })
};
```

### update
```javascript
handleUpdate = async post => {
  post.title = 'Updated';
  // Calling backend to update
  // axios.put(targetUrl, updateBody)
  await axios.put(`${apiEndpoint}/${post.id}`, post);

  // Update UI 
  const posts = [...this.state.posts];
  const index = posts.indexOf(post);
  posts[index] = { ...post }; // Create new object to cover the origin one
  this.setState({ posts });
};
```

### delete
```javascript
handleDelete = async post => {
  // Calling backend to delete
  // axios.delete(targetUrl)
  await axios.delete(`${apiEndpoint}/${post.id}`);

  // Update UI
  const posts = this.state.posts.filter(p => p.id !== post.id);
  this.setState({ posts });
};
```

### Optimistic Updates
先更新 UI ，然後再 call api，如果 api 沒順利完成的話再重新變回之前原本的狀態。

```javascript
  handleDelete = async post => {
    const originalPosts = this.state.posts;

    // Update UI
    const posts = this.state.posts.filter(p => p.id !== post.id);
    this.setState({ posts });

    // Calling backend to delete
    try{
      await axios.delete(`${apiEndpoint}/${post.id}`);
      throw new Error("");
    }
    catch (e) {
      alert('Something failed while deleting a post!');
      this.setState({ originalPosts }) // 原本的狀態
    }
  };
```

### Expected vs Unexpected Errors
+ Expected (404: not found, 400: bad request) - CLIENT ERRORS
Display a specific error message

+ Unexpected (network down, server down, db down, bug)
1. Log them
2. Display a generic and friendly error message

```javascript
    try{
      await axios.delete(`${apiEndpoint}/${post.id}`);
    }
    catch (e) {
      if (e.response && e.response.status === 404)
        alert('This post had already been deleted.');
      else {
        console.log('Loggin the error', e);
        alert('An unexpected error occurred.');
      }

      this.setState({ originalPosts })
    }
```

### 錯誤通知
使用 `react-toastify` 可以讓錯誤通知變得很有質感。

```bash
npm install react-toastify
```

import 套件以及 css 樣式，並在 render 內加入 `ToastContainer`。

```javascript
// in app.js
import 'react-toastify/dist/ReactToastify.css';
import { ToastContainer } from 'react-toastify';

render() {
  return(
    <ToastContainer/>
    // ...
  )
}
```

`ToastContainer` 是負責顯示 error message 的區域，而呼叫 error 的方式是 `toast`。除了使用 `toast.error()` 外，也可以用 `warning`, `success` ... 。
```javascript
// in httpSerive
import { toast } from 'react-toastify';

if (error) {
  toast.error('An unexpected error occurred.');
}
```

### 錯誤追蹤
因為 console.log 只會在自己的裝置上顯示，所以如果想要追蹤其他人的錯誤訊息的話，可以使用 [Sentry]('https://sentry.io/welcome/?utm_source=google&utm_medium=cpc&utm_campaign=9575834316&utm_content=g&utm_term=sentry&gclid=CjwKCAjwoZWHBhBgEiwAiMN66fWVuX2TYiCWVenqdC06GOHkiJ6AhzccGevG8VNXTbK6atw2ZP9PXhoCWaQQAvD_BwE'), 基本上就是照著教學方式進行操作，設定好 js 檔後當別人使用你的網站有錯誤訊息時，你可以看到他的 console 中出現的錯誤。

## React Hook

### useState
`useState` 分別會會回傳一個變數的 value 以及設定那個變數至 `state` 的 function (setState)。
```javascript
const [count, setCount] = useState(0); // count = 0, setCount = setState({ count:  })
const [name, setName] = useState('Alee'); // name = 'Alee', setName = setState({ name:  })
```

### useEffect
用來執行 class component 中的 : 
+ componentDidMount
+ componentDidUpdate
+ componentWillUnmount
會於每一次 render 的時候呼叫 `useEffect`
```javascript
useEffect(() => {
    document.title = title;

    return () => {
        console.log('Clean');
    };
});
```

### useContext
在建立一個 React App 的時候很常會出現從很上面一個一個往下傳，傳到最底下的這種情況。

像是假設現在階層是 A(B(C(D))) ，A 在最上面 D 在最下面，如果我要從 A 傳到 D 的話中間就要經過 B C，然後 B C 也要設定好要傳送的變數。

而如果用 `Context` 的話我們可以把上面的階層變得像是這樣 A(`Context.Provider`(B(C(D)))，這樣子底下的 B C 可以不用設定要傳送的參數，D 也可以更具 `Context Provider` 收到傳送的值。

in userContext.js
```javascript
import React from 'react';

const UserContext = React.createContext();

UserContext.displayName = "UserContext"; // 設定 Context 的名稱為 UserContext

export default UserContext;
```

in App.jsx
```javascript
import React, {useState} from 'react';
import MoviePage from './MoviePage';
import UserContext from './userContext';

export default function Appp(props) {
    const [currentUser, setCurrentUser] = useState({name: 'Alee', age: 21});
    return(
        <UserContext.Provider value={currentUser}>
            <div>
                <MoviePage/> 
            </div>
        </UserContext.Provider>
    );
}
```

in MovieList.jsx
> 中間 App 與 MovieList 相隔一個 MoviePage

```javascript
import React, { useContext } from 'react';
import UserContext from './userContext';

export default function MovieList(props) {
    const currentUser = useContext(UserContext);
    return(
        <div>
            <span>Movie List {currentUser.name} {currentUser.age}</span>
        </div>
    );
}
```

所以使用上大概就是 : 
+ 建立 Context
+ 上層傳送值的 Component 使用 `Context.Provider` 傳送值
+ 底層接收值的 Component 使用 `useContext` 接收值