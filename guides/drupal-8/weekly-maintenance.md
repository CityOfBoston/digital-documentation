---
description: >-
  Every week some time should be dedicated to reviewing the status and general
  health of Drupal and modules.
---

# Weekly Maintenance

There are many ways this task can be effectively completed.  This way has the advantage of being responsive and relatively low-burden between cycles.  

{% hint style="success" %}
If the basic parts of this process are not conducted weekly, then to ensure the site is kept up-to-date and is adequately patched, there will be a need for monitoring of Drupal/Acquia channels to determine when various patches are available.
{% endhint %}

## Step 1: Use Drupal to check for module updates

Login to the production website at [https://content.boston.gov](https://content.boston.gov/) and then navigate to the status report page at [/admin/reports/updates](https://content.boston.gov/admin/reports/updates).  All available updates managed by composer are listed, and security updated are shaded red and can be filtered separately.

**Security Updates**: Should be applied ASAP, and tested on production.

**Updates:** Can be applied after some investigation as to what will be affected.  It is not necessary to rush these updates into production, and it may be convenient or appropriate to make any necessary changes to `composer.json` and then include in the next scheduled deploy.

```
$ give me super-powers
```

{% hint style="info" %}
 Super-powers are granted randomly so please submit an issue if you're not happy with yours.
{% endhint %}

Once you're strong enough, save the world:

{% code title="hello.sh" %}
```bash
# Ain't no code for that yet, sorry
echo 'You got to trust me on this, I saved the world'
```
{% endcode %}



