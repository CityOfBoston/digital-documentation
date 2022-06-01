---
description: How we document code and workflows.
---

# Technical documentation

{% hint style="info" %}
If you’re working on a task that’s missing documentation (or you think of something randomly) but you don’t have time to write it up, add a card for it to the documentation project board: [https://github.com/CityOfBoston/digital-documentation/projects/1](https://github.com/CityOfBoston/digital-documentation/projects/1)
{% endhint %}

## General documentation

This (GitBook) is the place to document technical information that isn’t specific to a particular project. We should be liberal with documenting our workflows and standards, and aim for the standard that any new developer could independently find everything they needed to start working on any of our technical tasks.

{% content-ref url="../getting-started/using-gitbook.md" %}
[using-gitbook.md](../getting-started/using-gitbook.md)
{% endcontent-ref %}

## Project-specific documentation

Any technical information that’s specific to a particular project should be in a `README.md` at the root directory of the project’s source files. Examples include: how a new developer could install the project locally, a list of npm/Yarn tasks... basically, anything that “should be in a README”. [https://www.makeareadme.com/](https://www.makeareadme.com/) is a great reference for the types of information that should be included.

Also see project specific information further down in Gitbook.\
\
The project’s page in the “Project” section of this GitBook should include a link to its README on GitHub.

## Code comments

Documentation comments are super-useful! See the dedicated page for more guidance.

{% content-ref url="code-quality/code-comments.md" %}
[code-comments.md](code-quality/code-comments.md)
{% endcontent-ref %}
