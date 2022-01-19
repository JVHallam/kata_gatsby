* Alternative routing:
    * If you want more complex queries, you have to use the gatsby-node.js file
    * gatsby-node.js will run on start up
    * Your routes can be defined in here too
    * You can use the more complex graphql queries too
    * Filtering out routes (docs) - using graphql

* Plugins:
    * Do plugin stuff

* I could have a quick section on Some React stuff
    * Layout Components:
        * src/components/layout.js
        * https://www.gatsbyjs.com/docs/how-to/routing/layout-components/ 
        * Again, wrap your pages in a layout component
        * combine it with https://www.gatsbyjs.com/docs/reference/config-files/gatsby-ssr/#wrapPageElement
        * That auto wraps your page with content

    * State persistance and toggles
        * This would be a react part
        * Saving state
            * Retrieving state
            * Very basic. I'm looking to save a single boolean
        * A toggle to hide / show code examples
