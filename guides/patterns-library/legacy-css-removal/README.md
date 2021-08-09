---
description: >-
  CSS is a simple stylesheet language for defining a website or document’s
  presentation.
---

# Refactoring Legacy CSS

However, this simplicity leaves the door open for many potential issues and **technical debt** — bloated code, specificity hell, duplicated code blocks with very little to no difference, leftover unused selectors, unnecessary hacks, and workarounds, to name a few.

### Refactor Or Rewrite? <a id="refactor-or-rewrite"></a>

_Refactoring_ allows developers to **gradually and strategically improve** to the existing codebase, without changing its presentation or core functionality. These improvements are usually small in scope and limited, and don’t introduce breaking, wide-ranging architectural changes or add new behavior, features, or functionality to the existing codebase.

### Incremental Refactoring Strategy <a id="incremental-refactoring-strategy"></a>

Refactoring is a challenging process that is much more complex than simply deleting the legacy code and replacing it with the refactored code. There is also the matter of integrating the refactored codebase with the legacy codebase and avoiding regressions, accidental code deletions, preventing stylesheet conflicts, etc. To avoid these issues, I would recommend using an incremental \(or granular\) refactoring strategy.

### Auditing CSS Codebase Health <a id="auditing-css-codebase-health"></a>

One of the tools we used to analyze  the codebase is [CSS Stats](https://cssstats.com/). A free tool that provides a useful overview of the CSS codebase quality with lots of useful metrics that can help developers catch some hard-to-spot issues. Other tools are: 

[Yellow Lab Tools](https://yellowlab.tools/), is a free tool for auditing web performance, but it also includes some very helpful helpers for **measure the complexity of your CSS** — and also provides actionable insights into how to resolve these issues.

[CSS Specificity Visualizer](https://isellsoap.github.io/specificity-visualizer/) provides an overview of CSS selectors and their specificities across a CSS file. Once you submit a stylesheet, the tool returns a specificity graph.

Unlike other tools, [Project Wallace](https://www.projectwallace.com/), created by Bart Veneman, additionally keeps the history of your CSS over time. You can use webhooks to [automatically analyze CSS on every push](https://www.projectwallace.com/blog/automatically-analyze-css-on-every-push/) in your CI. The tool tracks the state of your CSS over time by looking into specific CSS-related metrics such as **average selector per rule**, maximum selectors per rule and declarations per rule, along with a general overview of CSS complexity.





