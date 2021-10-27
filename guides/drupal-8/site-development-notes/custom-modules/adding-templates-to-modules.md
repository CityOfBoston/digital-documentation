# Adding Templates to Custom Modules

When you make an `html.twig` file and add it to the templates folder of a custom **theme** you are pretty much done (after refreshing caches!).  The Drupal theme rendering processes detect the template and uses it in preference to any template of the same name from a parent or default theme. You don't really have to do anything more than add the file and refresh cache.&#x20;

But, if you add a template to a custom **module** -even if your intent is just to override a theme default template (e.g.`field.html.twig`) or to provide a suggested template, there are a few extra things you must do.

{% hint style="info" %}
**Using the example of a custom content type (node) called "node\_landing\_page", the steps below fully implement a template to be used to render the nodes `full` display. **

Note: Drupal automatically generates the suggestion for`node__landing_page__full` which can be used for rendering the "default" (i.e. "full") display.

_You can generate other suggestions using the `hook_theme_suggestions_hook` hook._
{% endhint %}

1. Create the twig template you wish to use, and give it a name that matches an existing Drupal theme suggestion with ".html.twig" as the extension. \
   _In rare cases you may want to create a new template suggestion. Do this by returning an array of suggestions from a`hook_theme_suggestions_hook()`in your custom module (see last example below)._\
   _**Convention is to name the template using an "entity breadcrumb" style, with "--"'s between entities and no spaces. **_
2. Save the template file in a folder called `templates` in your custom modules root folder.  In our example `docroot/modules/custom/node_landing_page/templates` .\
   _- You could organize files by creating a sub-folder tree - but if you do, you will then have to specify the `path` to your template in the `hook_theme` - see step 3 below._ &#x20;
3. In the `hook_theme` of your module you must define your new template.  This hook is read by the Drupal core theme engine and loaded into a template cache (aka register).  _**Whenever a change is made to this hook you need to clear all caches to load your changes into the cache**_.  \
   In `hook_theme` return an assoc array with key-value pair nested arrays for each template you wish to define. \
   \- The outer keys (template-keys) should be one for each of the templates you are defining.  Keep it simple and traceable by setting the the template-key name to be the template filename without the ".html.twig".  **Important: Replace all "-"'s with "\_"'s in the template-key string.**  (in our example the template-key is `node__landing_page_full`) \
   \- The value for the key (template-key) is an array with a required `base_hook` and several other optional fields.  \
   The `base_hook` should define the entity type this template is used to render (in our case `node` but other common entities we theme are `field, region, block, paragraph, taxonomy_term` ) .  \
   \[optional] The `render element` defaults to `elements` if not specified.\
   \[optional] If you wish to use a template file which is not the same name as the suggestion (with "\_"'s replaced with "-"'s) then you must specify its name in the `template` field. Omit the "html.twig" extension.  _This could be useful if you want 2 display to share the same template._\
   \[optional] If you want to use a custom path to the template file (i.e. not the default templates folder) then use the `path` field.\
   (see `bos_link_collections_theme` in boston.gov for example)\
   (see "Our Example hook\_theme" below for the complete hook)
4. **\[optional]** Once the cache is cleared you can then catch pre-process events using `hook_preprocess_hook` in our example this would be `node_landing_page_preprocess_node` (to catch all node pre-process events) or `node_landing_page_preprocess_node__landing_page__full` (to catch only this new template pre-process events) - notice that the hook uses the `template-key` defined in the `hook_theme` array.
5. **\[optional] **You can also catch `template_preprocess_hook` events (in our example this is `template_preprocess_node__landing_page__full`). \
   This hook is commonly used to create a `content` variable which contains all the rendered (or renderable) elements of the `elements` (or whatever the field is named in the templates `render element`) array.&#x20;

**Our Example template file:**

{% code title="templates/node--landing-page--full.html.twig" %}
```markup
{#
/**
 * @file
 *
 * @see template_preprocess_landing_page()
 */
#}
<article{{ attributes }}>
  {% if (title_prefix or title_suffix or display_submitted or unpublished or preview or (not page)) and title %}
    <header>

      {{ title_prefix  }}

      {{ title_suffix  }}

      {% if unpublished %}
        <mark class="watermark">Unpublished</mark>
      {% elseif preview %}
        <mark class="watermark">Preview</mark>
      {% endif %}

    </header>
  {% endif %}

  {{ content }}

</article>
```
{% endcode %}

**Our Example hook\_theme:**

{% code title="node_landing_page.module" %}
```php
/**
 * Implements hook_theme().
 */
 function node_landing_page_theme() {
  return [
    'node__landing_page__full' => [
      'base_hook' => 'node',
      'render element' => 'elements',
      'template' => 'node--landing-page--full',
      'path' => drupal_get_path('module', 'node_landing_page') . '/templates/node/'
    ],
  ];
}
```
{% endcode %}

**Our Example hook\_preprocess\_hook (version 1):**

{% code title="node_landing_page.module" %}
```php
/**
 * Implements hook_preprocess_HOOK().
 */
function node_landing_page_preprocess_node(&$variables) {
  if (isset($variables["node"]) && is_object($variables["node"]) && $variables["node"]->getType() == "landing_page") {
    $variables["attributes"]->addClass("full-display");
    $variables["unpublished"] = 0;
  }
}
```
{% endcode %}

**Our Example hook\_preprocess\_hook (version 2):**

{% code title="node_landing_page.module" %}
```php
/**
 * Implements hook_preprocess_HOOK().
 */
function node_landing_page_preprocess_node__landing_page__full(&$variables) {
  $variables["attributes"]->addClass("full-display");
  $variables["unpublished"] = 0;
}
```
{% endcode %}

**Our Example template\_preprocess\_hook:**

{% code title="node_landing_page.module" %}
```php
function template_preprocess_node__landing_page__full(array &$variables) {
  foreach (Element::children($variables['elements']) as $key) {
    $variables['content'][$key] = $variables['elements'][$key];
  }
}
```
{% endcode %}

**Our Example hook\_theme\_suggestions\_hook:**

{% code title="node_landing_page.module" %}
```php
/**
 * Implements hook_theme_suggestions_HOOK().
 *
 * This is used to create a cut-down node when site_alert is called from
 * the page display of a view.
 */
function node_landing_page_theme_suggestions_node(array $variables) {
  if ($variables["elements"]["#region"] == "site_alert") {
    return ["node__no_wrapper"];
  }
}
```
{% endcode %}
