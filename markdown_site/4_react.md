# React Stuff:
This kata covers the idea of using react to:
* Create a global site layout
* Store state in the react components
* Store state between page refreshes

## Getting a Layout component
* Create an example page
    * src/pages/example.js
    ```js
    import * as React from "react";

    export default function Page()
    {
        return (
            <div>
                This is the example page
            </div>
        );
    }
    ```

* Create the layout
    * Create the src/components/layout.js
    * Create the component
    ```
    import React from "react";

    export default function Layout({ children })
    {
        return (
            <div>
                <h2>This is the layout component</h2>
                <div>
                    { children }
                </div>
            </div>
        );
    }
    ```

* Use it
    * edit : src/pages/example.js
    * Import The layout component
    * Change the div to be layout
    ```
    return (
        <Layout>
            This is the example page
        </Layout>
    );
    ```

* Test it:
    * Navigate to /example
    * check that the super bland page is now wrapped by the layout component

## State management
* Create the test page
    * create : src/pages/state_example.js
    * Create a component that renders it's state
        * Create a toggleState variable
        ```js
        export default function Page()
        {
            const state = true;
        ```

        * Create a variable that turns that into a message
        ```js
        const stateMessage = `State : ${state}`;
        ```

        * Render that out

    * Create a button
        * Add the button to the html
        ```jsx
        <button> Hello </button>
        ```

        * OnClick fuction => console.log("click");
        ```jsx
        <button onClick={() => console.log("click")}> Hello </button>
        ```

* Add state and toggle it with the button:
    * Create a react hook
        * Import useState
        ```jsx
        import { useState } from "react";
        ```

        * Create a react hook
        ```
        const [state, setState] = useState(true);
        ```

        * Use that instead of the hardcoded state

    * Make the button toggle the state
        * onClick={() => setState(!state)}

* Test:
    * Click the button
    * The toggle toggles

## Custom React Hooks - That persist the state on page change
Note:  The toggle state doesn't persist on page refresh - let's fix that

* Create a custom react hook function for our toggle
    * create file : src/useToggle.js
    * export the "useToggle" function
    * return from it useState(true);
    * Use the above in example.js component

* Make it persist on page refresh
    * Get the initial state from localStorage
    ```js
    localStorage.getItem("toggleState");
    ```

    * JSON.parse(the returned value)

    * Check if it's null

    * on setState, call localStorage.setItem("toggleState", state)
        * Create a function - setPersistentState
        * it calls setState(state)
        * it also calls localStorage.setItem

    * return [state, setPersistentState]

    * Update the original component to use toggleStateFunction() vs setState()

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

* Use it on another page:
    * Create file : src/pages/stateCheck.js
    * use the toggle here too 
    * Render out it's state
    * Example:
    ```jsx
    import React from "react";
    import useToggle from "../useToggle.js";

    export default function Page()
    {
        const [state] = useToggle();

        const stateMessage = `State : ${state}`;

        return (
            <div>
                { stateMessage } 
            </div>
        );
    }
    ```

--------------------------------------------------
Finished example of the above:
