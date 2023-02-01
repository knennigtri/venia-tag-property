# Data Elements <!-- omit in toc -->
The data elements defined below are all apart of the data layer and most are from the Adobe Client Data Layer. Each element defined below is labeled by project that exposes the data for that element.
- [Helper Elements](#helper-elements)
  - [Helper Code Snippet](#helper-code-snippet)
- [Browser Elements](#browser-elements)
  - [Browser Code Snippets](#browser-code-snippets)
- [WCM Core Component Elements](#wcm-core-component-elements)
  - [WCM Component Code Snippets](#wcm-component-code-snippets)
- [CIF Core Component Elements](#cif-core-component-elements)
  - [CIF Component Code Snippets](#cif-component-code-snippets)
- [Magento Events SDK Elements](#magento-events-sdk-elements)
- [XDM Schema Elements](#xdm-schema-elements)

## Helper Elements
These are helper elements to other data elements
| Name           | Extension | Data Element Type   | Value                                   |
| -------------- | --------- | ------------------- | --------------------------------------- |
| ECID           | ECID      | Javascript Variable | navigator.language                      |
| getCoreCmpJson | Core      | Custom Code         | See [Helper Code](#helper-code-snippet) |
| return1        | Core      | Custom Code         | `return 1;`                             |

### Helper Code Snippet
**Data Element:** getCoreCmpJson

``` javascript
/*
* This method can get core components from the ACDL via eventInfo.path or by first occurance
* Example 1. With an event that contains event.message.eventInfo.path value (core cmp event)
*   var cmpProperties = getCoreCmpJson(event);
* Example 2. It can be used to get the page component properties even if the event was not triggered by the page
*   var pageCmpProperties = getCoreCmpJson(event, "page");
* Example 3. It can be used to get the first cmp instance of a specified component on the page
*   var productCmpProperty = getCoreCmpJson(event, "component", "product");
* 
* returns a JSON object that contains:
*    this.path - the unique component path in the ACDL
*    this.component - the component json
*/
return function(event, cmpType, cmpName){
    if(event && event.message && event.message.hasOwnProperty("eventInfo")){
        var coreCmpPath="";
        //If the eventInfo.path is not given, then find the first occurance of the cmp in the ACDL
        if(!event.message.eventInfo.hasOwnProperty("path") && cmpType){
            var jsonObj;
            var cmpPathStr;
            //Get the page json if cmpType equals the page cmp
            if(cmpType.toLowerCase().includes("page")){
                jsonObj= JSON.parse(JSON.stringify(event.message.eventInfo.page));
                cmpPathStr = "page.";
                cmpName = "page"
            } else if(cmpType.toLowerCase().includes("component")){
                //Get the component json for all other components
                jsonObj= JSON.parse(JSON.stringify(event.message.eventInfo.component));
                cmpPathStr = "component.";
            }
            //Find the first occurance in the json (page json only has 1)
            for(var key in jsonObj){
                if(JSON.stringify(key).includes(cmpName)){
                val = JSON.stringify(key).replaceAll('\"','');
                coreCmpPath = cmpPathStr + val;
                //console.debug("Using cmp in ACDL: " + coreCmpPath);
                }
            }
        } else {
            //Get component path that triggered the event. This is standard with AEM Core Components
            coreCmpPath = event.message.eventInfo.path;
        }
        var cmpEvent = {
            //include the unique component path in the ACDL
            path: coreCmpPath,
            //get the state of the component
            component: window.adobeDataLayer.getState(coreCmpPath)
         };
        //return the json object of the component
        return cmpEvent;
    }
}
```

## Browser Elements
| Name                | Extension | Data Element Type   | Value                                      |
| ------------------- | --------- | ------------------- | ------------------------------------------ |
| browser.language    | Core      | Javascript Variable | navigator.language                         |
| browser.userAgent   | Core      | Javascript Variable | navigator.userAgent                        |
| page.name           | Core      | Page Info           | Title                                      |
| page.referrerUrl    | Core      | Page Info           | Referrer                                   |
| page.Url            | Core      | Page info           | Url                                        |
| page.siteSection    | Core      | Custom Code         | See [Browser code](#browser-code-snippets) |
| page.siteSubSection | Core      | Custom Code         | See [Browser code](#browser-code-snippets) |


### Browser Code Snippets
**Data Element:** `page.siteSection`

**Mapped from:** URL manipulation based Venia site structure
``` javascript
/* /content/venia/us/en/products/product-page.html/venia-bottoms/venia-pants/honora-wide-leg-pants.html
   /content/venia/us/en/products/category-page.html/venia-bottoms/venia-pants.html
   The siteSection in the above examples is venia-bottoms
*/
/* /content/venia/us/en.html
   The siteSection above is en
*/
var pathNames = window.location.pathname.split(".html").filter(element => element);
var paths;
if(pathNames.length > 1){
  paths = pathNames[1].split("/").filter(element => element);
  return paths[0];
} else {
  //return the last item in the path before the first .html
  //Ex: /path/to/my/awesomePage.html returns awesomePage
  paths = pathNames[0].split("/").filter(element => element);
  return paths[paths.length-1];
}
```
**Data Element:** `page.siteSubSection`

**Mapped from:** URL manipulation based Venia site structure
``` javascript
/* /content/venia/us/en/products/product-page.html/venia-bottoms/venia-pants/honora-wide-leg-pants.html
   /content/venia/us/en/products/category-page.html/venia-bottoms/venia-pants.html
   The siteSubSection in the above examples is venia-pants
*/
/* /content/venia/us/en/products/product-page.html/venia-bottoms.html
   /content/venia/us/en.html
   The siteSubSection above is ""
*/
var pathNames = window.location.pathname.split(".html").filter(element => element);
var sections;
if(pathNames.length > 1){
  sections = pathNames[1].split("/").filter(element => element);
  if((pathNames[0].includes("category-page") && sections.length > 1) ||
     (pathNames[0].includes("product-page") && sections.length > 2)){
    return sections[1];
  }
}
// if a subsection is not found return ""
return "";
```

## WCM Core Component Elements
https://github.com/adobe/aem-core-wcm-components 
| Name                     | Extension | Data Element Type                           | Value                     |
| ------------------------ | --------- | ------------------------------------------- | ------------------------- |
| coreCmp.name             | Core      | [Custom Code](#wcm-component-code-snippets) | property = dc:title       |
| coreCmp.description      | Core      | [Custom Code](#wcm-component-code-snippets) | property = dc:description |
| coreCmp.parentId         | Core      | [Custom Code](#wcm-component-code-snippets) | property = parentId       |
| coreCmp.type             | Core      | [Custom Code](#wcm-component-code-snippets) | property = @type          |
| coreCmp.xdm:linkURL      | Core      | [Custom Code](#wcm-component-code-snippets) | property = xdm:linkURL    |
| coreCmp.xdm:text         | Core      | [Custom Code](#wcm-component-code-snippets) | property = xdm:text       |
| pageCoreCmp.type         | Core      | [Custom Code](#wcm-component-code-snippets) | property = @type          |
| pageCoreCmp.path         | Core      | [Custom Code](#wcm-component-code-snippets) | property = repo:path      |
| pageCoreCmp.template     | Core      | [Custom Code](#wcm-component-code-snippets) | property = xdm:template   |
| pageCoreCmp.xdm:language | Core      | [Custom Code](#wcm-component-code-snippets) | property = xdm:language   |
| pageCoreCmp.xdm:tags     | Core      | [Custom Code](#wcm-component-code-snippets) | property = xdm:tags       |

### WCM Component Code Snippets
**Data Elements:** `coreCmp.*`

**Mapped from:** ACDL events `cmp:show`, `cmp:click`, `cmp:hide`
``` javascript
/* Returns a string of: */
var property= 'dc:title';
/* In the component schema:
id: {                   // component ID
    @type               // resource type
    repo:modifyDate     // last modified date
    dc:title            // title
    dc:description      // description
    xdm:text            // text
    xdm:linkURL         // link URL
    parentId            // parent component ID
}
*/
var getCoreCmpJSON = _satellite.getVar("getCoreCmpJSON");
var coreCmpJSON = getCoreCmpJSON(event); //Get the triggered component in the Adobe Client Data Layer

if(coreCmpJSON.component && coreCmpJSON.component.hasOwnProperty(property)) {
  return coreCmpJSON.component[property];
}
```
**Data Elements:** `pageCoreCmps.*`

**Mapped from:** ACDL fullState object
``` javascript
/* Returns a string of: */
var property= 'repo:path';
/* In the page schema:
id: {                   // component ID
    @type               // resource type
    repo:modifyDate     // last modified date
    dc:title            // title
    dc:description      // description
    xdm:text            // text
    xdm:linkURL         // link URL
    parentId            // parent component ID
    xdm:tags            // page tags
    repo:path           // page path
    xdm:template        // page template
    xdm:language        // page language
}
*/
var getCoreCmpJSON = _satellite.getVar("getCoreCmpJSON");
var coreCmpJSON = getCoreCmpJSON(event, "page"); //Get the page component in the Adobe Client Data Layer

if(coreCmpJSON.component && coreCmpJSON.component.hasOwnProperty(property)) {
  return coreCmpJSON.component[property];
}
```

## CIF Core Component Elements
https://github.com/adobe/aem-core-cif-components 
| Name                            | Extension | Data Element Type                           | Value                       |
| ------------------------------- | --------- | ------------------------------------------- | --------------------------- |
| cifAddEvent.id                  | Core      | [Custom Code](#cif-component-code-snippets) | property = @id              |
| cifAddEvent.xdm:quantity        | Core      | [Custom Code](#cif-component-code-snippets) | property = xdm:quantity     |
| cifAddEvent.xdm:SKU             | Core      | [Custom Code](#cif-component-code-snippets) | property = xdm:SKU          |
| productCoreCmp.xdm:currencyCode | Core      | [Custom Code](#cif-component-code-snippets) | property = xdm:currencyCode |
| productCoreCmp.xdm:listPrice    | Core      | [Custom Code](#cif-component-code-snippets) | property = xdm:listPrice    |
| productCoreCmp.xdm:SKU          | Core      | [Custom Code](#cif-component-code-snippets) | property = xdm:SKU          |
| productCoreCmp.xdm:categories   | Core      | [Custom Code](#cif-component-code-snippets) | property = xdm:categories   |
| productCoreCmp.xdm:assets       | Core      | [Custom Code](#cif-component-code-snippets) | property = xdm:assets       |



### CIF Component Code Snippets
**Data Elements:** cifAddEvent.*

**Mapped from:** ACDL events `cif:addToCart` and `cif:addToWishList`
``` javascript
var property = "@id"
if(event && event.message && event.message.eventInfo && event.message.eventInfo.hasOwnProperty(property)){
    return event.message.eventInfo[property];
}
```

**Data Elements:** `productCoreCmp.*`

**Mapped from:** ACDL fullState object
``` javascript
/* Returns a string of: */®
var property= 'xdm:currencyCode';
/* In the product schema:
id: {                   // component ID
    @type               // resource type
    repo:modifyDate     // last modified date
    dc:title            // title
    dc:description      // description
    parentId            // parent component ID
    xdm:SKU             // product SKU
    xdm:assets: [       // product assets array
      0: {              // assets schema array item
        repo:id             // asset UUID
        repo:path           // asset path
        @type               // asset resource type
      } ]
    xdm:categories: [   // product categories array
      0: {              // category schema array item
        repo:id           // category ID
        xdm:name          //category name
        xdm:asset: {      // asset schema associated with category
          repo:id             // asset UUID
          repo:path           // asset path
          @type               // asset resource type
        } } ]
    xdm:currencyCode    // product currency code
    xdm:listPrice       // product price
}
*/
var getCoreCmpJSON = _satellite.getVar("getCoreCmpJSON");
var coreCmpJSON = getCoreCmpJSON(event, "component", "product"); //Get the first product component in the Adobe Client Data Layer

if(coreCmpJSON.component && coreCmpJSON.component.hasOwnProperty(property)) {
  return coreCmpJSON.component[property];
}
```

## Magento Events SDK Elements
https://github.com/adobe/magento-storefront-events-sdk
| Name                           | Extension               | Data Element Type         | Value                                   |
| ------------------------------ | ----------------------- | ------------------------- | --------------------------------------- |
| categoryContext.name           | Adobe Client Data Layer | Data Layer Computed State | categoryContext.name                    |
| categoryContext.urlKey         | Adobe Client Data Layer | Data Layer Computed State | categoryContext.urlKey                  |
| categoryContext.urlPath        | Adobe Client Data Layer | Data Layer Computed State | categoryContext.urlPath                 |
| productContext.name            | Adobe Client Data Layer | Data Layer Computed State | productContext.name                     |
| productContext.productId       | Adobe Client Data Layer | Data Layer Computed State | productContext.productId                |
| productContext.sku             | Adobe Client Data Layer | Data Layer Computed State | productContext.sku                      |
| shopperContext.shopperId       | Adobe Client Data Layer | Data Layer Computed State | shopperContext.shopperId                |
| accountContext.emailAddress    | Adobe Client Data Layer | Data Layer Computed State | accountContext.emailAddress             |
| accountContext.firstName       | Adobe Client Data Layer | Data Layer Computed State | accountContext.firstName                |
| accountContext.lastName        | Adobe Client Data Layer | Data Layer Computed State | accountContext.lastName                 |
| accountContext.phoneNumber     | Adobe Client Data Layer | Data Layer Computed State | accountContext.phoneNumber              |
| searchInputContext.*           | Adobe Client Data Layer | Data Layer Computed State | searchInputContext.units[0].*           |
| searchInputContext.phrase      | Adobe Client Data Layer | Data Layer Computed State | searchInputContext.units[0].phrase      |
| searchResponseContext.*        | Adobe Client Data Layer | Data Layer Computed State | searchResponseContext.units[0].*        |
| searchResponseContext.products | Adobe Client Data Layer | Data Layer Computed State | searchResponseContext.units[0].products |

The products returned from Search are in the form of an array:
```
searchResponseContext.units[0].products[
    {
        "url": "",
        "rank": 0,
        "price": 0,
        "sku": "",
        "imageUrl": "",
        "name": ""
    },
    {...},
    {...},
    {...}
]
```

## XDM Schema Elements
These data elements are example mappings for the AEP Web SDK.
| Name                     | Extension                         | Data Element Type | Value                      |
| ------------------------ | --------------------------------- | ----------------- | -------------------------- |
| xdm-pageViews            | Adobe Experience Platform Web SDK | XDM object        | See [xdm-schema-mapping]() |
| xdm-productViews         | Adobe Experience Platform Web SDK | XDM object        | See [xdm-schema-mapping]() |
| xdm-onClick              | Adobe Experience Platform Web SDK | XDM object        | See [xdm-schema-mapping]() |
| xdm-addToCart            | Adobe Experience Platform Web SDK | XDM object        | See [xdm-schema-mapping]() |
| xdm-addToWishList        | Adobe Experience Platform Web SDK | XDM object        | See [xdm-schema-mapping]() |
| xdm-purchaseConfirmation | Adobe Experience Platform Web SDK | XDM object        | See [xdm-schema-mapping]() |
