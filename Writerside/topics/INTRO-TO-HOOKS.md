# INTRO TO HOOKS

3. Intro to Hooks﻿
   Last modified: 13 September 2024
   5 types of hooks
   What are Hooks?﻿
   Hooks are a feature introduced in React 16.8 that allow you to use state and other React features in functional components, which were previously only possible in class components. Hooks provide a more direct API to the React concepts you’re already familiar with, like state management, lifecycle methods, and context.

Why Hooks?﻿
Before Hooks, managing state and lifecycle methods in React components required the use of class components. This led to some challenges:

Code Reusability: Logic for things like fetching data had to be duplicated across multiple components or extracted into higher-order components (HOCs) or render props, leading to complicated patterns.

Complex Components: Class components often grew complex, as stateful logic and side effects were handled within lifecycle methods like componentDidMount, componentDidUpdate, and componentWillUnmount.

No Logic Sharing: Without hooks, it was hard to share logic across components without restructuring components or using patterns like HOCs, which can make the component tree more complicated.

Hooks address these issues by allowing you to:

Use state and other React features in functional components.

Reuse logic across components via custom hooks.

Simplify complex components by splitting related logic into smaller functions.

Basic Hooks:﻿
useState: Manages state in a functional component.

import React, { useState } from 'react';

const Counter: React.FC = () => {
const [count, setCount] = useState<number>(0);

    return (
        <div>
            <p>Count: {count}</p>
            <button onClick={() => setCount(count + 1)}>Increment</button>
        </div>
    );
};

export default Counter;
useEffect: Manages side effects, such as data fetching or manually updating the DOM.

import React, { useState, useEffect } from 'react';

const DataFetcher: React.FC = () => {
const [data, setData] = useState<string | null>(null);

    useEffect(() => {
        const fetchData = async () => {
            const response = await fetch('https://api.example.com/data');
            const result = await response.json();
            setData(result.data);
        };

        fetchData();
    }, []); // Empty dependency array means this effect runs once on mount

    return <div>Data: {data}</div>;
};

export default DataFetcher;
useContext: Allows you to access React's context API within functional components.

import React, { useContext } from 'react';

const ThemeContext = React.createContext('light');

const ThemeToggler: React.FC = () => {
const theme = useContext(ThemeContext);

    return <div>Current Theme: {theme}</div>;
};

export default ThemeToggler;
Rules of Hooks﻿
To ensure that Hooks work as expected and to maintain the reliability of your component logic, React enforces a set of rules known as the Rules of Hooks.

1. Only Call Hooks at the Top Level﻿
   Hooks should only be called at the top level of your functional component or custom hook. This means:

Do not call Hooks inside loops, conditions, or nested functions.

Always use Hooks in the same order each time your component renders.

Why?﻿
React relies on the order of Hook calls to associate them with the correct useState and useEffect calls. If you violate this rule, your component's state or effect logic may become unpredictable.

Example of Incorrect Usage:﻿
import React, { useState, useEffect } from 'react';

const ConditionalHook: React.FC = () => {
const [isVisible, setIsVisible] = useState<boolean>(true);

    if (isVisible) {
        // This is incorrect! Hooks should not be inside conditions.
        useEffect(() => {
            console.log('This effect may cause problems.');
        }, []);
    }

    return (
        <div>
            <button onClick={() => setIsVisible(!isVisible)}>Toggle Visibility</button>
            {isVisible && <p>Visible Content</p>}
        </div>
    );
};

export default ConditionalHook;
2. Only Call Hooks from React Functions﻿
   Hooks should only be called from:

React functional components.

Custom Hooks (functions that start with use and follow the Rules of Hooks).

Why?﻿
This rule ensures that all stateful logic is associated with a component or a custom Hook. Calling Hooks outside of a component or a custom Hook would break the relationship between your state and the component lifecycle.

Example of Correct Usage:﻿
import React, { useState } from 'react';

// Custom Hook that follows the Rules of Hooks
const useToggle = (initialValue: boolean): [boolean, () => void] => {
const [value, setValue] = useState(initialValue);
const toggleValue = () => setValue(!value);

    return [value, toggleValue];
};

const ToggleComponent: React.FC = () => {
const [isToggled, toggle] = useToggle(false);

    return (
        <div>
            <p>{isToggled ? 'ON' : 'OFF'}</p>
            <button onClick={toggle}>Toggle</button>
        </div>
    );
};

export default ToggleComponent;
Additional Best Practices for Using Hooks﻿
Use useEffect Wisely: Avoid using useEffect for tasks that can be handled without side effects, like transforming data for display. This helps reduce unnecessary renders and potential performance issues.

Use Custom Hooks for Reusability: If you find yourself duplicating Hook logic across components, extract that logic into a custom Hook. This keeps your components clean and makes the logic reusable.