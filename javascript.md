# Javascript/Typescript
Some guidelines for writing maintainable and modular Javascript/Typescript components.

## General
### Javascript vs Typescript
We generally try to create all of our new components using Typescript but there are exceptions.


### Functional Components vs Class Components
Functional components w/ [hooks](https://reactjs.org/docs/hooks-intro.html) are preferred.


### JSX
The `.tsx` or `.jsx` file extension should be used when creating a new file that contains JSX.


### Template Literals
Template literals are our preferred method of string concatenation. e.g.
```typescript
const firstName = 'Test';
const lastName = 'User';

const name = `${firstName} ${lastName}`; // Test User
```


### Destructuring Assignment
We [destructure](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) values from arrays and objects when:
* Multiple values from the object/array need to be accessed.
* The values are used more than once.
* For readability.

### When to create a folder for component organization.

A folder should be created in the instance where a group of components are so related and interdependent that you would want to put them in the same file. This is preferred over having multiple components in a single file because it keeps the line count of files from growing too large.

One instance where this should be implemented is Radio and Radio Group. These two components will always be used together, so it makes sense to let their file structure reflect that.

```
- Components/
	- radio-group/
		- __tests__/
			- Option.js
			- RadioGroup.js
		- index.js
		- Option.js
		- RadioGroup.js
```

We will use an index in order to export all of the related components. We are also able to namespace the components as we export them. This allows us to avoid adding a namespace within the radio folder, while still namespacing it where it is used.

`/index.js`
```javascript
import Option from './Option';
import RadioGroup from './RadioGroup';
RadioGroup.Option = Option;
export default RadioGroup;
```

When we implement RadioGroup it will look like this:

```javascript
import RadioGroup from '../radio-group';
<RadioGroup checked={checked} name="testradio" onChange={handleRadioChange}>
	<RadioGroup.Option label="Option 1" value={0} />
	<RadioGroup.Option label="Option 2" value={1} />
	<RadioGroup.Option label="Option 3" value={2} />
</RadioGroup>
```


## Typescript
### Interfaces
Always create an `Interface` for your React components. (We do not use `PropTypes` for our Typescript components.)

Our naming convention for `Interface`s is "I" + `componentName` + "(Props)".  e.g.
```typescript
interface ICardProps extends React.HTMLAttributes<HTMLElement> {
  date: moment.Moment;
  displayType: React.ComponentProps<typeof ClayButton>['displayType'];
  title: string;
}

const Card: React.FC<ICardProps, ICardState> = () => {/*...*/};
```

### Functions
When using Typescript, the parameters and return of functions should generally be typed.  e.g.
```typescript
const add = (x: number, y: number): number => x + y;
```

More examples can be viwed [here](https://www.typescriptlang.org/docs/handbook/functions.html).


## Javascript
### PropTypes
PropTypes should be used when using Javascript.
