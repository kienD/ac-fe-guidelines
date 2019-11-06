# Frontend Unit Testing


## General

### Technology
Our unit testing stack currently consists of [Jest](https://jestjs.io/), [Enzyme](https://airbnb.io/enzyme/), and more recently [Coveralls](https://coveralls.io/features). We use Jest as a test runner and Enzyme for working in React component tree. We use Coveralls to continuously monitor and improve code quality by setting thresholds for test coverage.

### Test File Structure
All tests are co-located in the same directory as the components under test, in a `__tests__` folder.

For example, in the image above, you can see the folder directory follows the pattern:

- folder_name/
  - __tests__/
    - __snapshots__/
      - Component.jsx.snap
      - OtherComponent.jsx.snap
  - Component.jsx
  - OtherComponent.jsx
  - Component.jsx
  - OtherComponent.jsx

## Testing Strategy

### Component Testing
* We mostly try to avoid mounting components into a DOM for testing. We primarily use [shallow()](https://airbnb.io/enzyme/docs/api/ShallowWrapper/shallow.html) from enzyme, so it will return the rendered component in isolation.
* We start with a base test case, often called 'should render', that represents the most generic form of the component.
* Test all the possible visual states the component could have as HTML
* We don't render any SVG elements from charts in tests.
* Sometimes we will render a JSON snapshot of the component output, either in component form, or as plain HTML.
* For a component, we often make assertions about the rendered component tree
  * For example, suppose some component should render an alert in some scenario and the alert should be of this type and should contain this content
    * Find component in tree assert class name and call `.text()` to assert text content
* We may also extract something from a component to test the rendered output
  * For example, some class may have a method called `RenderEmptyState`. In a test, we may call the method and check the rendered output

### Pages
* If the page renders differently per entity, we would add a test case for each entity. ( segment, account, individual, etc)
* For pages we rarely render deeply, but focus instead on what is present at the file level being tested. Ex: (Overview page layout). Instead of rendering MetricsCard in its entirety, we just capture it shallowly as being in the layout.
```html
<div className='col-xl-12'>
  <MetricsCard
    className='site-metrics'
    label='Site Metrics'
  />
 </div>
```

### Testing Pure Functions (other code that is not a UI component like a static function/util)
We add expected input/output tests with primarily in-line mock data.
```javascript
describe('getMilliseconds', () => {
  it.each`
    duration | unit             | retVal
    ${3600}  | ${time.SECONDS}  | ${3600000}
    ${60}    | ${time.MINUTES}  | ${3600000}
    ${1}     | ${time.HOURS}    | ${3600000}
  `('format $unit to milliseconds', ({duration, retVal, unit}) => {
    expect(time.getMilliseconds(duration, unit)).toBe(retVal);
  });
});
```

### Coverage
```json
{
  "coverageThreshold": {
    "global": {
      "branches": 55,
      "functions": 55,
      "lines": 65,
      "statements": 65
    }
  }    
}
```

## Mocking Data
For mocking data, we generally use a combination of the following strategies:
* Import mock data functions from a shared file
* Built in Jest mock features
  * Spies
  * Adding `__mocks__` directory at the same level of functions we want to mock, so functions in those `__mocks__` directories will be used instead of the real ones during test
* Inline mock data
  * We just write out a plain object to use as mock data in the test file.

## Future Plans
Our future plans involve a few key items:

* Exploring a more implementation-detail free approach for increased test efficacy
  * We have added 'react-testing-library' to the project, which should help us to write more user-oriented component tests.
  * Instead of digging into a component instance to manually fire a prop that is a function, we could explore fully mounting a component in a virtual DOM, and triggering some interaction with the rendered UI and asserting the outcome.
    * Potentially this kind of mindset change could produce tests that are more behavioral and are closer to how users will see/interact with the UI.
* Reduce frequency of large snapshots
  * Large snapshots are tough for a tester to read when they fail and they may also capture too much unrelated information, causing the test to fail when nothing really in the specific component is affected.
* Continuously increase coverage across all metrics (branches, functions, etc.)
