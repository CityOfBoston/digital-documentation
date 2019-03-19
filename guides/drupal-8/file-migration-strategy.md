# File Migration Strategy

I have written the file migration assuming that the process for performing the migration will work like this: 

* Archive the public files directory on D7 and store it somewhere outside of Acquia. This will allow us to retrieve missing files after the migration is over if we need to.
* Rsync files to `/sites/default/files/migration`
* Run the migration with `drush mim managed_files`
* When migration is done and site is launched, delete all files in `/sites/default/files/migration` as they are no longer needed.

Other Notes:

* We have created a managed file for each entry in the `file_managed` table of the D7 site.
* For any files in the root of the public files directory, we have updated the file URI to `sites/default/files/{hash(filename)}/{filename}`
* We have written a process plugin for rich text fileds that switches  `<a>` and `<img>` tags that reference files on the local file system  over to entity embeds. This is inserted at the end of each migration pipeline for rich text fields programatically.

