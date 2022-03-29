---
title: Next.js Getting Start Note
date: 2022-03-28 22:02:30
tags:
---
## 目錄

1. [Pages](#pages)
   + Dynamic Routes
   + Link
2. [Pre-render](#pre-render)
   + Static generation
   + Server side rendering
   + getServerSideProps, getStaticProps 差別
3. [Head](#head)
4. [Image](#image)
   + local image
   + remote image
   + LCP
   + Priority


## Pages
在 `pages` folder 裡面的 React Component，會根據檔案名稱對應到 route，像是:

```jsx
// (網址) → 檔案位置 
/ → pages/index.js
/about → pages/about.js
/blog/hello-world → pages/blog/[slug].js
```
(被中括號括住的檔名可以使用 dynamic routes，底下介紹)

### Dynamic Routes
可以將檔案/資料夾名稱設定成像是 `[pid]` 這種用中括號包起來的檔案名稱，它會自動對應網址參數名稱
```js
// 在 post資料夾裡面的 [pid]資料夾裡面的 [comment].js
// post/[pid]/[comment].js
import { useRouter } from "next/router";

const Post = () => {
  const router = useRouter();
  // 從網址參數中提取 pid 以及 comment   
  // 如果網址是 "/post/1/hello"，則 pid 為 1，comment 為 hello 
  const { pid, comment } = router.query;
  
  return (
    <p>
      Pid: {pid} Comment: {comment}
    </p>
  );
};

export default Post;

```
### Link
使用 `<Link/>` 來相連不同的 pages，像是底下建立一個 Home Pages 來連接各個其他的 pages

```js
import Link from 'next/link'

function Home() {
  return (
    <ul>
      <li>
        {/* pages/index.js */}
        <Link href="/">
          <a>Home</a>
        </Link>
      </li>
      <li>
        <Link href="/about">
          <a>About Us</a>
        </Link>
      </li>
      <li>
        <Link href="/blog/hello-world">
          <a>Blog Post</a>
        </Link>
      </li>
    </ul>
  )
}

export default Home
```

## Pre-render
Next.js 會 pre-render 所有的 pages，而 pre-render 又可分為 `static generation` 以及 `server-side rendering`，為了更好的效能，官方推薦使用 `static generation`。
詳細: [link](https://nextjs.org/docs/basic-features/pages#pre-rendering)

### Static generation

> If a page uses Static Generation, the page HTML is generated at build time. That means in production, the page HTML is generated when you run next build . This HTML will then be reused on each request. It can be cached by a CDN.

使用 static generation 時, 若我們需要使用外部資源來引入我們的 component 的話，可以`export async getStatciProps` 來獲得外部資源。

底下有一個 Blog Component 負責 render 外部引入的 posts
```js
function Blog({ posts }) {
  return (
    <ul>
      {posts.map(post => <li>{post.title}</li>)}
    </ul>
  )
}

export default Blog
```

而接下來我們要 `export` 一個 `async` function `getStaticProps` ，讓這個 Component 能在 pre-render 時就獲得外部資源 ( `getStaticProps` 會在 build time 時被呼叫)
```js
// This function gets called at build time
export async function getStaticProps() {
  // Call an external API endpoint to get posts
  const res = await fetch('https://.../posts')
  const posts = await res.json()

  // By returning { props: { posts } }, the Blog component
  // will receive posts as a prop at build time
  return {
    props: {
      posts
    }
  } 
}
```

### Server-side Rendering
> If a page uses Server-side Rendering, the page HTML is generated on each request.

server-side rendering 需要使用到外部資源的話，我們可以 `export async getServerSideProps`，它會在每次送出 request 時被呼叫。

```js
function Pages({ data }) {
  // Render data
}

// This gets called on every request
export async function getServerSideProps() {
  // Fetch data from exnternal API
  const res = await fetch('https://.../data')
  const data = await res.json();

  // Pass data to page via props
  return { props: {data} }
}
```
### getServerSideProps, getStaticProps 差別
`getServerSideProps` 會在每次 request 的時候被呼叫，而 `getStaticProps` 只會在 build time 的時候被呼叫。

## Head
我們可以在 component 中引入 Next.js 提供的 `Head` 來設定 html 中的 `<head>`
```js
import Head from "next/head";

function HomePage() {
  return (
    <div>
      <Head>
        <title>Here is HomePage</title>
        <meta name="viewport" content="initial-scale=1.0 width=device-width" />
      </Head>
      <p>Welcome to Next.js!</p>
    </div>
  );
}

export default HomePage;

```

## Image
從 Next.js 中匯入 `Image` component，並加入入圖片的 `src`。

### Local Image
> Next.js will automatically determine the width and height of your image based on the imported file
```js
// use local image 
import profilePic from '../public/me.png'
import Image from 'next/image'

function Home() {
  return (
    <>
      <h1>My HomePage</h1>
      <Image
        src={profilePic}
        alt='Picture of the author'
        // width={500} automatically provided
        // height={500} automatically provided
        // blurDataURL="data:..." automatically provided
        // placeholder="blur" // Optional blur-up while loading
      />
      <p>Welcome to my homePage!</p>
    <>
  )
}
```

### Remote Image
> To use a remote image, the src property should be a URL string, which can be relative or absolute. Because Next.js does not have access to remote files during the build process, you'll need to provide the width, height and optional blurDataURL props manually

More: [link](https://nextjs.org/docs/basic-features/image-optimization#remote-images)

### LCP (Largest Contentful Paint)
甚麼是 LCP 呢？ LCP翻譯是「最大內容繪製」，簡單來說就是網站最大的文字、圖片或是影片呈現到眼前所需要的時間，越快越好，Google只測試網站最大的那個內容，以一般網站首頁來說，第一個被掃到的很可能就是形象大圖，或是首頁形象影片，這些內容的優化就變得非常重要，也是影響網站速度的重要原因。 

原出處: [網站速度- SEO優化的重要項目](https://www.da-vinci.com.tw/tw/seo-speed)

### Priority
我們應該要在每個 page 中設定它的 Largest Contentful Paint (LCP) element，使 Next.js 能夠優先 loading 它，提升 LCP。
```diff
import Image from 'next/image'

export default function Home() {
  return (
    <>
      <h1>My Homepage</h1>
      <Image
        src="/me.png"
        alt="Picture of the author"
        width={500}
        height={500}
+       priority
      />
      <p>Welcome to my homepage!</p>
    </>
  )
}
```