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

* Dependancies:
    ```sh
    npm install gatsby-plugin-mdx @mdx-js/mdx @mdx-js/react
    ```
* Update : gatsby-plugin.js
    * Add to the plugins array:
    * "gatsby-plugin-mdx"


* Front matter:
    ```md
    ---
    title : this is the title
    description: Anything placed at the top of the MDX files, in this format, is front matter
    ---
    ```

* Querying frontmatter
    ```graphQL
    query MyQuery {
      allMdx {
        nodes {
          frontmatter {
            date(formatString: "MMMM D, YYYY")
          }
        }
      }
    }
    ```
----------------------------------------------------------------------------

# Graphiql
* THERE'S A FUCKING GRAPHIQL IDE BUILT IN
    * localhost:8000/__graphiql

----------------------------------------------------------------------------
# Routing
* Routing is done by the "slug" attribute
* Slugs are added automatically, as the files name, by gatsby

* IT might be the routing!

* It's called Collection routing
    {Product.name}.js

{MarkdownRemark.parent__(File)__name}.js
    * Parent node
    * File
    * Name

* Can i add directory to this?

* The routing will handle the dynamic page generation, this all centers around the Queries

* THE ROUTING SOLUTION I FOUND:
    * \pages\{MarkdownRemark.parent__(File)__relativeDirectory}\{MarkdownRemark.parent__(File)__name}.js
    * Results in /markdown-pages/blog/first.md -> route -> /blog/first


* The files are kept in markdown/blog, but the filesystem is pointing to the directory above it
    {
      resolve: "gatsby-source-filesystem",
      options: {
        name: `blog`,
        path: `${__dirname}/src/markdown/`,
      }
    }


----------------------------------------------------------------------------
# How can i modify the markdown files?

* Read a value, in the page

* Mutate it

* Insert another page's values
    * That would require a pair of files being passed in, via graphql
    * Could i get 2 files?

* This retrieves the file and a named file - This is how you'd find get common steps whilst having the file
```graphQL
query MyQuery {
  markdownRemark{
    html
    headings {
      id
      value
    }
  }
  allMarkdownRemark(filter: {fileAbsolutePath: {regex: "/third/"}}) {
    group(field: id) {
      nodes {
        html
      }
    }
  }
}
```

* JSX for loading a pair of pages:
```jsx
import React from "react"
import { graphql } from "gatsby"

export default function Template({
  data, // this prop will be injected by the GraphQL query below.
}) {
    const { html } = data.markdownRemark;
    const extraHtml = data.allMarkdownRemark.group[0].nodes[0].html;
    console.log( extraHtml );

  return (
    <div className="blog-post-container">
      <div className="blog-post">
        <div
          className="blog-post-content"
          dangerouslySetInnerHTML={{ __html: html }}
        />
      </div>
      <hr />
      <div dangerouslySetInnerHTML={{ __html: extraHtml }}>
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
    allMarkdownRemark(filter: {fileAbsolutePath: {regex: "/common_steps/"}}) {
        group(field: id) {
            nodes {
                html
                headings {
                    id
                    value
                }
                internal {
                    content
                }
            }
        }
    }
}
`
```

* You could write the JS code from here.
    * Any H1 tags that start with "Common Steps:"
    * Strip out the common steps bit and white space
    * Find the node in the common steps elements that matches
    * Replace
