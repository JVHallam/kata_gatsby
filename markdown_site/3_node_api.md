# Gatsby Node API
This kata is focused on using the Gatsby Node API.
This way, we can define dynamic routes, that are built using templates.

## Hello World!
* Reporter and hooks: 
    * Create File : ./gatsby-node.js
    * Create the onPreInit Hook

* Echo "hello world"
    ```js
    exports.onPreInit = ({ reporter }) => {
      reporter.info(`Hello World!`);
    }
    ```

* Test it:
    * gatsby develop -> info at the top, should show the "hello world"

## Create a page:
* Create a page:
    * Create Directory : src/templates

    * Create the hello-world component:
        * src/templates/hello-world.js
        ```js
        import React from "react";

        export default function Page()
        {
            return (
                <div>
                    This is my sick template
                </div>
            );
        }
        ```

* Render it from gatsby-node.js
    * Require the path module
    ```js
    const path = require("path")
    ```

    * use the createPage hook
    ```js
    exports.createPages = async ({ graphql, actions }) => {
    }
    ```

    * Load in the template component
    ```js
    exports.createPages = async ({ graphql, actions }) => {
        const { createPage } = actions;
        const helloWorldComponent = path.resolve("src/templates/hello-world.js");
    }
    ```

    * Create the page using the template
    ```js
    exports.createPages = async ({ graphql, actions }) => {

        ...

        createPage({
            path: "hello-world",
            component: helloWorldComponent,
        });
    }
    ```

* Navigate to it
    * Restart gatsby dev server
    * Navigate  : /t/404
    * Test      : /hello-world should appear

## Common Step : Create the markdown content

## Render out the markdown pages
* Create the routes:
    * Query for the pages' file names
    ```js
    exports.createPages = async ({ graphql, actions }) => {
        ...

        const result = await graphql(`
            query MyQuery {
                allMarkdownRemark {
                    nodes {
                        parent {
                            ... on File {
                                relativeDirectory
                                name
                            }
                        }
                    }
                }
            }
        `);
    }
    ```

    * Render a page per node - render out the helloWorldComponent from earlier
        * Path should be directory/filename
    ```js
    exports.createPages = async ({ graphql, actions }) => {
        ...
        //After the query

        result.data.allMarkdownRemark.nodes.forEach(node => {
            const { relativeDirectory, name } = node.parent;

            createPage({
                path: `${relativeDirectory}/${name}`,
                component: helloWorldComponent,
            })
        })
    }
    ```

* Extend hello-world.js to render out the markdown
    * gatsby-node.js
        * Add "fileAbsolutePath" to the "nodes" part of the query
        ```js
        const result = await graphql(`
            query MyQuery {
                allMarkdownRemark {
                    nodes {
                        fileAbsolutePath

                        parent {
                            ... on File {
                                relativeDirectory
                                name
                            }
                        }
                    }
                }
            }
        `);
        ```

        * Pass that in as context to the create page
        ```js
        result.data.allMarkdownRemark.nodes.forEach(node => {
            ...

            const { fileAbsolutePath } = node;

            createPage({
                ...

                context : {
                    fileAbsolutePath
                }
            });
        });
        ```

    * Update the page
        * Use the query to get the html
        ```js
        export const query = graphql`
        query MyQuery($fileAbsolutePath : String!){
          markdownRemark(fileAbsolutePath: {eq: $fileAbsolutePath}) {
            html
          }
        }
        `;
        ```

    * Render out the innerHtml
        * Break the below down:
        ```js
        export default function Page({ data })
        {
            const { html } = data.markdownRemark;

            return (
                <div dangerouslySetInnerHTML={{__html : html}}></div>
            );
        }
        ```

* Check the pages exist:
    * restart the dev server
    * 404 page
    * Do these pages exist:
        * markdown/first/one
        * markdown/first/two
        * markdown/second/one
        * markdown/second/two

    * Do they have contents that match the markdown files?
