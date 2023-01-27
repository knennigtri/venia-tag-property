# AEM Tag Property demo
//TODO add link
Check out the [medium article]() for this proof of concept

This is an example property definition on how to setup a Adobe Expierence Platform Tag (previously Adobe Launch) for an AEM website using WCM core components, CIF core components, and Magento events SDK. This tutorial is valid for the following reference websites:
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
 * Web Property Extensions used:
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

## Installing the Venia web property

1. Install npm if not already installed
2. install the AEP tag tool
```
 npm install -g @knennigtri/aep-tag-tool
```
3. Create and Adobe IO project with Launch API added
4. Download [example.config.yml](https://github.com/knennigtri/aep-tag-tool/blob/main/example.config.yml) and add your credentials from the AOI project
5. Create a new AEP Tag property. 
   1. Take note of the PID from the URL: P12341234123123123123
   2. Install these plugins:
      1. Experience Cloud ID
      2. Adobe Client Data Layer
      3. Adobe Experience Platform Web SDK
      4. Adobe Target v2
      5. Adobe Analytics
      6. Adobe Analytics Product String
6. Download [venia-property.json](venia-property.json)
7. Import the venia data elements and starter rules into your property:
```
 aep-tag-tool -c yourConfig.yml --import venia-property.json -p <yourPID> -DR
```

