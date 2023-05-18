---
description: Testing And Avoiding Regressions.
---

# Regression Testing

Nevertheless, bugs and regressions can happen. Ideally, We want to avoid introducing issues and regressions in the codebase — conflicting styles, missing styles, incorrect markup, leaked styles from other elements, etc.

Using automated visual regression testing tools like [Percy](https://percy.io/) on a Pull Request level can speed up testing, allow developers to address regressions quickly and early, and **make sure that the bugs don’t end up on the live site**. These tools can take screenshots of individual pages and components and compare changes in style and layout and notify developers of any visual changes that can happen due to changes in the markup or CSS.

Since the CSS refactor process shouldn’t result in any visual changes in usual cases and depending on the task and issues in the legacy codebase, the visual regression testing process can potentially be simple as checking if any visual changes happened at all and checking if these changes were intentional or unintentional.
