

# Build your site with Gatsby Cloud

Go to your Gatsby Cloud Dashboard. Click on the “Add a site” button.



GitHubリポジトリのメインブランチに新しい変更をプッシュするたびに、Gatsby Cloudは変更を確認し、サイトの新しいバージョンのビルドを自動的に開始します。

# Use and Style React Components

## Create a page component

* pagesディレクトリに追加することでページが追加される

```js
// Step 1: Import React
import * as React from 'react'
// Step 2: Define your component
const IndexPage = () => {
  return (
    <main>
      <title>Home Page</title>
      <h1>Welcome to my Gatsby site!</h1>
      <p>I'm making this by following the Gatsby Tutorial.</p>
    </main>
  )
}
// Step 3: Export your component
export default IndexPage
```

## Use the <Link> component

```
import { Link } from 'gatsby'
<Link to="/about">About</Link>
```

## 再利用可能なlayoutコンポーネント

src/components

## Style components with CSS Modules

* src/components/layout.module.css

```css
.container {
  margin: auto;
  max-width: 500px;
  font-family: sans-serif;
}
```

```js
import { container } from './layout.module.css'
<div className={container}>
```