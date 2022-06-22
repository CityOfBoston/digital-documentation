---
description: API Feed for Status Items from boston.gov.
---

# Bos311 API

{% swagger method="get" path="" baseUrl="/api/v1/status_items" summary="Provides a list of the current status items and messages as they display on the site." %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-response status="200: OK" description="" %}
```javascript
[
  {
    "id":"11564601",
    "enabled":"True",
    "title":{
      "en":"COVID-19 vaccine information"
    },
    "body":{
      "en":"We have vaccine safety information, as well as a map and list of vaccination sites for eligible residents in Boston."},"priority":"1","alert":"False","published_at":"2020-03-19T11:17:21-0400","updated_at":"2022-06-17T00:00:01-0400"},{"id":"13622986","enabled":"True","title":{"en":"COVID-19 testing sites"},"body":{"en":"We've partnered\u00a0with community health centers, hospitals, and pharmacies to increase access to COVID-19 testing."
    },
    "priority":"2",
    "alert":"False",
    "published_at":"2020-05-15T07:50:46-0400",
    "updated_at":"2022-06-17T00:00:01-0400"
  },
  {
    "id":"11555366",
    "enabled":"True",
    "title":{
      "ht":"Tow lot",
      "pt-br":"Reboque",
      "ar":"\u0633\u062d\u0628 \u0644\u0648\u0637",
      "ru":"\u042d\u0432\u0430\u043a\u0443\u0430\u0442\u043e\u0440",
      "en":"Tow lot",
      "so":"Jiid badan",
      "es":"lote de remolque",
      "zh-hans":"\u62d6\u8f6690",
      "fr":"Lot de remorquage"
    },
    "body":{
      "ht":"Remorquage a ouvri s\u00e8lman lendi jiska vandredi, ant 7 a.m. ak 10:30 p.m. Kontwar otomatik yo disponib 24 \u00e8dtan pa jou, s\u00e8t jou pa sem\u00e8n pou degaje machin yo. Plis enfomasyon: https:\/\/d8-dev2.boston.gov\/node\/2486",
      "pt-br":"O reboque funciona apenas de segunda a sexta-feira, das 7h \u00e0s 22h30. Quiosques automatizados est\u00e3o dispon\u00edveis 24 horas por dia, sete dias por semana para libera\u00e7\u00e3o de ve\u00edculos. Mais Informa\u00e7\u00f5es: https:\/\/d8-dev2.boston.gov\/node\/2486",
      "ar":"\u062a\u0641\u062a\u062d \u0633\u0627\u062d\u0629 \u0627\u0644\u0633\u062d\u0628 \u0641\u0642\u0637 \u0645\u0646 \u0627\u0644\u0627\u062b\u0646\u064a\u0646 \u0625\u0644\u0649 \u0627\u0644\u062c\u0645\u0639\u0629 \u060c \u0645\u0646 \u0627\u0644\u0633\u0627\u0639\u0629 7 \u0635\u0628\u0627\u062d\u064b\u0627 \u062d\u062a\u0649 10:30 \u0645\u0633\u0627\u0621\u064b. \u0627\u0644\u0623\u0643\u0634\u0627\u0643 \u0627\u0644\u0622\u0644\u064a\u0629 \u0645\u062a\u0627\u062d\u0629 \u0639\u0644\u0649 \u0645\u062f\u0627\u0631 24 \u0633\u0627\u0639\u0629 \u0641\u064a \u0627\u0644\u064a\u0648\u0645 \u060c \u0633\u0628\u0639\u0629 \u0623\u064a\u0627\u0645 \u0641\u064a \u0627\u0644\u0623\u0633\u0628\u0648\u0639 \u0644\u0625\u0637\u0644\u0627\u0642 \u0627\u0644\u0645\u0631\u0643\u0628\u0627\u062a. \u0645\u0639\u0644\u0648\u0645\u0627\u062a \u0627\u0643\u062b\u0631: https:\/\/d8-dev2.boston.gov\/node\/2486",
      "ru":"\u0410\u0432\u0442\u043e\u0441\u0442\u043e\u044f\u043d\u043a\u0430 \u0440\u0430\u0431\u043e\u0442\u0430\u0435\u0442 \u0442\u043e\u043b\u044c\u043a\u043e \u0441 \u043f\u043e\u043d\u0435\u0434\u0435\u043b\u044c\u043d\u0438\u043a\u0430 \u043f\u043e \u043f\u044f\u0442\u043d\u0438\u0446\u0443 \u0441 7:00 \u0434\u043e 22:30. \u0410\u0432\u0442\u043e\u043c\u0430\u0442\u0438\u0437\u0438\u0440\u043e\u0432\u0430\u043d\u043d\u044b\u0435 \u043a\u0438\u043e\u0441\u043a\u0438 \u0434\u043e\u0441\u0442\u0443\u043f\u043d\u044b 24 \u0447\u0430\u0441\u0430 \u0432 \u0441\u0443\u0442\u043a\u0438, \u0441\u0435\u043c\u044c \u0434\u043d\u0435\u0439 \u0432 \u043d\u0435\u0434\u0435\u043b\u044e \u0434\u043b\u044f \u0432\u044b\u043f\u0443\u0441\u043a\u0430 \u0442\u0440\u0430\u043d\u0441\u043f\u043e\u0440\u0442\u043d\u044b\u0445 \u0441\u0440\u0435\u0434\u0441\u0442\u0432. \u0414\u043e\u043f\u043e\u043b\u043d\u0438\u0442\u0435\u043b\u044c\u043d\u0430\u044f \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0446\u0438\u044f: https:\/\/d8-dev2.boston.gov\/node\/2486",
      "en":"The tow lot is only open Monday through Friday, from 7 a.m. - 10:30 p.m. Automated kiosks are available 24 hours a day, seven days a week for vehicle releases. More Information: https:\/\/d8-dev2.boston.gov\/node\/2486",
      "so":"Goobtu waxay furan tahay Isniin ilaa Jimce, laga bilaabo 7 subaxnimo - 10:30 galabnimo. Dukaamada otomaatiga ah ayaa diyaar ah 24 saacadood maalintii, todobada maalmood ee usbuuca si baabuurta loo sii daayo. Macluumaad Dheeraad ah: https:\/\/d8-dev2.boston.gov\/node\/2486",
      "es":"El lote de remolque solo est\u00e1 abierto de lunes a viernes, de 7 a. m. a 10:30 p. m. Los quioscos automatizados est\u00e1n disponibles las 24 horas del d\u00eda, los siete d\u00edas de la semana para la entrega de veh\u00edculos. M\u00e1s informaci\u00f3n: https:\/\/d8-dev2.boston.gov\/node\/2486",
      "zh-hans":"\u62d6\u8f66\u573a\u4ec5\u5728\u5468\u4e00\u81f3\u5468\u4e94\u4e0a\u5348 7 \u70b9\u81f3\u665a\u4e0a 10:30 \u5f00\u653e\u3002 \u81ea\u52a8\u552e\u8d27\u4ead\u6bcf\u5468 7 \u5929\u3001\u6bcf\u5929 24 \u5c0f\u65f6\u63d0\u4f9b\u8f66\u8f86\u53d1\u5e03\u670d\u52a1\u3002 \u66f4\u591a\u4fe1\u606f: https:\/\/d8-dev2.boston.gov\/node\/2486",
      "fr":"Le d\u00e9panneur est ouvert uniquement du lundi au vendredi, de 7 h \u00e0 22 h 30. Des kiosques automatis\u00e9s sont disponibles 24 heures sur 24, sept jours sur sept pour les livraisons de v\u00e9hicules. Plus d'information: https:\/\/d8-dev2.boston.gov\/node\/2486"
    },
    "priority":"7",
    "alert":"False",
    "published_at":"2020-01-07T14:43:17-0500",
    "updated_at":"2022-06-17T00:00:01-0400"
  }
]
```
{% endswagger-response %}
{% endswagger %}

