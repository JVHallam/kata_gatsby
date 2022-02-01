# We're doing REACT stuff this time around

# Getting a Layout component
* Create an example page
    * src/pages/example.js
    ```js
    import * as React from "react";
    import { graphql } from "gatsby";

    export default function Page()
    {
        return (
            <div>
                This is a super bland page
            </div>
        );
    }
    ```

* Create the layout
    * Create the src/components/layout.js
    ```
    import React from "react";

    export default function Layout({ children })
    {
        return (
                <div>
                    This is the layout component
                    <div>
                        { children }
                    </div>
                </div>
        );
    }
    ```

* Use it
    * src/pages/example.js
    ```
    return (
        <Layout>
            <div>
                This is a super bland page
            </div>
        </Layout>
    );
    ```

* Test it:
    * Navigate to /example
    * check that the super bland page is now wrapped by the layout component

# State management
* Create a react hook, for a toggle
```js
```

* Create a custom react hook function

* Create an element that hides on stuff
* Add in stuff to persist the state between refreshes
* Move that to another file

# Debugging:
* gatsby develop --inspect
* chrome://inspect
* setup sources
* Hit a breakpoint on the above toggle
