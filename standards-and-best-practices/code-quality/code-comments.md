---
description: Code tells you how; comments tell you why.
---

# Code comments

Code comments can save a lot of time and stress for the next person working in the codebase \(spoiler: it’s probably future you\) by reducing the effort to understand a piece of code, or the intent behind it. [Context switching is expensive](https://www.joelonsoftware.com/2001/02/12/human-task-switches-considered-harmful/); a well-placed explanatory comment can reduce or prevent the need, and allow a developer to continue on with their initial task uninterrupted.

## Consider including a comment when...

### Code is written a certain way for a particular reason

```typescript
// We use forEach and push as opposed to Array.from or the spread operator
// because both are supported in IE 11 without polyfills.
optionsSet.forEach(option => {
    this.selectOptions.push(option);
```

### You ran into Something Weird during development

```typescript
/* This component works exclusively in UTC due to bugs / inconsistencies in how
 * browsers handle daylight savings time in the past. E.g., Safari creates
 * midnight 4/10/57 should be as 4am UTC but (correctly) displays it assuming a
 * -500 offset.
 */
```

### You’ve made a fix for a particular issue or PR

```typescript
// See https://github.com/CityOfBoston/digital/issues/360
```

### Solution has been copied from elsewhere

```typescript
// see https://react-dropzone.netlify.com/#!/Previews
// Make sure to revoke the data uris to avoid memory leaks
if (this.state.file) {
    URL.revokeObjectURL(this.state.file.preview);
```

