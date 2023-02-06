---
description: >-
  This page documents implementation of the Google Translate Basic API on
  boston.gov.
---

# Translation on boston.gov

## Background

Previously, the Bing Translate Web Widget (a free javascript widget) was used to translate content on boston.gov. The widget was deprecated in July 2019. A new translation solution is required.&#x20;

The free Google Translate JavaScript Widget is no longer being serviced and while it is still usable, the translations it yields proved too poor to move forward with this as viable option for Boston.gov. As an alternative, Google offers two versions of their Cloud Translation API: Basic (i.e. pre trained ) and Advanced API (i.e. uses your domain specific data, training required).&#x20;

### &#x20;Basic API&#x20;

Translates text that appears between tags. The output retains the (untranslated) HTML tags, with the translated text between the tags to the extent possible due to differences between the source and target languages. The order of HTML tags in the output may differ from the order in the input text due to word order changes in the translation.&#x20;

Using just an API KEY will make a RESTful API call to the server. This would require multiple API calls, and thus would prove costly on high traffic pages. Using the required cloud libraries would require fewer API calls with static pages stored on the server.



### Advanced API&#x20;

Cloud Translation - Advanced supports translating text using custom AutoML Translation models, and for creating glossaries to ensure that the Translation translates a customer's domain-specific terminology correctly.

Before you can use Cloud Translation - Advanced, you must enable the AutoML API (automl.googleapis.com) if you want to use AutoML custom models for your project. If you plan to use a glossary or the batch features, you also need to create a Google Cloud Storage bucket and grant your service account access to it.

Based on feature/price comparisons, we decided to move forward with the Basic API. &#x20;

There are some municipalities that use the Google Translate web translator directly, rather than the API or widget, to translate content on their sites. The State of Maryland's website, for example, employs a small bit of Javascript which provides users with a Translate button and drop down in the site header. When a user selects a language in the dropdown, all the text is run through the web translator and a translated version of the page is displayed. There is no call to an API, thus no cost. Example:

{% embed url="http://translate.google.com/translate?hl=en&sl=en&u=https://www.maryland.gov/Pages/default.aspx&tl=pt" %}

This solution has zero cost, but does not offer the same flexibility in terms of implementation nor the ability to incorporate domain specific data to populate glossaries.&#x20;

Based on all the available options, we decided to move forward with a short and long term translation strategy.

## Short term translation strategy

Use translate.google.com to provide translations for all languages provided/supported by the tool. Users should be able to translate any page or document (on pages) after selecting a language of their choice.

There is no cost associated with translation in this first iteration, given that we are proposing to use the free Google Translate web translator.

This short term strategy has no effect on any of our current content creation workflows.

### Issues

1.  **Pre-translated content**

    We encountered an issue with existing multilingual content on boston.gov, i.e. pre-translated pages in Drupal. When, for example, user visits the boston.gov homepage and selects a given language using the translate button in the site header, the URL of the homepage (www.boston.gov) is run through the Google web translator and a translated version of the homepage is displayed to the user. When the user subsequently navigates through the site, new pages openedy the user are also opened in the Google web translator. Thus, a user could navigate from the homepage (post-translation) to the one of the existing multilingual pages on boston.gov. In this case, the Google web translator will re-translate the translated text, even if the translation setting of the translator and the language of the text on the page are  the same. And most importantly, there are differences in the two version of the text, i.e. between the pre-translated text (translated by a human) and the re-translated pre-translated text (re-translated by Google web translator). To solve this issue, we followed these steps:

## &#x20;Long term translation strategy

Integrate Basic Google Translate  API into current Drupal workflow such that when a new English language page is created in Drupal, multilingual copies of that page can be automatically generated, saved as drafts, and then subsequently quality checked and published by translators.&#x20;

The cost of translation for the Basic API is $20 per million characters translated. Because we are only proposing to translate pages at the moment of publication, and then saving those translated pages as unique nodes in Drupal, the translation cost for this implementation of Google Translate API will likely be significantly lower than the cost of translating pages per user requests.&#x20;

This long term strategy requires some changes to our current content creation workflow:&#x20;

![Future workflow](<../../.gitbook/assets/image (20).png>)



### Metrolist REACT pages issue resolution

While Metrolist loads under the offsite version of Google Translate, the React views (Search and AMI Estimator) do not populate.

#### Background

* React Router matches on the current URL (`window.location` or `document.location`).
* The Google Translate widget loads the entire page into an `iframe`.
* Under normal circumstances, the React page loading inside of an `iframe` would not break anything, since `iframe`s are self-contained. The `location` would still be e.g. `https://www.boston.gov/metrolist/search` even if included on another domain. However, Google needs to modify the content on the page in order to translate it, and it isn’t possible to modify the contents of an `iframe` from the parent page (unless they talk to each other using `postMessage`). Therefore, Google merely scrapes the content of the included page and dynamically inserts it into an `iframe` that it controls.
*   Because of the above process, the `location` under Google Translate is not `www.boston.gov` but rather `translate.googleusercontent.com`. The path of the page becomes `/translate_c`, which throws off React Router matching that expects `/metrolist`.

    > index.bundle.js?v=2.x:2 Warning: You are attempting to use a basename on a page whose URL path does not begin with the basename. Expected path "/translate\_c?depth=1\&pto=aue\&rurl=translate.google.com\&sl=auto\&sp=nmt4\&tl=ja\&u=[https://www.boston.gov/metrolist/ami-estimator\&usg=ALkJrhjYWXizTPU7YYBqcKUYUV0LgW-l5g](https://www.boston.gov/metrolist/ami-estimator\&usg=ALkJrhjYWXizTPU7YYBqcKUYUV0LgW-l5g)" to begin with "/metrolist".
