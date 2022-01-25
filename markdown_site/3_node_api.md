# Gatsby Node API
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
