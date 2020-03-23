---
layout: post
title: Setting up React.js component testing for create-react-app with jest and enzyme
date: 2020-03-22 22:28 -0400
---
<img class="mx-auto" style="width: 100%" src="https://ned-alyona.github.io/presentations/react-components-testing/pictures/cover.png" alt="Jest, React and Enzyme logos" />

Testing code has become a integral part of any programs life cycle and any developer worth their salt is creating meaningful tests for the code their writing.  Some even prefer to start with writing the tests before the code in what is referred to ask Test Driven Development and has the advantage of defining what it is you want that specific piece of code to do and has long lasting benefits as you build larger and more complex applications.

This post will be looking at setting up tests for program created with `create-react-app` and will be using the default test framework [Jest](https://jestjs.io/) that is generated default with `create-react-app` and adding the [Enzyme](https://github.com/enzymejs/enzyme) framework.  It should be noted that Jest and Enzyme are specifically designed to test react apps Jest can also be used to test any other JavaScript application while Enzyme is a test framework for only React.  If you don't already have it installed you can run `npm i -g create-react-app` to install the package globally, then you can generate the boilerplate app files with `create-react-app react-testing` where in this example `react-testing` is the name of the app we are creating.  After running that command you will end up with the following file structure.

![Screenshot of file structure generated with create-react-app](https://i.imgur.com/1XZmSdM.png)

As you can see we already have a few test related files created for us, the `App.test.js` and `setupTests.js` as well as some configuration set up in the `package.json` file.  You should be aware that if you decide to use a test framework other than jest like [tape](https://ci.testling.com/guide/tape), [mocha](https://mochajs.org/) or [jasmine](https://jasmine.github.io/) you would have to modify the `package.json` file.

The `setupTests.js` file come preconfigured for jest and looks like this.
```
// # src/setupTests.js

// jest-dom adds custom jest matchers for asserting on DOM nodes.
// allows you to do things like:
// expect(element).toHaveTextContent(/react/i)
// learn more: https://github.com/testing-library/jest-dom
import '@testing-library/jest-dom/extend-expect';
```

And `create-react-app` has also generously provided a test file for the `App` component with a single test that just makes sure that it renders the 'learn react' link.
```
// # src/app.test.js

import React from 'react';
import { render } from '@testing-library/react';
import App from './App';

test('renders learn react link', () => {
  const { getByText } = render(<App />);
  const linkElement = getByText(/learn react/i);
  expect(linkElement).toBeInTheDocument();
});
```

And we can check to make sure everything with jest it working properly by running that test with `npm test` which should give you this screen showing that all 1 tests passed.
![Screenshot of initial test output](https://i.imgur.com/VnWDUaz.png)

Next we add a few packages to get Enzyme working by running `npm i --save-dev enzyme enzyme-adapter-react-16`.  Then we need to modify the `src/setupTests.js` file to include and configure the newly added Enzyme packages.
```
// # src/setupTests.js

// jest-dom adds custom jest matchers for asserting on DOM nodes.
// allows you to do things like:
// expect(element).toHaveTextContent(/react/i)
// learn more: https://github.com/testing-library/jest-dom
import '@testing-library/jest-dom/extend-expect';

import { configure } from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';
configure({ adapter: new Adapter() });
```

Now we can add new tests that use Enzyme functions like shallow rendering to test components.  We just need to import the proper test functions which will usually be similar to this.  
```
import React from 'react';
import { render } from '@testing-library/react';
import { shallow } from 'enzyme';
```

Then we add tests with this format
```
describe('App Component', () => {

  it('renders without crashing', () => {
    // code to for test here
  })

})
```

So for example if we wanted to make sure that the App component renders the React logo properly we could modify our `src/App.test.js` file to look like this.
```
import React from 'react';
import { render } from '@testing-library/react';
import { shallow } from 'enzyme';
import logo from './logo.svg';
import App from './App';

test('renders learn react link', () => {
  const { getByText } = render(<App />);
  const linkElement = getByText(/learn react/i);
  expect(linkElement).toBeInTheDocument();
});

it('renders the react logo image', () => {
  const wrapper = shallow(<App />);
  const reactLogo = <img src={logo} className="App-logo" alt="logo" />
  expect(wrapper.contains(reactLogo)).toBe(true)
});
```

Just follow the naming convention for test files by creating a file with the component name plus `.test.js`, so a test file for a new component called `Counter.js` would be `Counter.test.js`.

And that's all you need to get testing your components in your app, I will be adding another post in the future to get full DOM rendering up and running for tests by adding [Sinon](https://sinonjs.org/) and [Chai](https://www.chaijs.com/)
