

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

# plugin

Webサイトの新機能を構築するのは大変な作業になる可能性があります。 幸い、Gatsbyプラグインを使用すると、サイトを最初から作成しなくても、サイトに新しい機能をすばやく追加できます。 Gatsbyのプラグインエコシステムには、何千ものビルド済みパッケージがあり、そこから選択できます。

Configure your plugins in your gatsby-config.js file.
Install the plugin using npm.
Configure the plugin in your site’s gatsby-config.js file.

## static image to your home page

npm install gatsby-plugin-image gatsby-plugin-sharp gatsby-source-filesystem
```js
module.exports = {
  siteMetadata: {
    title: "My First Gatsby Site",
  },
  plugins: [
    "gatsby-plugin-image",
    "gatsby-plugin-sharp",
  ],
};
```

```js
import * as React from 'react'
import Layout from '../components/layout'
import { StaticImage } from 'gatsby-plugin-image'

const IndexPage = () => {
  return (
    <Layout pageTitle="Home Page">
      <p>I'm making this by following the Gatsby Tutorial.</p>
      <StaticImage
        alt="Clifford, a reddish-brown pitbull, posing on a couch and looking stoically at the camera"
        src="https://pbs.twimg.com/media/E1oMV3QVgAIr1NT?format=jpg&name=large"
      />
    </Layout>
  )
}

export default IndexPage
```

# data layer

便利なことに、Gatsbyにはデータレイヤーと呼ばれる強力な機能があり、どこからでもデータをサイトに取り込むために使用できます。 ブログの投稿をWordPressに、ストアの商品をShopifyに、ユーザーデータをAirtableに保存したいですか？ 問題ない！ Gatsbyのデータレイヤーを使用すると、複数のソースからのデータを組み合わせることができ、データの種類ごとに最適なプラットフォームを選択できます。

* Use GraphiQL to explore the data in the data layer and build your own GraphQL queries.
* useStaticQueryフックを使用して、データを「ビルディングブロック」コンポーネントにプルします。
* gatsby-source-filesystemプラグインを使用して、コンピューターのファイルシステムからサイトにデータをプルします。
* ページクエリを作成して、データをページコンポーネントにプルします。

Webブラウザーで、http：// localhost：8000/___graphqlにアクセスします

```
import { Link, useStaticQuery, graphql } from 'gatsby'
```

```js
  const data = useStaticQuery(graphql`
    query {
      site {
        siteMetadata {
          title
        }
      }
    }
  `)
<header>{data.site.siteMetadata.title}</header>
```

https://www.gatsbyjs.com/docs/tutorial/part-4/#queries-in-page-components-create-a-blog-page-with-a-list-of-post-filenames
