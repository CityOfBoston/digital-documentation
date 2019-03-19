---
description: Set up environment for Drupal development on various operating systems.
---

# Development environment

Select your operating system from below, and follow the instructions to setup your development environment and prepare to install the City of Boston Drupal 8 website.

{% tabs %}
{% tab title="Linux" %}
## Linux

{% hint style="warning" %}
City of Boston recommend the Ubuntu 16.04 or later distribution.  While other Linux distributions will operate well, the instructions below assume the use of Ubuntu and, in particular, the `apt` package manager. 
{% endhint %}

### 1. Install Docker

```bash
apt-get update
apt-get install docker
```

### 2. Install Docker-compose and make executable

Check [Docker pre-requisites.](https://docs.docker.com/compose/install/#prerequisites)

```bash
sudo curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose    
sudo chmod +x /usr/local/bin/docker-compose
```

### 3. Install Docker-machine

If using PHPStorm, [install Docker-machine](https://docs.docker.com/machine/install-machine/#install-machine-directly) 

```bash
base=https://github.com/docker/machine/releases/download/v0.14.0 &&
curl -L $base/docker-machine-$(uname -s)-$(uname -m) >/tmp/docker-machine &&
sudo install /tmp/docker-machine /usr/local/bin/docker-machine
```

### 4. Install Lando

```bash
apt-get update
apt-get install lando
```

### 5. Install Git

```bash
apt-get update
apt-get install git
```

### 6. Generate and register ssh keys

[How to create SSH keys for github](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)

### 7. Install IDE and configure debugger \[optional\].

There are many IDE's capable of being used to write, verify and deploy PHP code.  City of Boston do not endorse any particular platform, but have successfully used the following:

* [Notepad++](https://notepad-plus-plus.org/download/) \(basic text editor\)
* [Sublime Text](https://www.sublimetext.com/3) \(improved text editor\)
* VIM \(Linux-based advanced text editor\)
* [Visual Studio Code](https://code.visualstudio.com/) \(full IDE\)
* Eclipse \(full IDE\)
* [PHPStorm](https://www.jetbrains.com/phpstorm/download/) \(full IDE\)
{% endtab %}

{% tab title="MacOS" %}
## Mac/OSX
{% endtab %}

{% tab title="Windows" %}
## Windows

{% hint style="warning" %}
Because Drupal is most commonly installed on Linux servers, City of Boston DoIT **does** **not** recommend using Windows® as a developer machine due to the increased difficulty in emulating the most common Drupal production web server.
{% endhint %}

However, if you have no alternative, or harbor an unquenchable desire to use Windows® then the following best practices and instructions should get you headed in the right direction.

{% page-ref page="using-windows.md" %}
{% endtab %}
{% endtabs %}

