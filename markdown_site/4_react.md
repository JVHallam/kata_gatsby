# React Stuff:
## Getting a Layout component
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

## State management
* Create a component that renders it's state
    * Create a toggleState variable
    * Create a variable that turns that into a message
    * Render that out

* Create a button
    * Create a button
    * OnClick fuction => console.log("click");

* Create a react hook
    * import { useState } from "react";
    * const [toggleState, setToggle] = useState(true);

* Make the button toggle the state
    * onClick={() => setToggle(!toggleState)}

* Test:
    * Click the button
    * The toggle toggles

## Custom React Hooks - That persist the state on page change
* Note:  The toggle state doesn't persist on page refresh

* Create a custom react hook function
    * create file : src/useToggle
    * return from it useState(true);
    * Use the above in example.js component

* Make it persist on page refresh
    * localStorage.getItem("toggleState");
    * JSON.parse(the returned value)
    * Check if it's null

    * on toggle, call localStorage.setItem("toggleState", state)

* Isolate that code, into the custom hook
    * return [state, toggleStateFunction] from that function

* Use it on another page:
    * Create file : src/pages/stateCheck.js
    * use the toggle here too 
    * Render out it's state

--------------------------------------------------
Finished example of the above:
```jsx
import { useState } from "react";

export default function useToggle()
{
    const storedState = JSON.parse(localStorage.getItem("toggleState"));
    const initialState = storedState == null ? 'true' : storedState;

    const [state, setState] = useState(initialState);

    const toggleStateFunction = function()
    {
        const newState = !state;
        setState(newState);
        localStorage.setItem("toggleState", newState);
    }

    return [state, toggleStateFunction];
}
```
