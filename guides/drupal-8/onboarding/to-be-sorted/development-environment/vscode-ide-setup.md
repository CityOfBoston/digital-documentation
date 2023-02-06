---
description: Setting up the Visual Studio Code editor to work well with Drupal
---

# VSCode IDE Setup

#### Add XDebug

![](<../../../../../.gitbook/assets/debug (1).png>)

1. Under Extensions <img src="../../../../../.gitbook/assets/extensions.png" alt="" data-size="line">in the left sidebar, search for "PHP Debug" and click "Install" <img src="../../../../../.gitbook/assets/install.png" alt="" data-size="line">&#x20;
2. Edit `.vscode/launch.json`  &#x20;
3. Add the following configuration:

```
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Listen for XDebug",
            "type": "php",
            "request": "launch",
            "port": 9000,
            "pathMappings": {
              "/app/": "${workspaceRoot}/",
            }
        },
        {
            "name": "Launch currently open script",
            "type": "php",
            "request": "launch",
            "program": "${file}",
            "cwd": "${fileDirname}",
            "port": 9000
        }
    ]
}
```

#### Add Drupal coding standards

![](../../../../../.gitbook/assets/phpcs\_drupal.png)

1. Under Extensions <img src="../../../../../.gitbook/assets/extensions.png" alt="" data-size="line">in the left sidebar, search for "phpcs" and click "Install" <img src="../../../../../.gitbook/assets/install.png" alt="" data-size="line">&#x20;
2. Click in top navbar navigate to `file > preferences > settings`&#x20;
3. Under `Workspace Settings` expand the `Extensions` option&#x20;
4. Locate `PHP CodeSniffer configuration` and scroll down to the `Standard` section and click the "Edit in settings.json" link
5. Add the following configuration to your `Workspace Settings`:

```
{
    "phpcs.standard": "vendor/drupal/coder/coder_sniffer/Drupal",
}
```
