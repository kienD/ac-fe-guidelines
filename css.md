## CSS

### One File Per Component File

In the same way that we should try to keep our javascript logic modular by organizing it into components, we should do the same for the related styles:

```
- Components/
	- Button.js
	- Avatar.js
- Styles/
	- Button.scss
	- Avatar.scss
```

If there happens to be two components in the same file(one being private), we only need one scss file.

```
- Components/
	- InputList.js
		- contains two components, InputList and InputListItem. But it only exports InputList since InputList consumes InputListItem.
- Styles/
	- InputList.scss

```Component styles should always be namespaced under a selector with the suffix `-container`:

### Namespace Component Styles

```scss
.button-root {
	/* Styles for the Button component */
}
```

Make sure to declare variables, mixins, or any other type inside of this selector so that they are scoped to that component.

### Use Variables

If two styles are meant to have the same value, make sure to extract it into a variable. [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) still applies here, as usual. However, don't do this just because two styles happen to have the same value.

```scss
.distribution-card-root {
	$contentMargin: 0 0 24px;

	/* The next two elements should always have the same margin */
	.name {
		color: green;
		margin: $contentMargin;
	}

	.text {
		margin: $contentMargin;
	}

	/* This element just happens to have the same value, no semantic relation */
	.avatar-container {
		margin: 0 0 24px;
	}
}
```

If you have global values shared between multiple components, make sure they are named well to avoid conflicts:

```scss
/* _globals.scss */

$baseBoxShadow: 0 0 4px 4px rgba(0, 0, 0, 0.2);
$cardTitleHeight: 24px;

/* BAD: This name is easily clobbered by later declarations */
$padding: 4px;
```
