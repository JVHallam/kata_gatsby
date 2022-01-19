# Hello World!

## Setting up

* Setup the site
```ps1
npm install -g gatsby-cli

gatsby new
# Name : kata
# location : default
# CMS : No
# Styling : No
# Additional stuff : Add these both
    # * Add markdown Support (Without MDX)
    # * Add markdown and MDX support 
# Should we do this : y

gatsby develop
```

## Introduce markdown with routing
* Create the src/pages/markdown directory
    * Put in some markdown pages
        * first.md
            * Give it the heading "# First Page"
            * Give it the paragraph "Hey, this is the first page!"
        * second.md
            * Give it the heading "# Second Page"
            * Give it the paragraph "Hey, this is the second page!"

    * Give them a #title and a paragraph of text

    * These are now the content

* Routing  
    * Create the file:
        * src\pages\{MarkdownRemark.parent__(File)__name}.js

    * Give it the code:
    ```jsx
    import React from "react"
    import { graphql } from "gatsby"

    export default function Template({
      data,
    }) {
      const { markdownRemark } = data;
      const { html } = markdownRemark;
      return (
        <div
          dangerouslySetInnerHTML={{ __html: html }}
        />
      )
    }

    export const pageQuery = graphql`
      query($id: String!) {
        markdownRemark(id: { eq: $id }) {
          html
        }
      }
    `
    ```

* Navigate to the markdown pages!
    * navigate to the developers 404 page:
        * localhost:8000/f/f
    * Select first and second
    * You should now see the contents of the .md files

# Linking To these pages from the home page:
* Write the graphql query:
    * use localhost:8000/__graphql
    * Write a query that:
        * Gets all markdown remark pages:
            * File name
            * File heading

* Make the index.js file 
    * Clear out index.js

    * Add the graphql query
        * import { graphql } from "gatsby"
        * Write and export the query

    * Add the element
        * import * as React from "react";
        * export default function Index()
        * Return <div> Hello world! </div>

* Passing props from the query:
    * Get the files from the props:
        * Make the function take the props
        * extract "data" from props
        * extract allMarkdownRemark from the data
        * extract the nodes from the allMarkdownRemark

    * Render out those nodes into a list:
        * the file name is node.parent.name => fileName
        * the heading is node.heading[0].value
        * map over the nodes
        * Use the heading as the anchor's value and the fileName as the href path
        * Render them out in a list


* Finished example:
    ```jsx
    export default function Index({data})
    {
        const { allMarkdownRemark } = data;
        const { nodes } = allMarkdownRemark;

        const mappedNodes = nodes.map((node, index) => {
            const fileName = node.parent.name;
            const heading = node.headings[0].value;

            return (
                <li key={index}>
                    <a href={fileName}>
                        heading : {heading}
                    </a>
                </li>
            );
        });

        return (
            <div>
                <ul>
                    { mappedNodes }
                </ul>
            </div>
        );
    }
    ```
