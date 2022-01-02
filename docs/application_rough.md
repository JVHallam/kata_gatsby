------------------------------------------------------------------------------------
# Setting up the Markdown site:

* gatsby new
    * With MDX support

* npm install --save gatsby-transformer-remark

* src\pages\{MarkdownRemark.frontmatter__slug}.js
```jsx
import React from "react"
import { graphql } from "gatsby"

export default function Template({
  data, // this prop will be injected by the GraphQL query below.
}) {
  const { markdownRemark } = data // data.markdownRemark holds your post data
  const { frontmatter, html } = markdownRemark
  return (
    <div className="blog-post-container">
      <div className="blog-post">
        <h1>{frontmatter.title}</h1>
        <div
          className="blog-post-content"
          dangerouslySetInnerHTML={{ __html: html }}
        />
      </div>
    </div>
  )
}

export const pageQuery = graphql`
  query($id: String!) {
    markdownRemark(id: { eq: $id }) {
      html
      frontmatter {
        slug
        title
      }
    }
  }
`
```

* Create directory : src/pages/markdown

* Create : src/pages/markdown/test.md
    ```md
    ---
    slug: /markdown/page
    title: Jakes
    ---

    # This is a page!
    ```

* Update : gatsby-config.js
    * Add to the plugins array:
        ```js
        "gatsby-transformer-remark",
        {
          resolve: `gatsby-source-filesystem`,
          options: {
            name: `markdown-pages`,
            path: `${__dirname}/src/markdown/`
          }
        }
        ```

* Navigate : http://localhost:8000/markdown/page/

-------------------------------------------------------------------------------
# Adding a gatsby.js page


* Create the page:

    * src/pages/test.js
    * Add the code
    ```jsx
    import React from "react"

    export default function Template({
      data
    }) {
      return (
        <div>
          This is a test!
        </div>
      )
    }
    ```

    * Navigate : (To the page)[localhost:8000/test]

* Do this, then get back to me
    * https://www.gatsbyjs.com/docs/tutorial/part-4/#queries-in-page-components-create-a-blog-page-with-a-list-of-post-filenames

-------------------------------------------------------------------------------
# Something

* src/pages/blog.js
```jsx
import * as React from 'react'
const BlogPage = () => {
  return (
    <p>My cool posts will go in here</p>
  )
}
export default BlogPage
```

* src/index.js
    * Link to your new page
    ```html
    <a href="/blog."> The blog </a>
    ```

* Create a number of blog files
    * src/markdown/blog/first.md
    * src/markdown/blog/second.md
    * src/markdown/blog/third.md

* Add it to the gatsby-config.js, plugins
    ```
    {
      resolve: "gatsby-source-filesystem",
      options: {
        name: `blog`,
        path: `${__dirname}/src/markdown/blog`,
      }
    }
    ```

* Using Graphql
    * import graphql and use graphql
    ```js
    import { graphql } from 'gatsby'

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

* Enable the result to be passed to your element
    * Add "data" to props
    ```jsx
    const BlogPage = ({ data }) => {
      return (
        <p>My cool posts will go in here</p>
      )
    }
    ```

* Final Result
    ```jsx
    import * as React from 'react'
    import { graphql } from 'gatsby'

    const BlogPage = ({ data }) => {
      return (
        <React.Fragment>
          <p>My cool posts will go in here</p>
          <ul>
          {
            data.allFile.nodes.map(node => (
              <li key={node.name}>
                {node.name}
              </li>
            ))
          }
          </ul>
        </React.Fragment>
      )
    }
    export default BlogPage

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

-------------------------------------------------------------------------------
# Graphiql

* This could be a cool little cli tool to test out graphql queries

-------------------------------------------------------------------------------
# State
* This is a react question, not a gatsby question.
    * You're going to need to use state, lifecycle and localStorage

-------------------------------------------------------------------------------
# Next

* This will be super useful: https://www.gatsbyjs.com/docs/tutorial/part-5/
    * This is for transforming file nodes
    * DO THIS INSTEAD OF MDX
