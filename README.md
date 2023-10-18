# unit component testing guid

### KEY POINTS
- [mocking popular lib functions and hooks](https://github.com/suvel/jest_notes#mocking-popular-lib-functions-and-hooks)
- [mocking exports](https://github.com/suvel/jest_notes/edit/main/README.md#mocking-exports)
- [finding elements](https://github.com/suvel/jest_notes#mocking-exports)
- [triggering click](https://github.com/suvel/jest_notes#triggering-click)
- [checking condition](https://github.com/suvel/jest_notes#checking-condition)
- [debugg](https://github.com/suvel/jest_notes#debugg)
- [mitigating common error and warning](https://github.com/suvel/jest_notes#mitigating-common-error-and-warning)


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

### triggering click

```
fireEvent.click(<element>, <event>);
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
