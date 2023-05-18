# Backend

The legacy back-end sits mainly on a MSSQL Server, with some data manipulation performed by asp pages on the IIS server

As a first step, it is possible to strip all business logic and processing from the SQL server, and extract to a module within Drupal.

The best way to do this might be to create a rest service which is capable of subscribing and unsubscribing users, and processing alerts.

Then a front end can be created to call the various endpoints of the REST service to query, add and remove subscriberse
