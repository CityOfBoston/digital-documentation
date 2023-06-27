---
description: Outline which parts of the application each team is responsible for.
---

# Ownership of Concerns



<table><thead><tr><th width="356.5">Digital Team</th><th>Security Team</th></tr></thead><tbody><tr><td><p><strong>Presentation Logic</strong> </p><p>i.e Application design, UI elements, UX (functionality), etc</p></td><td><p><strong>Business Logic</strong> </p><p>i.e sorting, password change, session login, changes to application data, etc</p></td></tr><tr><td>Data Requests (from Security Endpoints)</td><td>Endpoint/API Access</td></tr><tr><td>Data Formatting (for display)</td><td>Data Formating</td></tr><tr><td><p><strong>Application Session</strong> </p><p>Store user data after login in application session, used to hold tokens for password change, etc</p></td><td><p><strong>Password Validation</strong> (Server side)</p><p>Compare against previous used password, etc</p></td></tr><tr><td>UI shows error message when accessing endpoint fails</td><td>GENERATE error when authentication fails for any reason</td></tr><tr><td>UI/UX informs user of missing/errors on input fields</td><td>GENERATE error when user account not set up properly</td></tr><tr><td>REPORT error generated by endpoint (Rollbar tracking)</td><td>GENERATE error when FID not functioning properly</td></tr><tr><td>REPORT error generated by UX to Rollbar</td><td>Certification changes, errors</td></tr></tbody></table>


