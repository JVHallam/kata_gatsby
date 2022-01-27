# Gatsby Node API

# Pre-reqs:
* Using the files
    * /first/one.md
    * /first/two.md
    * /second/one.md
    * /second/two.md

# Hello World!
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
    * Create the hello-world component:
        * src/pages/template/hello-world.js
        ```js
        import * as React from "react";

        export default function Page()
        {
            return (
                <div>
                    This is my dumb page
                </div>
            );
        }
        ```

    * Render it from gatsby-node.js
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

# Render out the markdown pages
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
            * /first/one
            * /first/two
            * /second/one
            * /second/two

* Render out the expected contents
    * Pass in context ( relativeDirectory + name + id + stuff )

    * Use that and the code from kata 2 to render out the page

# Render out the tree to those:
* create the template for first / second
* pass it the context it's name -> ( first, second )
    * Get this from the directory names maybe?
* render out it's name
* use a grapqhl query on the page, to render out the above

# Route to the above from index
* Oof, how to do this best? - Directory names maybe?



-----------------------------------------------------
* Focus on doing node stuff, with routing too
    * This focuses on the gatsby-node.js

* Maybe make a plugin?! OOOOOOOOOOOH!

# Do some routing with CreatePages

index/
    /repos
        /katas
            /kata.md

* So i'll need 3 route creators
    * repos
    * katas
    * katas

* I'll then need to link between the 3

* YOU CAN PASS CONTEXT TO COMPONENTS! You might not even have to re-query
    * Go on faggot, i bet you could use a recursive function for this
    * allFile -> Get all the files in the katas directory
    * Recursively use createPage

# Some more interesting node apis:
* createResolvers - Extends grapqhl
* createSchemaCustomization - full on extras to graphql schemas
* onCreateNode -> On every node, could do something funny
* onCreatePage -> If you want to make changes to every page, for whatever reason
* resolvableExtensions -> Change which extensions gatsby supports