## Overview

**Status Items** are information cards which are maintained by the website Content Editors to advise constituents on the status of various (usually physical) city services.

Conceptually, a status item relates to a city service (parking, street sweeping, city office opening hours etc) and contains a set of messages that can be displayed at different times.&#x20;

The only place we presently display status items is on the homepage.&#x20;

The API is consumed by the ConnectedBits mobile App, which calls the API periodically to get an update on the status items.  The ConnectedBits App checks for added/deleted status items and the `updated_at`field from the API and alters content on the App as necessary.  The response format is customised to meet requirements provided by ConnectedBits in this document.

{% file src="../../../.gitbook/assets/posts api.pdf" %}
Connected Bits API Consumer Specification
{% endfile %}

## Status Item Construction

The main status item entity is a **node** called `status_item`. The status\_item node contains:

* a title field which is the name of the status item,&#x20;
* a link to an icon,&#x20;
* a field to enable/disable the status item, and&#x20;
*   a collection of messages to show for the status item.&#x20;

    Each message in the collection is a message\_for\_the\_day **paragraph** entity which contains:

    * the text for the message and&#x20;
    * information on when the message will be displayed.&#x20;

## API Construction

The API is implemented using a **view** **display** (view:`status_items` - display: `bos_311_motd_api`\[Bos311]).  The view handles the selection and filtering of the information that will be provided by the API.

There is a small amount of pre-processing code in the `node_status_item.module` and `bos_messages.module` to validate and de-duplicate records to be usre the output matches that which is displayed on the site at the time the API is called..

There is a Drupal **View Formatter** (a style plugin) `Bos311Serializer.php` which is used to re-organize the fields once the view has completed processing. This class does not filter or otherwise alter the view output other than ensuring the json result is organized in the format required by the ConnectedBits specification document (above).&#x20;

## Translations

{% embed url="https://cloud.google.com/translate" %}
Googles online translation GUI
{% endembed %}

The&#x20;

## Reference Docs

{% embed url="https://docs.google.com/document/d/1z7mFI2VHb0oXTq11P2lr2kxeDfJmgW7BeLjVxlCs4YE/edit" %}
Original Specification Document.
{% endembed %}

{% embed url="https://cloud.google.com/translate/docs/setup" %}
Google Translate API Docs
{% endembed %}

{% embed url="https://drudesk.com/blog/drupal-8-multilingual-modules" %}
Drupal Multi-language overview
{% endembed %}

{% embed url="https://camp.drupal.cornell.edu/sessions/automate-your-content-translation-google-translate-api" %}
Example Multi-lingual implementation
{% endembed %}
