# Filesystem Routing:

# Creating the content and structures
* Create the content:
    * Create Some embedded files
        * src/pages/first/one.md -> "# First - One"
        * src/pages/first/two.md -> "# First - two"
        * src/pages/second/one.md -> "# Second - one"
        * src/pages/second/two.md -> "# Second - two"

* Route to it:
    * Create the routes:
        * first/one
        * first/two
        * second/one
        * second/two

    * Use the 
        * Create the directory
        * src/pages/{MarkdownRemark.parent__(File)__relativeDirectory}/
        * Add the file to the above : {MarkdownRemark.parent__(File)__name}.js    

        * Completed component:
        ```jsx
        import React from "react"
        import { graphql } from "gatsby"

        const ComponentName = ({ data }) => {
            const { html } = data.markdownRemark;

            return (
                <div dangerouslySetInnerHTML={{ __html: html }} />
            );
        }

        export const query = graphql`
        query($id : String!)
        {
            markdownRemark(id : { eq : $id })
            {
                html
            }
        }
        `

        export default ComponentName
        ```

* Check:
    * Go to the 404 page
    * Check that you have the pages:
        * first/one
        * first/two
        * second/one
        * second/two

# Creating links to the new pages

* Create src/pages/first.js
    * first.js
        * src/pages/first.js
        ```jsx
        import * as React from "react"
        import { graphql, Link } from "gatsby"

        export default function Page()
        {
            return (
                <div>
                    This is the page!
                </div>
            );
        }
        ```

    * You can now route to this page via localhost:8000/first

* Get the children and render them out
    * localhost:8000/__grapqhl
    * Create a query that:
        * grabs all markdown pages
        * from src/pages/first

    * Result:
    ```js
    query {
      allFile(filter: {relativeDirectory: {eq: "first"}}) {
        nodes {
          name
        }
      }
    }
    ```

    * Add this to to the component and render out the path:
        * use the "Link" component to link to the file's "name"
        * <Link to={name}> {name} </Link>

* second.js
    * Copy first.js
    * name it second.js
    * Have it render src/pages/second's child files

    * 404 page -> should link to second
    * Second -> one and second -> two are now navigatable
        * Check the contents are for "second" and not "first"

# Create links to the new delegation pages
* Make index.js render links to first and second
    * Clear out index.js
    * Create a list of pages
        * Link to first
        * Link to second

    * Example:
    ```jsx
    import * as React from "react";
    import { Link } from "gatsby";

    export default function Index()
    {
        return (
            <div>
                This is this index page!
                <ul>
                    <li><Link to="first"> First </Link></li>
                    <li><Link to="second"> Second </Link></li>
                </ul>
            </div>
        );
    }
    ```

* Test you can navigate to the four pages
