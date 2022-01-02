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
# Playing with stuff
