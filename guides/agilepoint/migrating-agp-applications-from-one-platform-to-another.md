# Migrating AGP Applications from one platform to another

There are 3 AgilePoint environments. DEVELOPMENT (https://boston-d-app01.nxone.com) - Used for creating applications TEST (https://boston-s-app.nxone.com) - Used for end-user testing PRODUCTION (https://boston.nxone.com) - Used for “live” applications

Applications are created in the DEV environment and then moved to the TEST environment for end-user testing. If changes are required then the app should be updated in DEV and then when ready for additional testing migrated again to TEST.

There should not be any creation or updating of apps in TEST or PRODUCTION environments.

When updating or making changes to an existing application, this should be done in DEV, and then moved to TEST as well as PROD. Even if no end user testing for the change/update is required. E.g. If you are only making cosmetic changes to wording etc the updated app should be moved into TEST as well.

_NOTE: If there are one or more Data Entities involved with an application, and there usually is, these need to be migrated to the TEST & PRODUCTION environments first before migrating the application. You can only migrate a Data Entity once. If you need to add additional fields to the data entity after it has been migrated, then you will need to add the field manually in each environment. So - before migrating, try & make sure the Data Entity is final or as near final as can be as adding fields manually in the additional two environments can be time consuming._

Migrating an app to another environment requires several steps&#x20;

DEV Environment&#x20;

Publish the application. Highlight the app you want to migrate&#x20;

(1) Click Publish&#x20;

(2)

