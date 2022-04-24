# AEM Tag Property demo
How did this project begin? Check out [the story](the-story.md).

This is an example property difinition on how to setup a Adobe Expierence Platform Tag (previously Adobe Launch) for an AEM website using WCM core components, CIF core components, and Magento events SDK. This tutorial is valid for the following reference websites:
 * [Venia](https://github.com/adobe/aem-cif-guides-venia) with WCM and CIF core components
 * [WKND](https://github.com/adobe/aem-guides-wknd) with WCM core components

A few general points for this Tag property project:
 * When building a launch property, Data Elements should contain any logic to pull values out of the websites's Data Layer. This allows Rule Actions to be freely built without deep knowledge of the data layer.
 * Rules can be quickly stubbed out by using data layer events from the ACDL. This allows product experts to quickly build Actions based on these defined events.
 * Venia uses several projects to build it's data layer:
   * WCM core components: https://github.com/adobe/aem-core-wcm-components 
   * CIF core components: https://github.com/adobe/aem-core-cif-components
   * Magento Events SDK: https://github.com/adobe/magento-storefront-events-sdk 
 * WKND uses a single project to build it's data layer:
   * WCM core components: https://github.com/adobe/aem-core-wcm-components 
 * //TODO Extensions were sparenly used to make this implementation as simple as possible. Each data element defined specifies which extension was used. In total, these extensions were used:
   * Core
   * ECID
   * Adobe Client Data Layer
   * Adobe Analytics
   * Adobe Target v2
   * AEP Web SDK
   * Analytics product string

In this documentation:
 * [Data Elements](data-elements.md) exposed from the ACDL
 * [Starter Rules](starter-rules.md)
 * [Example XDM mappings](xdm-schema-mapping.md)
 * [Example Analytics Variables](analytics-variables.md)