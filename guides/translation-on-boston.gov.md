---
description: >-
  This page documents implementation of the Google Translate Basic API on
  boston.gov.
---

# Translation on boston.gov

## Background

Previously, the Bing Translate Web Widget \(a free javascript widget\) was used to translate content on boston.gov. The widget was deprecated in July 2019. A new translation solution is required. 

The free Google Translate JavaScript Widget is no longer being serviced and while it is still usable, the translations it yields proved too poor to move forward with this as viable option for Boston.gov. As an alternative, Google offers two versions of their Cloud Translation API: Basic \(i.e. pre trained \) and Advanced API \(i.e. uses your domain specific data, training required\). 

###  Basic API 

Translates text that appears between tags. The output retains the \(untranslated\) HTML tags, with the translated text between the tags to the extent possible due to differences between the source and target languages. The order of HTML tags in the output may differ from the order in the input text due to word order changes in the translation. 

Using just an API KEY will make a RESTful API call to the server. This would require multiple API calls, and thus would prove costly on high traffic pages. Using the required cloud libraries would require fewer API calls with static pages stored on the server.



### Advanced API 

Cloud Translation - Advanced supports translating text using custom AutoML Translation models, and for creating glossaries to ensure that the Translation translates a customer's domain-specific terminology correctly.

Before you can use Cloud Translation - Advanced, you must enable the AutoML API \(automl.googleapis.com\) if you want to use AutoML custom models for your project. If you plan to use a glossary or the batch features, you also need to create a Google Cloud Storage bucket and grant your service account access to it.

Based on feature/price comparisons, we decided to move forward with the Basic API.  

There are some municipalities that use the Google Translate web translator directly, rather than the API or widget, to translate content on their sites. The State of Maryland's website, for example, employs a small bit of Javascript which provides users with a Translate button and drop down in the site header. When a user selects a language in the dropdown, all the text is run through the web translator and a translated version of the page is displayed. There is no call to an API, thus no cost. Example:

{% embed url="http://translate.google.com/translate?hl=en&sl=en&u=https://www.maryland.gov/Pages/default.aspx&tl=pt" %}

This solution has zero cost, but does not offer the same flexibility in terms of implementation nor the ability to incorporate domain specific data to populate glossaries. 

Based on all the available options, we decided to move forward with a short and long term translation strategy.

## Short term translation strategy

Use translate.google.com to provide translations for all languages provided/supported by the tool. Users should be able to translate any page or document \(on pages\) after selecting a language of their choice.

There is no cost associated with translation in this first iteration, given that we are proposing to use the free Google Translate web translator.

This short term strategy has no effect on any of our current content creation workflows.

##  Long term translation strategy

Integrate Basic Google Translate  API into current Drupal workflow such that when a new English language page is created in Drupal, multilingual copies of that page can be automatically generated, saved as drafts, and then subsequently quality checked and published by translators. 

The cost of translation for the Basic API is $20 per million characters translated. Because we are only proposing to translate pages at the moment of publication, and then saving those translated pages as unique nodes in Drupal, the translation cost for this implementation of Google Translate API will likely be significantly lower than the cost of translating pages per user requests. 

This long term strategy requires some changes to our current content creation workflow: 

![Future workflow](../.gitbook/assets/image%20%2820%29.png)





 

