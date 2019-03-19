---
description: Set up tools for cross-browser and mobile testing
---

# Drupal QA Setup

## Browser Plugins

| TOOL | USE |
| :--- | :--- |
| \*\*\*\*[**Page Load Time**](https://chrome.google.com/webstore/detail/page-load-time/fploionmjgeclbkemipmkogoaohcdbig?hl=en) | displays time to load |
| \*\*\*\*[**LinkMiner**](https://chrome.google.com/webstore/detail/linkminer/ogdhdnpiclkaeicicamopfohidjokoom?hl=en)\*\*\*\* | crawls page and highlights dead links on pages |
| \*\*\*\*[**BrowserStack**](https://live.browserstack.com/?utm_source=chrome&utm_medium=extension&utm_campaign=quick-launch#os=ios&os_version=11.0&device=iPhone+X&zoom_to_fit=true&full_screen=true&url=https%3A%2F%2Fwww.boston.gov%2F&speed=1&host_ports=google.com%2C80%2C0)\*\*\*\* | cross-browser testing and mobile testing environment |



{% tabs %}
{% tab title="1 - SETUP BROWSER TOOLS" %}
**CONFIGURE CHROME BROWSER EXTENSIONS**

| **STEP** | **ACTIVITY** | EXPECTED BEHAVIOR |
| :--- | :--- | :--- |
| 1 | Open the Chrome Browser | Opens chrome browser window |
| 2 | Enter chrome://extensions/ in address bar | Takes you to the extensions page |
| 3 | Locate **Page Load Time** extension in the list and click the toggle to the right | Toggle button turns blue, enables Page load time |
| 4 | Locate **LinkMiner** extension in the list and click the toggle to the right | Toggle button turns blue, enables LinkMiner |
| 5 | Locate **BrowserStack** and click the toggle to the right | Toggle button turns blue, enables **BrowserStack** |
{% endtab %}

{% tab title="2 - SET UP LOG STREAMS " %}
### SET UP LOG STREAMS

Monitor log streams when running a Regression test

{% embed url="https://docs.acquia.com/acquia-cloud/monitor/logstream/" %}

| ACTIVITY | EXPECTED BEHAVIOR |
| :--- | :--- |
| Open a new browser window, Click on link below to sign into Acquia cloud account: | Opens to Applications page |
| [https://cloud.acquia.com/app/develop](https://cloud.acquia.com/app/develop) |  |
| Click on the Application for the site youre currently testing on | Opens to Environments page |
| Click on the Environment you're testing \(choose between Dev, Stage, Prod or Uat\) | Opens to the Overview page |
| Click on Logs in the left hand sidebar | Opens to the Streams tab on the Logs page |
| Click Stream above the log stream box | Opens up check box list providing options to select which logs to stream |
| Select the check boxes beside apache-error, php-error, and drupal-watchdog | Logs page stream will show log entries from the selected logs |
{% endtab %}

{% tab title="3 - LOG INTO DRUPAL" %}
| **STEP** | LOG INTO DRUPAL / GO TO TESTING ENVIRONMENT | \(If not regression testing on production\) |
| :--- | :--- | :--- |
| 1 | Select the URL for the testing environment in the dropdown menu. Enter your log in credentials | Takes you to My Workbench page |
| 2 | Click on the white Home icon button in the admin toolbar | Opens to Homepage at [www.boston.gov](www.boston.gov) |
{% endtab %}
{% endtabs %}



