# AEM Tag Property demo
This is an example on how to setup a Adobe Expierence Platform Tag (previously Adobe Launch) for an AEM website using WCM core components, CIF core components, and Magento events SDK. This tutorial is valid for the following reference websites:
 * [Venia](https://github.com/adobe/aem-cif-guides-venia) with WCM and CIF core components
 * [WKND](https://github.com/adobe/aem-guides-wknd) with WCM core components

A few key points to understand: 
 * When building a launch property, Data Elements should contain any logic to pull values out of the websites's Data Layer. This allows rules to be freely built without deep knowledge of the data layer.
 * Venia uses several projects to build it's data layer:
   * WCM core components: https://github.com/adobe/aem-core-wcm-components 
   * CIF core components: https://github.com/adobe/aem-core-cif-components
   * Magento Events SDK: https://github.com/adobe/magento-storefront-events-sdk 

> Note: WKND only uses the WCM core components and only those data elements should be used in a tag property.

1. write out events triggered
2. Document all data elements
   1. name
   2. extension
   3. logic