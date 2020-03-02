# Redirecting from cityofboston.gov

When we redirect something from cityofboston.gov we should both redirect and then two weeks later delete the content, files, and code.

We will need to rethink where we put in the redirect so that we can indeed delete the content, files, and code.

For now, redirects can be done by doing the following on cityofboston.gov / zpcobweb01 server. Some are asp files; some are html files.

**ASP Redirect**

Include this at the top of the file:   
&lt;% Response.CacheControl="max-age=120, Public" Response.CharSet = "utf-8" Response.Status="301 Moved Permanently" Response.AddHeader "Location","[https://www.boston.gov/finance/boston-about-results](https://www.boston.gov/finance/boston-about-results) \[BOSTON.GOV LOCATION TO REDIRECT TO\]"  
%&gt;

**HTML Redirect**

Include t his in the &lt;head&gt; tag:  
&lt;meta http-equiv="Refresh" content="0; url=[https://www.boston.gov/finance/boston-about-results](https://www.boston.gov/finance/boston-about-results) \[BOSTON.GOV LOCATION TO REDIRECT TO\]" /&gt;

  




