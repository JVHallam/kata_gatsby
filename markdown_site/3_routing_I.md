# Filesystem Routing:

# Refactoring this exercise:
* Create the content
* Create the content routing page
    * Use the {MarkdownRemark.parent__(File)__relativeDirectory}/{MarkdownRemark.parent__(File)__name}.js    
* Create the first.js page
* Create the index.js page to link to those
    * Link is below
    * Duplicate first.js to be second.js



# Create some routes:
* Create Some embedded files
    * src/pages/first/one.md
    * src/pages/first/two.md
    * src/pages/second/one.md
    * src/pages/second/two.md

* Play with some __graphql routes

* Make the routes:
    * Use the {MarkdownRemark.parent__(File)__relativeDirectory}/{MarkdownRemark.parent__(File)__name}.js    
    * <root>/directory/filename
        * <root>/first/one
        * <root>/first/two
        * <root>/second/one
        * <root>/second/two

* Use the 404 page to route to them:



* Make
    * <root>/first/index.js
    ```jsx
    import React from "react"
    import { graphql } from "gatsby"

    export const query = graphql`
    query MyQuery {
      allFile(filter: {relativeDirectory: {regex: "/first/g"}}) {
        edges {
          node {
            name
          }
        }
      }
    }
    `

    export default function First({ data })
    {
        console.log(data);

        return (
            <div>
                Test
            </div>
        );
    }
    ```

    * Add to the gatsby-config.js - plugins
    ```js
    {
        resolve: 'gatsby-source-filesystem',
        options: {
            "name": "pages",
            "path": "./src/pages/first"
        },
        __key: "pages"
    }
    ```

* Duplicate this for second


-------------------------------------------------------------------------------------------------------------------

* Make the index.js page link to this
import React from "react"
import { Link, graphql } from "gatsby"

export default function Index({ data })
{
    console.log(data);

    const directories = data.allMarkdownRemark.nodes.map(node => node.parent.relativeDirectory);

    const uniqueDirectories = [...new Set(directories)];

    const listElements = uniqueDirectories.map((name,index) => {
        return (
            <li key={index}>
                <Link to={name}>{name}</Link>
            </li>
        );
    });

    return (
        <div>
            <ul>
                { listElements }
            </ul>
        </div>
    );
}

export const query = graphql`
{
  allMarkdownRemark {
    nodes {
      parent {
        ... on File {
          relativeDirectory
        }
      }
    }
  }
}
`


-------------------------------------------------------------------------------------------------------------------

# Generate some embedded rendering:
* Have this
    * {MarkdownRemark.parent__(File)__relativeDirectory}.js
    * Just echo out "PAGE"
    ```jsx
    import React from "react"
    import { graphql } from "gatsby"

    export default function Template({
        data,
    }) {
        return (
            <div> This should be first and second </div>
        )
    }
    ```
    * Navigate to the 404
    * /first and /second should now be routes - no more additional routes
    * /first/one and so on, should still work

* Have /first and /second render out their children respectively - and only their children
    * Conver that route into a query
    * Make it on the page
    * Get the relativeDirectory, that's being used to render the page
    * Echo it out, on the page

------------------------------------------------------------------------------------------------------------

//All files, is the children of the directory
export const pageQuery = graphql`
query MyQuery {
  allFile(filter: {relativeDirectory: {eq: "first"}}) {
    edges {
      node {
        id
        name
      }
    }
  }
}
`

//Directory is the directory i'm trying to render
query MyQuery {
  allFile(filter: {relativeDirectory: {eq: "first"}}) {
    edges {
      node {
        id
        name
      }
    }
  }
  directory(name: {eq: "first"}) {
    id
    name
  }
}

//Render the directory's name, on the page
props.data.directory.name

//Now add in the relativeDirectory part


------------------------------------------------------------------------------------------------------------

* src/pages/first/index.js
    * Renders out links to first/one and two

* src/pages/second/index.js
    * Renders out links to second/one and two

* Route to them
    * home index.js
    * routes to the above

# Auto route generation
* Can i combine the above two into one file?
