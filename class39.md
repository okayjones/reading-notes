# React III

[Next.js Basics](https://nextjs.org/learn/basics/create-nextjs-app)

## Create a Next.js App

1. Create app

    ```bash
    npx create-next-app nextjs-blog --use-npm --example [template name]
    ````

2. Run server

    ```bash
    cd nextjs-blog
    npm run dev
    ````

3. Go to [http://localhost:3000](http://localhost:3000)
4. Edit `pages/index.js`

## Navigate Between Pages

- A page is a React component
- Pages have routes associated
- Route is defined by file name
- Component can be named anything
- Must export as default

```jsx
// pages/first-post.js
// route is 
export default function FirstPost() {
  return <h1>First Post</h1>
}
```

### Link Component

- Used instead of `<a>`
- Import from `next/link`
- Client side navigation
  - Faster than default navigation
  - Browser does not load full page
- Automatic code splitting, so only required things are rendered
- Link component pre-fetches linked code in background
- Keep using `<a>` for external links

```jsx
// home
<h1 className="title">
  Read{' '}
  <Link href="/posts/first-post">
    <a>this page!</a>
  </Link>
</h1>
// posts/first-post
import Link from 'next/link'
export default function FirstPost() {
  return (
    <>
      <h1>First Post</h1>
      <h2>
        <Link href="/">
          <a>Back to home</a>
        </Link>
      </h2>
    </>
  )
}
```

## Assets, Metadata, and CSS

- Use top-level `public` directory
- Can also be used for things like `robots.txt` and Google Site Verification
- `Image` component handles responsiveness
- Works on external image links
- Lazy loading by default

### Images

```jsx
import Image from 'next/image'
const YourComponent = () => (
  <Image
    src="/images/profile.jpg" // Route of the image file
    height={144} // Desired size with correct aspect ratio
    width={144} // Desired size with correct aspect ratio
    alt="Your Name"
  />
)
```

### Metadata

```jsx
import Head from 'next/head'
export default function FirstPost() {
  return (
    <>
      <Head>
        <title>First Post</title>
      </Head>
      <h1>First Post</h1>
      <h2>
        <Link href="/">
          <a>Back to home</a>
        </Link>
      </h2>
    </>
  )
}
```

### CSS

- Support for `styled-jsx`
- Support for `styled-components`
- Support for `emotion`
- Support for Sass
- Support for CSS libraries like `Tailwind`

### Layout component

```jsx
// components/layout.js
export default function Layout({ children }) {
  return <div>{children}</div>
}
```

```jsx
// pages/posts/first-post.js
import Head from 'next/head'
import Link from 'next/link'
import Layout from '../../components/layout'
export default function FirstPost() {
  return (
    <Layout>
      <Head>
        <title>First Post</title>
      </Head>
      <h1>First Post</h1>
      <h2>
        <Link href="/">
          <a>Back to home</a>
        </Link>
      </h2>
    </Layout>
  )
}
```

### Adding CSS with Modules

- Useful for component-level styles

```css
/*  components/layout.module.css */
.container {
  max-width: 36rem;
  padding: 0 1rem;
  margin: 3rem auto 6rem;
}
```

```jsx
// components/layout.js (above)
import styles from './layout.module.css'
export default function Layout({ children }) {
  return <div className={styles.container}>{children}</div>
}
```

### Global CSS

- CSS to load for every page

```jsx
// pages/_app.js
import '../styles/global.css'
export default function App({ Component, pageProps }) {
  return <Component {...pageProps} />
}
```

> `npm run dev` to pickup changes
```css
/* styles/global.css */
html,
body {
  padding: 0;
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, Segoe UI, Roboto, Oxygen, Ubuntu,
    Cantarell, Fira Sans, Droid Sans, Helvetica Neue, sans-serif;
  line-height: 1.6;
  font-size: 18px;
}
* {
  box-sizing: border-box;
}
a {
  color: #0070f3;
  text-decoration: none;
}
a:hover {
  text-decoration: underline;
}
img {
  max-width: 100%;
  display: block;
}
```