

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


# Create some MDX blog posts

サイトでは、各ブログ投稿をプロジェクトのフォルダ内に個別のファイルとして保存します。

* プロジェクトフォルダの最上位にblogという名前の新しいディレクトリを作成します。
* 投稿ごとに1つです。拡張子が.mdxである限り、名前は関係ありません。 
* ローカルファイルシステムにいくつかの投稿が保存されたので、次にそれらのファイルをGatsbyデータレイヤーにプルします。そのためには、gatsby-source-filesystemというプラグインを使用します。

gatsby-config.jsファイルでgatsby-source-filesystemを構成します。 gatsby-source-filesystemにはいくつかの追加の構成オプションが必要なため、文字列の代わりに構成オブジェクトを使用します。 以下のコード例は、ブログディレクトリからファイルを「調達」する方法（つまり、ファイルをデータレイヤーに追加する方法）を示しています。

```
module.exports = {
  siteMetadata: {
    title: "My First Gatsby Site",
  },
  plugins: [
    "gatsby-plugin-image",
    "gatsby-plugin-sharp",
    {
      resolve: "gatsby-source-filesystem",
      options: {
        name: `blog`,
        path: `${__dirname}/blog`,
      }
    },
  ],
};
```

ファイル名取得
```
query MyQuery {
  allFile {
    nodes {
      name
    }
  }
}
```

# ページクエリを使用して、投稿ファイル名のリストをブログページにプルします

```
const BlogPage = ({ data }) => {
  return (
    <Layout pageTitle="My Blog Posts">
      <ul>
        {
          data.allFile.nodes.map(node => (
            <li key={node.name}>
              {node.name}
            </li>
          ))
        }
      </ul>
    </Layout>
  )
}
export const query = graphql`
  query {
    allFile {
      nodes {
        name
      }
    }
  }
`
```

# サイトでデータを使用するための一般的なプロセス

* ソースプラグインを追加して、GraphQLデータレイヤーにデータを追加します。
* GraphiQLを使用して、データレイヤーから必要なデータで応答するクエリを設計します。
* Add the query into your component.
  * 「ビルディングブロック」コンポーネントにはuseStaticQueryを使用します。
* コンポーネントの応答からのデータを使用します。


# Transform Data to Use MDX

トランスフォーマープラグインは、ノードをあるタイプから別のタイプに変換します。 たとえば、gatsby-plugin-mdxプラグインは、拡張子が.mdxのファイルノードをMDXノードに変換します。MDXノードには、GraphQLを使用してクエリできるさまざまなフィールドのセットがあります。 Transformerプラグインを使用すると、ソースプラグインによって作成されたノードの生データを操作できるため、必要な構造または形式にデータを取り込むことができます。

# gatsby-plugin-mdx

ブログページに投稿をレンダリングするには、いくつかの異なる手順を実行します。
1. gatsby-plugin-mdxトランスフォーマープラグインとその依存関係をインストールして構成します。
1. gatsby-plugin-mdxのallMdxフィールドを使用する
1. gatsby-plugin-mdxのMDXRendererコンポーネントを使用して、ブログページのJSXで投稿のMDXコンテンツをレンダリングします。

```
npm install gatsby-plugin-mdx @mdx-js/mdx@v1 @mdx-js/react@v1
```

gatsyby-config
```
  plugins: [
    "gatsby-plugin-image",
    "gatsby-plugin-sharp",
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `blog`,
        path: `${__dirname}/blog/`,
      },
    },
    "gatsby-plugin-mdx", // add
  ],
```

# under scoree3つ

[並べ替え]で、[フィールド]引数を確認し、ドロップダウンを使用して、データノードを並べ替えるフィールドを選択します。この場合、それはfrontmatter ___ date（3つのアンダースコア付き）になります。


