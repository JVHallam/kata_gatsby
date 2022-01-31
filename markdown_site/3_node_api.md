# Gatsby Node API

## Common Step : Create the markdown content

## Hello World!
* Reporter and hooks: 
    * create "gatsby-node.js"
    * Create the onPreInit Hook
    * Echo "hello world"
        ```js
        exports.onPreInit = ({ reporter }) => {
          reporter.info(`Hello World!`);
        }
        ```
    * gatsby develop -> info at the top, should show the "hello world"

* Create a page:
    * Create Directory : src/pages/templates
    * Create the hello-world component:
        * src/pages/templates/hello-world.js
        ```js
        import * as React from "react";

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
        * This needs to be more indepth about adding stuff
    ```js
    const path = require(`path`)
    exports.createPages = async ({ graphql, actions }) => {
        const { createPage } = actions;
        const helloWorldComponent = path.resolve("src/templates/hello-world.js");

        createPage({
            path: "hello-world",
            component: helloWorldComponent,
        });
    }
    ```

* Navigate to it
    * Restart gatsby dev server
    * 404
    * See if it's there

## Render out the markdown pages
* Create the routes:
    * Query for the pages
    ```
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

    ```

    * Render a page per node - render out the helloWorldComponent from earlier
    ```js
    result.data.allMarkdownRemark.nodes.forEach(node => {
        const { relativeDirectory, name } = node.parent;

        createPage({
            path: `${relativeDirectory}/${name}`,
            component: helloWorldComponent,
        })
    })
    ```

    * Check the pages exist:
        * restart the dev server
        * 404 page
        * Do these pages exist:
            * markdown/first/one
            * markdown/first/two
            * markdown/second/one
            * markdown/second/two

## Render out the expected contents

---
This section needs to be broken down into sections
---


* Add id to the routing query
    ```
    const result = await graphql(`
        query MyQuery {
            allMarkdownRemark {
                nodes {
                    id
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

* Pass that in as context:
```js
const { id } = node;

createPage({
    path: `${relativeDirectory}/${name}`,
    component: helloWorldComponent,
    context : {
        id
    }
});
```

* Use that in the template query to get the markdown html
```js
export const query = graphql`
query($id : String!)
{
    markdownRemark(id: { eq : $id })
    {
        html
    }
}
`;
```

* extract the html from the props
    ```js
    export default function Page({ data }){
        const { html } = data.markdownRemark;
    ```

* Render that out
```jsx
<div dangerouslySetInnerHTML={{ __html :  html }} />
```

* test:
    * Navigate the 404's again
    * Check that each page renders out it's contents

# Update index 
* Use the same query as the routing
    ```
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
    ```

* Map all nodes and render them out
    * Map each one to a Link
    * the Link to=./relativeDirectory/name

* Render them out
    * <ul> { links } </ul>
