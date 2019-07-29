---
description: Run phpcs on your custom modules
---

# PHP CodeSniffer

PHP CodeSniffer \(https://github.com/squizlabs/PHP\_CodeSniffer\) is already included with our D8 project with composer. If you run a `lando composer install` you should have it available at `./vendor/bin/phpcs`

### Local Setup:

1. You need to specifically add Drupal coding standards to phpcs. You can do this globally for your computer by running:

```text
composer global require drupal/coder:^8.3.1
```

2. You need to make sure phpcs knows about your newly installed coding standard \(note the path below assumes you're using Ubuntu, yours might be different on a mac\):

```text
./vendor/bin/phpcs --config-set installed_paths ~/.config/composer/vendor/drupal/coder/coder_sniffer/
```

3. Now you can run this manually against your custom modules:

```text
./vendor/bin/phpcs --standard=Drupal docroot/modules/custom/
```

If you're looking for more info, here's a good place to get started: [https://www.drupal.org/docs/8/modules/code-review-module/installing-coder-sniffer](https://www.drupal.org/docs/8/modules/code-review-module/installing-coder-sniffer)