```
query MyQuery {
  allMdx(sort: {fields: frontmatter___date, order: DESC}) {
    nodes {
      frontmatter {
        date(formatString: "MMMM D, YYYY")
        title
      }
      id
      body
    }
  }
}
```


MDXブログ投稿の実際のコンテンツをレンダリングすることです。これを行うには、MDXRendererと呼ばれるgatsby-plugin-mdxのコンポーネントを使用する必要があります

```
<MDXRenderer>
  {node.body}
</MDXRenderer>
```

# プログラムでページを作成する

## GatsbyのファイルシステムルートAPIを使用して動的に新しいルートを作成する

ただし、1つのページコンポーネントを使用して複数のページを作成することもできます。 すべてのコンテンツをハードコーディングする代わりに、ページの基本構造の概要を示すテンプレートを作成すると、Gatsbyはビルド時に各ページの特定のデータを動的に追加できます。 これを行うには、GatsbyのファイルシステムルートAPIを使用します。これにより、ページファイルに特別な構文で名前を付けることにより、ルートを動的に作成できます。

## ブログ投稿ページテンプレートを作成する

src/pages/{mdx.slug}.js ファイル作成

src/page/blog/{mdx.slug}.js 設置することで

```js
import * as React from 'react'
import { graphql } from 'gatsby'
import Layout from '../../components/layout'
import { MDXRenderer } from 'gatsby-plugin-mdx'

const BlogPost = ({ data }) => {
  return (
    <Layout pageTitle={data.mdx.frontmatter.title}>
      <p>{data.mdx.frontmatter.date}</p>
      <MDXRenderer>
        {data.mdx.body}
      </MDXRenderer>
    </Layout>
  )
}

export const query = graphql`
  query ($id: String) {
    mdx(id: {eq: $id}) {
      frontmatter {
        title
        date(formatString: "MMMM D, YYYY")
      }
      body
    }
  }
`

export default BlogPost
```

## link

```js
<h2>
  <Link to={`/blog/${node.slug}`}>
    {node.frontmatter.title}
  </Link>
</h2>
```

## まとめ

* ファイルシステムルートは、src内のファイルでのみ機能します
* ファイルに{nodeType.field} .jsという名前を付けます。ここで、nodeTypeはページを作成するノードのタイプであり、fieldはで使用するノードタイプのデータフィールドです。そのページのURL。
* クエリ変数を使用すると、同じGraphQLクエリに異なるデータ値を渡すことができます。これらをフィールド引数と組み合わせて、特定のノードに関するデータのみを取得できます。
* クエリ変数は、ページクエリでのみ使用できます。

# データから動的画像を追加する

GatsbyImageコンポーネントを使用して、データから動的に画像を作成します。

mdxに画像
example
```
hero_image: the relative path to the hero image file for that post
hero_image_alt: a short description of the image, to be used as alternative text for screen readers or in case the image doesn’t load correctly
hero_image_credit_text: the text to display to give the photographer credit for the hero image
hero_image_credit_link: a link to the page where your hero image was downloaded from
```
```js
title: "My First Post"
date: "2021-07-23"
hero_image: "./christopher-ayme-ocZ-_Y7-Ptg-unsplash.jpg"
hero_image_alt: "A gray pitbull relaxing on the sidewalk with its tongue hanging out"
hero_image_credit_text: "Christopher Ayme"
hero_image_credit_link: "https://unsplash.com/photos/ocZ-_Y7-Ptg"
```

* GatsbyImageコンポーネントを使用するには、gatsby-transformer-sharpトランスフォーマープラグインをサイトに追加する必要があります。

## Add hero image using GatsbyImage component

```js
export const query = graphql`
  query($id: String) {
    mdx(id: {eq: $id}) {
      body
      frontmatter {
        title
        date(formatString: "MMMM DD, YYYY")
        hero_image_alt
        hero_image_credit_link
        hero_image_credit_text
        hero_image {
          childImageSharp {
            gatsbyImageData
          }
        }
      }
    }
  }
`
```