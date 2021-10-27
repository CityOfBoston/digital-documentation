# Verify Installation

If the installation has completed without errors, then you should be able to check the following:

The repo that was checked out in Step 1 of the [installation instructions](../) is hosted on your dev computer, and is mounted into _**each**_ of the docker containers.  As you make changes to the files on your dev computer, they are instantly updated in all of your local docker containers. &#x20;

### Local boston.gov website

The production/public website is hosted by Acquia and can be accessed at https://www.boston.gov.

The local development version of the public website can be viewed at: **https://boston.lndo.site.  **This local copy of the Drupal website is served (by Apache) from the `appserver`docker container, and its content is stored and retrieved from a MySQL database in the `database` docker container.

You will find the **CityOfBoston/patterns** repo cloned into the `root/patterns` folder on your host dev computer.

### Local Fleet website

The production/public patterns library is hosted by City of Boston from our AWS/S3 infrastructure and can be accessed at https://patterns.boston.gov.

The local development version of the patterns library is hosted by Fleet and can be viewed at **https://patterns.lndo.site**. This local copy of the Fleet website is served (by Node/Fractal) from the `patterns`docker container.

You will find the **CityOfBoston/patterns** repo cloned into the `root/patterns` folder on your host dev computer.

The gulp, stencil, fractal and other services running in the`patterns` docker container will automatically build the local fleet static website into `root/patterns/public` from the underlying files in real-time as they are changed.

### Local Patterns CDN

The production/public patterns CDN is hosted by City of Boston from our AWS/S3 infrastructure at https://patterns.boston.gov.

The local development version of the CDN is hosted by Fleet at **https://patterns.lndo.site**. This local CDN is served (by Node/Fractal) from the `patterns`docker container.

You will find the **CityOfBoston/patterns** repo cloned into the `root/patterns` folder on your host dev computer.

The gulp, stencil, fractal and other services running in the`patterns` docker container will automatically build the local fleet static website into `root/patterns/public` from the underlying files in real-time as they are changed.
