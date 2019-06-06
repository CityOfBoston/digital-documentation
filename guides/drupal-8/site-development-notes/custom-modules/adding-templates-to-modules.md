# Adding Templates to Modules

When you make an `html.twig` file and add it to the templates folder of a custom theme you are pretty much done. It detects the template and uses it in preference to any template of the same name from the default theme you don't really have to do anything. But if you add a template to a module, there are a few things you must do:

1. put it in a folder called `templates` off the module root \(or else you will have to provide a path.
2. give it a name that matches an existing  suggestion or make a new suggestion
3. In the `hook_theme` of your module, in the array define the template replacing all "-"'s with "\_"'s, assigning a `base_hook` and optionally a `render_element`.  The render element defaults to `elements` \(you can actually give the array fieldname any name you choose, and map to any file you choose using a `template` element in the array. The array fieldname name must match a suggestion and all hooks will refer to the template using this name\)
4. You can then catch preprocess events using `hook_preprocess_fieldname` \(where fieldname is the name of the theme array element\)
5. You can also catch `template_preprocess_hook` events, where the hook=fieldname from above. In the `template_preprocess_hook` the `content` variable is set by taking all the rendered \(or renderable\) elements of the `elements` array.  An example is `template_preprocess_node__landing_page__full`

   -- the issue we have is that in our custom modules the `template_preprocess_hook` is not necessarily named properly and therefore the `content` array element is sometimes not built. \[a good/simple example of this working is the module `node_landing_page`\]

