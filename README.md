# unit component testing guid

### KEY POINTS

- [test case templarte](https://github.com/suvel/jest_notes#test-case-template)
- [mocking popular lib functions and hooks](https://github.com/suvel/jest_notes#mocking-popular-lib-functions-and-hooks)
- [mocking exports](https://github.com/suvel/jest_notes#mocking-exports)
- [finding elements](https://github.com/suvel/jest_notes#mocking-exports)
- [narrowing attributes and elements](https://github.com/suvel/jest_notes#narrowing-attributes-and-elements)
- [triggering click](https://github.com/suvel/jest_notes#triggering-click)
- [checking condition](https://github.com/suvel/jest_notes#checking-condition)
- [debugg](https://github.com/suvel/jest_notes#debugg)
- [mitigating common error and warning](https://github.com/suvel/jest_notes#mitigating-common-error-and-warning)
- [How to ignore files from running test coverage](https://github.com/suvel/jest_notes/tree/main?tab=readme-ov-file#how-to-ignore-files-from-testing-unnessary)
- [How to run and get test coverage for a individual file](https://github.com/suvel/jest_notes/tree/main?tab=readme-ov-file#run-and-get-test-coverage-for-individual-filecomponent)

### test case template

Below template give an idea to come up  write test case of a component.

```
/*
:: Test scenarios
     - functionality
        The component is used to show list of string, if the list more than 3 then , it show first three item and show "show more" button.
     - data driven test
        - positive
            - sending array of 5 string
        - negative
            - sending null
            - sending string instead of array
    - user interaction
        clicking on "show more" need show itm number greater then 3.
*/
```

### mocking popular lib functions and hooks.

- react-router-dom

```
jest.mock("react-router-dom", () => ({
    ...(jest.requireActual("react-router-dom")),
    useNavigate: () => { },
    useParams: jest.fn().mockReturnValue({ id: '123' })
}));
```

### mocking exports

```
jest.mock(<file path>, () => ({
    <function name>: <function statement>,
  <variable name>: <value>,
}));
```

```
jest.mock(<file path>, () => ({
  <function name>: jest.fn().mockReturnValue(
    Promise.resolve(<return value>)
  ),
}));
```

```
jest.mock(<file path>, () => ({ <function name>: jest.fn() }))
```

```
<function name>.mockImplementation(<function statement>)
```

### finding elements

```
const renderer = render(<Component>)
```

- Find element by text

```
screen.getByText(<text to search>)
```

- find container

```
renderer.container.querySelector(<dom selector query>)
```

- find by role

```
const parentElement = screen.getByRole(<role type>,<options>);
```

- find element within another

```
within(parentElement).getByRole(<role type>);
within(parentElement).getByText(<text to search>)
```

- querying by text content

```
screen.queryByText(<string>)
```

### narrowing attributes and elements

- checking class attributes

```
const element = screen.getByText(<string>);
const className = element.className;
const containsClass = className.includes(<string without ".">)
expect(containsClass).toBe(true);
```

- checking style properties

```
const { container } = render(<React component>);
const firstChild = container.firstChild;
const styles = window.getComputedStyle(firstChild);
expect(styles.getPropertyValue(<property name>)).toBe(<property value>);
```

- checking text contents

```
firstChild.children[0].textContent
```

### triggering click

```
fireEvent.click(<element>, <event>);
```

```
fireEvent(<element>, new MouseEvent('click', {
    bubbles: true,
    cancelable: true,
}));
```

### checking condition

```
expect(<variable>).toBe(<bollean|string|number|object>);
```

```
expect(<function>).toHaveBeenCalledTimes(<number>);
```

```
expect(<element>).toBeTruthy();
```

```
expect(<element>).not.toBeNull();
```

```
expect(<element>).toBeInTheDocument();
```

```
expect(<variable>).toEqual(<bollean|string|number|object>)
```

### debugg

- print dom

```
screen.debugg()
```

- increase the debugg line that is printed, update the package.json script

```
"test": "set DEBUG_PRINT_LIMIT=300000&&jest"
```

### mitigating common error and warning

```
// imgTransform.js

module.exports = {
    process() {
      return {
        code: `module.exports = {};`,
      };
    },
    getCacheKey() {
      // The output is always the same.
      return "imgTransform";
    },
  };
```

- React is undefined, the below code will help import React to all component that will be tested.

```
// add `jest.setup.js`

import "@testing-library/jest-dom";
import React from 'react';

global.React = React;
```

- Issue importing svg image

```
//jest.config.js

 transform: {
    "^.+\\.svg$": "<rootDir>/imgTransform.js",
  }
```

- Issue importing gif,jpg and png image

```
//jest.config.js

 transform: {
    ".+\\.(css|styl|less|sass|scss|png|jpg|ttf|woff|woff2)$": "<rootDir>/imgTransform.js"
  }
```

### How to ignore files from testing unnessary

In jest.config.js you can add the file(s) name under "coveragePathIgnorePatterns" to ignore running  covergae for the same.

Ref: https://stackoverflow.com/questions/50992518/how-can-i-ignore-a-file-pattern-for-jest-code-coverage

### Run and get test coverage for individual file/component

use the below script to run the same:
```
> npm test <test_file_name> -- --coverage --collectCoverageFrom=<file_src_relative_path>
```
Ref:https://stackoverflow.com/questions/66492412/how-to-run-jest-tests-with-coverage-for-one-file
