---
description: >-
  City of Boston support development of discrete React (and other JS framework)
  WebApps.  Because these services will be hosted on Drupal there is a custom
  Drupal webapp launcher and some conventions to
---

# WebApps

## Prerequisites:

1. Have stable local build of Drupal 8 website running on your machine.
2. Make sure you are “logged in” or have “admin” access to view the CMS and add new content / nodes. 
   * Using Drush: `lando drush uli`
   * Using Drupal web login: [https://boston.lndo.site/user/login?local](https://boston.lndo.site/user/login)

## Part 1: Drupal CMS

1. Navigate to Content menu item \(make sure you are logged into Drupal to view\) [https://boston.lndo.site/admin/content](https://boston.lndo.site/admin/content)
2. Scroll to bottom of the page and add content item by clicking “Add Content”, . Select “Listing Page” content type.
3. Give new Page content a Title. This is required.
4. Click on the “Components” tab on the left menu
5. Find the dropdown Select menu to add “new component” and select “Web App” from the list.
6. Name the Web App something appropriate as it relates to your project. \(i.e. Metrolist or My Neighborhood\)
7. Click “Save” near the bottom or side of the page to save and create a new page / node. This will serve as the container page / component for your new web app.

## Part 2: Drupal file system

1. Navigate to the “bos\_web\_app”  directory of the drupal 8 repository that is checked out to your local machine /docroot/modules/custom/bos\_components/modules/bos\_web\_app. 
2. Locate the “apps” folder / directory. If one doesn’t exist, please create. /docroot/modules/custom/bos\_components/modules/bos\_web\_app/apps
3. Inside this “apps” directory create an empty folder and name it the same name you called your Web App in Step 6 of Part 1 above. /docroot/modules/custom/bos\_components/modules/bos\_web\_app/apps/my\_neighborhood **NOTE:** Any spaces in your app name should be treated with underscores. For example, My Neighborhood  would have a folder name of “my\_neighborhood”.
4. Locate and open the libraries yml file named “bos\_web\_app.libraries.yml”. This file will serve as the pointer and compiler that will tell Drupal to attach and bunde all your JS and CSS files for your application. /docroot/modules/custom/bos\_components/modules/bos\_web\_app/bos\_web\_app.libraries.yml
5. Once you have the libraries file open, add an entry with the name of your application and add / attach necessary items to your application. For example, the application “My Neighborhood” would have a library entry as such…  
   ![](https://lh5.googleusercontent.com/f8HeVg3x8LuOresFakv91GV235RsqG1XP7MeWM04-yWuOruFgK-y9nKcvSr3c_HzSYY73IgU0MOLSeld6Zb_ieZZ4M50FMg28wPBsPxogboYpKhHBLG1h9i5O7l_j5_gN1vn7sCE)

   See an example libraries.yml file on GitHub for a project that is currently being developed.  
   [https://github.com/CityOfBoston/boston.gov-d8/blob/mnl\_12-9-2019/docroot/modules/custom/bos\_components/modules/bos\_web\_app/bos\_web\_app.libraries.yml](https://github.com/CityOfBoston/boston.gov-d8/blob/mnl_12-9-2019/docroot/modules/custom/bos_components/modules/bos_web_app/bos_web_app.libraries.yml)  
   Drupal also has good documentation on using libraries and attaching files.  
   [https://www.drupal.org/docs/8/creating-custom-modules/adding-stylesheets-css-and-javascript-js-to-a-drupal-8-module  
   ](https://www.drupal.org/docs/8/creating-custom-modules/adding-stylesheets-css-and-javascript-js-to-a-drupal-8-module) 

6. Once you have libraries file setup, go create the files needed, OR first create the files you’d like and then add them to the libraires.yml as laid out in Part 2 - Step 5 above.
7. It’s important to note that any time you add a new attached  file to libraires.yml, the Drupal cache will have to be cleared for changes to take effect. You can clear the cache either through the Drupal CMS or via Drupal drush CLI
   * Drupal CMS:  navigate to admin/config/development/performance, and click button at top of the page labeled “clear all caches”
   * Using Drush CLI: drush cr
8. After clearing the cache, you should now see your application load on the Drupal page you created and saved in Part 1 - Step 7. **NOTE:** You will **NOT** have to clear the Drupal cache every time you make a change to a CSS or JS file. This is only for new items in the libraires.yml file. 

