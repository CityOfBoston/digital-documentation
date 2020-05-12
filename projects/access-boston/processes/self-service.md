---
description: Edit and commit changes to the items displayed in Access-Boston Dashboard
---

# Self-Service

We created this [repository](https://github.com/CityOfBoston/access-boston-config) in Github to manage changes to the Access-Boston dashboard by editing the config files for each of the environments runs on. The following are the are steps needed to commit changes, this repository will then automatically notify the digital team that a new deploy to AWS is ready to be kicked off.

**Edit Process**

![https://raw.githubusercontent.com/CityOfBoston/access-boston-config/master/src/images/Screenshot1.png](https://raw.githubusercontent.com/CityOfBoston/access-boston-config/master/src/images/Screenshot1.png)

1. From the repository landing page, edit the config file for the environment \(dev/test/prod\) you want to change, by going from the 'src' and the 'config' folder; the click on the folder for the environment you want to edit.

   ![https://raw.githubusercontent.com/CityOfBoston/access-boston-config/master/src/images/Screenshot1.png](../../../.gitbook/assets/configs_dir.png)

2. Click on the 'apps.yaml' file, from the details view click the 'Edit this File' icon.

   ![https://raw.githubusercontent.com/CityOfBoston/access-boston-config/master/src/images/apps\_file.png](../../../.gitbook/assets/apps_file.png)

   ![https://raw.githubusercontent.com/CityOfBoston/access-boston-config/master/src/images/edit.png](../../../.gitbook/assets/edit.png)

   Adding new links require 3 of the following fields:

   * **title**
   * **url**
   * \***groups**: _Groups is a list of groups of people with access that application. The formatting should follow this style: groups_
   * \***icon**: _Icon is require for links in the 'Apps' section, at the top of the file_

3. When you're done making changes, go to the bottom of the page where it says 'Commit Changes' and provide a name and description for the changes made.

   ![https://raw.githubusercontent.com/CityOfBoston/access-boston-config/master/src/images/commit.png](../../../.gitbook/assets/commit.png)

4. Leave the "Commit directly to the 'master' branch" radio button checked
5. When you're done, hit the "Commit Changes" button

   ![https://raw.githubusercontent.com/CityOfBoston/access-boston-config/master/src/images/commit\_btn.png](../../../.gitbook/assets/commit_btn.png)

6.  No you can go back to either the [homepage](https://github.com/CityOfBoston/access-boston-config) or [commits page](https://github.com/CityOfBoston/access-boston-config/commits/master) to view this commits progress.

Homepage: 

![https://raw.githubusercontent.com/CityOfBoston/access-boston-config/master/src/images/homepage\_commit.png](../../../.gitbook/assets/homepage_commit.png)

Commits Page: 

![https://raw.githubusercontent.com/CityOfBoston/access-boston-config/master/src/images/commits\_page.png](../../../.gitbook/assets/commits_page.png)

If you see a yellow dot next to the commit, its still being processed, once its done the dot will check to a green check mark if it passed or a red x if it failed.

Passed:

![https://raw.githubusercontent.com/CityOfBoston/access-boston-config/master/src/images/commits\_page\_pass.png](https://raw.githubusercontent.com/CityOfBoston/access-boston-config/master/src/images/commits_page_pass.png)

Failed:

![https://raw.githubusercontent.com/CityOfBoston/access-boston-config/master/src/images/commits\_page\_fail.png](https://raw.githubusercontent.com/CityOfBoston/access-boston-config/master/src/images/commits_page_fail.png)

Once the build for the commit passes, our build integration with Travis notifies Digital Team will be notified via Slack that we can restart the application on AWS.

