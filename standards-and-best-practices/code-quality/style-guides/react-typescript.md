# React/TypeScript

Since we are using Prettier to enforce basic code style, there’s no need to go into detail on those bits of syntax (e.g. always using semicolons, avoiding extraneous whitespace, etc).

## General

* In nearly all cases, there should be only one React component per file. The file name should match the component name.
* A component’s story file (or test file) should be colocated with the component.

## Naming

* File/component name should be descriptive.
* Use `.tsx` extension for React components.
* Storybook files should be named `ComponentName.stories.tsx`.
* Unit test files should be named `ComponentName.test.ts`.
* Interface and type names should be in PascalCase.
* Constant value names should be in ALL\_CAPS.

## Syntax

* Prefer `<></>` shorthand over `<React.Fragment></React.Fragment>`.
* Avoid `var`; use `const` by default, or `let` when necessary.

### Comments

Use JSDoc style when documenting classes and functions, and standard `//` elsewhere in the code.

```typescript
/**
 * Function used to count the number of eggs in a given nest.
 * 
 * Broken eggs will not be counted.
 */
function eggCounter(nest: Eggs[]): number {
  // If an egg is broken, we still need to return the count.
  return nest.reduce((count, egg) => egg.isBroken ? count : count + 1, 0);
}
```

## TypeScript

### Overriding type errors

Remember: TypeScript errors are for **our** benefit as developers, so we should always take the time to resolve type errors as often as possible. Override only as a last resort!

```typescript
//@ts-ignore
this.setState({ [event.target.name]: event.target.value }); // avoid

this.setState({ [event.target.name]: event.target.value } as any); // preferred
```

## React

### Specifying defaultProps

Declare as a field on the component’s Class definition, and set its type as a Partial of the component’s Props interface.

```typescript
interface Props {
  isReady: boolean;
  firstName?: string;
}

static defaultProps: Partial<Props> = {
  isReady: false,
};
```

### Using Refs

See [https://reactjs.org/docs/refs-and-the-dom.html](https://reactjs.org/docs/refs-and-the-dom.html) for detailed information. When referencing a Ref in your code, don’t forget that you need to use the `current` attribute to access the actual DOM element!

```typescript
export default class CustomInput extends React.Component {
  private inputRef = React.createRef<HTMLInputElement>();
  
  private logValue = (): void => {
    console.log(this.inputRef.current.value);
  }

  render() {
    return (
      <input type="text" ref={this.inputRef} />
    );
  }
}
```



## Emotion

See the [Emotion guide](../../../guides/libraries-and-tools/emotion.md).