*   Google Translate also adds a `base` tag to the page’s `head` set to the original URL (e.g. `<base href="https://www.boston.gov/metrolist/ami-estimator" />`. This is to make sure relative links won’t break from being on a different domain. Ironically this breaks navigation for Single-Page Apps, which use the HTML5 History API rather than doing a real server request. History API cannot update `location`s across domains:

    > Uncaught DOMException: Failed to execute 'pushState' on 'History': A history state object with URL '[https://www.boston.gov/metrolist/ami-estimator/household-income](https://www.boston.gov/metrolist/ami-estimator/household-income)' cannot be created in a document with origin '[https://translate.googleusercontent.com](https://translate.googleusercontent.com/)' and URL '[https://translate.googleusercontent.com/translate\_c?depth=1\&pto=aue\&rurl=translate.google.com\&sl=auto\&sp=nmt4\&tl=ja\&u=https://www.boston.gov/metrolist/ami-estimator\&usg=ALkJrhjWcZACKPBWvA4U6iyZm2NCx47XBw](https://translate.googleusercontent.com/translate\_c?depth=1\&pto=aue\&rurl=translate.google.com\&sl=auto\&sp=nmt4\&tl=ja\&u=https://www.boston.gov/metrolist/ami-estimator\&usg=ALkJrhjWcZACKPBWvA4U6iyZm2NCx47XBw)'.
* Clicking the link on `/metrolist/ami-estimator/result` to `/metrolist/search` from within Google Translate is not possible since Google puts an `X-Frame-Options` security header on the `iframe`.

#### Solution

* The top-level basename of `/metrolist` was removed and the routes were updated from `/`, `/search`, and `/ami-estimator` to `/metrolist/`, `/metrolist/search`, and `/metrolist/ami-estimator` respectively. This removed the basename mismatch console warning, but it did not fix the routing.
* We detect whether we are inside a Google Translate `iframe`, i.e. if the current domain is `translate.googleusercontent.com`—which should match 100% of the time, but in case that were to change we also check for `translate.google.com` or the path `/translate_c`—and if there is a query string present. If both conditions are met, we scan for a query string parameter pointing to `/metrolist/*`, then extract the path from the first match. Google Translate re-hosts the page content by scraping whatever is specified in the `u` parameter (“u” for “URL” most likely), e.g. `u=https://www.boston.gov/metrolist/search`. Given that parameter is found, we can extract `/metrolist/search` and then manually override the React Router location to think it is on `/metrolist/search` even if it is actually on `/translate_c`.
* Additionally, we store references to the two Google URLs in localStorage (`metrolistGoogleTranslateUrl` and `metrolistGoogleTranslateIframeUrl`) for later use.
* On forward/back navigation between AMI Estimator subroutes, we temporarily change the `base` from `https://www.boston.gov/metrolist/ami-estimator` (or equivalent dev environment) to `https://translate.googleusercontent.com/metrolist/ami-estimator`. Even though the latter URL does not exist, it satisfies the necessary security conditions for navigation by keeping us on the same domain. Then, after navigating, the `base` is immediately changed back to `boston.gov` so links and assets do not break.
*   Finally, the link on `/metrolist/ami-estimator/result` to `/metrolist/search` is swapped out with a new Google Translate URL. If left alone, then the untranslated Search page would load inside the `iframe`. So we read `localStorage.metrolistGoogleTranslateUrl` and replace the `u` parameter with the equivalent `/metrolist/search` URL for whataver domain it’s on. This URL has to be read from localStorage because if we try to read `window.parent.location.href` it will be blocked for security reasons:

    > Uncaught DOMException: Blocked a frame with origin "[https://translate.googleusercontent.com](https://translate.googleusercontent.com/)" from accessing a cross-origin frame.\
    > It also has to be loaded in a new tab/window with `<a target="_blank"></a>` because otherwise we get another security error:\
    > Refused to display '[https://translate.google.com/translate?depth=1\&pto=aue\&rurl=translate.google.com\&sl=auto\&sp=nmt4\&tl=ja\&u=https://www.boston.gov/metrolist/search](https://translate.google.com/translate?depth=1\&pto=aue\&rurl=translate.google.com\&sl=auto\&sp=nmt4\&tl=ja\&u=https://www.boston.gov/metrolist/search)' in a frame because it set 'X-Frame-Options' to 'deny'.

#### Caveats

* If Google Translate changes the way their code works, this could break.
* Although this fix is verifiable on CI as far as translation goes, until the appropriate CORS headers are added to Acquia, the parts of the app that rely on API data will not resolve, so it will still appear broken. Although this also has to do with cross-origin restrictions, it is completely unrelated to the Translate issue, so it is safe to ignore. But to work around this and verify that the site is indeed 100% working, you can run Chrome without security enabled. Download Chrome Canary and run this command (macOS, but you can search for your platform equivalent): `open -n -a Google\ Chrome\ Canary --args --disable-web-security --user-data-dir=/tmp/chrome --disable-site-isolation-trials --allow-running-insecure-content`.



&#x20;
