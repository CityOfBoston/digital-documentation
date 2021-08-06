---
description: >-
  CSS is a simple stylesheet language for defining a website or document’s
  presentation.
---

# Legacy CSS removal

However, this simplicity leaves the door open for many potential issues and **technical debt** — bloated code, specificity hell, duplicated code blocks with very little to no difference, leftover unused selectors, unnecessary hacks, and workarounds, to name a few.

### Refactor Or Rewrite? <a id="refactor-or-rewrite"></a>

_Refactoring_ allows developers to **gradually and strategically improve** to the existing codebase, without changing its presentation or core functionality. These improvements are usually small in scope and limited, and don’t introduce breaking, wide-ranging architectural changes or add new behavior, features, or functionality to the existing codebase.

### Incremental Refactoring Strategy <a id="incremental-refactoring-strategy"></a>

Refactoring is a challenging process that is much more complex than simply deleting the legacy code and replacing it with the refactored code. There is also the matter of integrating the refactored codebase with the legacy codebase and avoiding regressions, accidental code deletions, preventing stylesheet conflicts, etc. To avoid these issues, I would recommend using an incremental \(or granular\) refactoring strategy.

