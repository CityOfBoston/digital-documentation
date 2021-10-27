---
description: Three options for setting up a development environment on Windows.
---

# Using Windows

## Introduction

Because Drupal is most commonly installed on Linux servers, City of Boston DoIT does not recommend using Windows® as a developer machine due to the increased difficulty in emulating the most common Drupal production web server.\
\
However, if you have no alternative, or harbor an unquenchable desire to use Windows® then the following best practices and instructions should get you headed in the right direction.\
\
There are 3 strategies to choose from:

{% tabs %}
{% tab title="Develop on Windows host" %}
This is the most complicated solution to setup, but allows the developer to use any windows-based tools desired to manage the Drupal codebase and databases.

## General Development Strategy

The git repo is cloned to a local Windows folder on the Windows host. This repo folder is mounted into a Linux (Ubuntu) Docker Container (like a VM). Docker manages the virtualization and the container contains all the apps and resources required to host and manage the website locally for development purposes. Git commands are run either from the Windows host, or from the container. Lando (a container manager tool) provides a “wrapper” whereby commands (e.g. Docker, Lando, Git, Phing, Drush, Composer, SSH etc) are typed into a console on the Windows host, and Lando executes them inside the container.\
\
To be clear, with this strategy:

* The container hosts the website
* The developer normally changes/adds/removes Drupal files in the Windows folder on the Windows host
* Changes to custom Drupal files (i.e. to files in the mounted folder) either on the host or in the container are immediately available to both the host and container without restarting docker or VMs
* The developer normally runs dev tools such as Git, Drush, Phing and Composer in the container, using Lando commands
* The Windows host does not require to have tools other than Docker, Lando and VBox or Hyper-V installed on it
* Some developers still like to have git installed on the Windows host so their IDE tools (e.g. PHPStorm) can manipulate the repos directly
* Developers’ need to interact directly with the container (i.e. via ssh) is minimized, and

## Set up local development environment

This installation creates a developer environment suitable for a Linux-based production deployment.

{% hint style="danger" %}
Due to Lando requirements to use Docker CE (not Docker Toolkit), which in turn requires Hyper-V, you: \
\
NEED to have a Windows 10 64bit Professional or Enterprise version\
CANNOT use Windows 7 or earlier \
CANNOT use Windows Home or Home Pro as Hyper-V is required by Lando and does not ship with home versions.
{% endhint %}

These 6 steps are all performed on the host (i.e your Windows®) PC.

### 1. Set up Virtualization

This is required to supply a Linux core which is needed by Docker to generate the necessary containers.

**Install Windows Subsystem for Linux (preferred method)**

These instructions also depend on having a current version of Windows® 10 (version later than `Fall Creators Update` and pref build 16215 or later).\
\
To install WSL support, do the following:

1. Open Windows Powershell as Administrator
2.  Run:&#x20;

    `Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux`
3. Restart Windows when prompted

_Taken from _[_here_](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

**Install Linux Distro**

DoIt suggests you install the [Linux distribution](https://www.microsoft.com/en-us/store/search/apps?q=%22linux+subsystem%22\&category=Developer+tools\&Price=0) from the Microsoft Store which most closely matches the Linux distro you will use on your production webservers. If you are unsure, install [Ubuntu](https://www.microsoft.com/en-us/p/ubuntu-1804/9n9tngvndl3q) or [Debian](https://www.microsoft.com/en-us/p/debian-gnu-linux/9msvkqc78pk6).

**Install Hyper-V**

If Hyper-V is not enabled when the Linux subsystem was installed (check by typing “Hyper-V” in the start menu), then follow these [instructions](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v).

### 2. Install Git (optional but recommended)

If you are not using WSL, then Git for Windows provides a bash terminal for the Windows host. Installing Git for Windows is a convenient way to get this, and also gives the developer the option to directly execute git commands (against the repo) from the Windows host. This step is optional if you use WSL, or if you are confident with some other tool to provide a bash style console.\
\
Use Git for Windows from [here](https://git-scm.com/download/win).\
\
This is a good [tutorial](https://hackernoon.com/install-git-on-windows-9acf2a1944f0) to step thru installation.

### 3. Install Docker

If you are using WSL and have enabled Hyper-V for your virtualization, then use the Docker “community version” from [here](https://docs.docker.com/docker-for-windows/install/) - this link also guides you through an install.  &#x20;

### **4. Install Lando**

Download the latest Windows .exe installer from [here](https://github.com/lando/lando/releases).

### **5. Install IDE/editor**

On Windows®, DoIT recommends:&#x20;

* [PHPStorm](https://www.jetbrains.com/phpstorm/download/)
* [VS Code](https://code.visualstudio.com)
* [Sublime Text](https://www.sublimetext.com/3)
* [Notepad++](https://notepad-plus-plus.org/download/)

{% hint style="success" %}
In order to use VS Code for Drupal development, [use this guide](https://www.drupal.org/docs/develop/development-tools/configuring-visual-studio-code) as a starting point. The editor is highly configurable with many extensions available. You will likely want to customize it further based on your needs.
{% endhint %}

### **6. Finished: goto Clone repo**

Pickup from step 3 on the [quick install guide](https://boston.gitbook.io/digital-documentation/installation-instructions).
{% endtab %}

{% tab title="Develop in Linux or OSX installed in a VM" %}
This solution may be a quick and viable option if you have a powerful Windows machine to use as the host, and are not doing much development which required extensive use of an IDE. Depending on your setup, there may be issues with IPAddress routing, requiring complex configurations.

{% hint style="warning" %}
This method is not used by City of Boston DoIT, the preferred solutions on Windows machines are A or B.
{% endhint %}

## **Guidance/Tips**

* For Windows® versions before 10 Fall Creators Update, we recommend that [VirtualBox](https://www.virtualbox.org/wiki/Downloads) (free from Oracle) is used
* For later versions you should use enable and use Hyper-V within Windows.
* In the VM, install a Linux distro as close as possible to the production distro you will use, and unless you are very comfortable with the Linux CLI, be sure to install a distro with a GUI.
* Once the Linux distro is installed, then follow the [setup instructions for Linux](https://github.com/CityOfBoston/boston.gov-d8/wiki/Linux-Developer-Guide).
{% endtab %}

{% tab title="Develop in Linux using a dual boot" %}

{% endtab %}
{% endtabs %}
