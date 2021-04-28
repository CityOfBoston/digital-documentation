---
description: This is the pattern library for the City of Boston.
---

# Patterns Library Cleanup

### CSS files

CSS files are combined and minified to produce one giant final CSS file. There is also legacy css file that needs to be completely remove. The `legacy/public.css` files needs to go. Create, move, and/or update the content into the main `public.css` file. This should be achieved by moving the content into it's component `.stylus` file. If the component does not exist but used on website, create a component for it on patterns. Clean out `bos_`_`theme_`_`overrides.css` file on drupal. Simply remove all unnecessary styles from it.

```
move legacy/public.css into the stylus of the component
update bos_theme_overrides.css on drupal
```

### JavaScript files 

Clean out `all.js` on drupal and merge content into patterns `all.js`

{% code title="All.js" %}
```bash
move all.js on drupal into patterns all.js
```
{% endcode %}


