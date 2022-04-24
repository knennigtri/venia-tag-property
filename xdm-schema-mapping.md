# Example XDM Schema mappings <!-- omit in toc -->
These mappings below are examples for different events triggered for Venia and what should be sent to AEP
- [Page View event](#page-view-event)
  - [XDM Mapping](#xdm-mapping)
- [On Click event](#on-click-event)
  - [XDM Mapping](#xdm-mapping-1)
- [Product Page View event](#product-page-view-event)
  - [XDM Mapping](#xdm-mapping-2)
- [Add To Cart event](#add-to-cart-event)
  - [XDM Mapping](#xdm-mapping-3)
- [Add To Wish List event](#add-to-wish-list-event)
  - [XDM Mapping](#xdm-mapping-4)
- [Purchase Confirmation event](#purchase-confirmation-event)

## Page View event
**Data Element:** `xdm-pageViews`

Schema: AEP Demo - Website Interactions Schema
> Note: This schema is based on [Demo Systems Next](https://docs.adobedemo.com/) schemas.

### XDM Mapping
 * <--tenantID-->
   * identification
     * ecid = %ECID%
 * environment
   * browserDetails
     * acceptLanguage = %browser.language%
     * userAgent = %browser.userAgent%
 * web
   * webPageDetails
     * name = %page.name%
     * siteSection = %page.siteSection%
     * pageViews
       * value = %return1%

## On Click event
**Data Element:** `xdm-onClick`

Schema: AEP Demo - Website Interactions Schema
> Note: This schema is based on [Demo Systems Next](https://docs.adobedemo.com/) schemas.

### XDM Mapping
 * <--tenantID-->
   * identification
     * ecid = %ECID%
 * environment
   * browserDetails
     * acceptLanguage = %browser.language%
     * userAgent = %browser.userAgent%
 * web
   * webinteraction
     * linkClicks
       * value = %return1%
     * name = %page.name%
     * type = other
   * webPageDetails
     * name = %page.name%
     * siteSection = %page.siteSection%

## Product Page View event
**Data Element:** `xdm-productPageViews`

Schema: AEP Demo - Website Interactions Schema
> Note: This schema is based on [Demo Systems Next](https://docs.adobedemo.com/) schemas.

### XDM Mapping
 * <--tenantID-->
   * identification
     * ecid = %ECID%
   * productData
     * productName = %productContext.name%
     * productPageUrl = %page.Url%
     * productPrice = %productCoreCmp.xdm:listPrice%
 * commerce
   * productViews
     * value = %return1%
 * environment
   * browserDetails
     * acceptLanguage = %browser.language%
     * userAgent = %browser.userAgent%
 * web
   * webPageDetails
     * name = %page.name%
     * siteSection = %page.siteSection%
     * pageViews
       * value = %return1%

## Add To Cart event
**Data Element:** `xdm-addToCart`

Schema: AEP Demo - Website Interactions Schema
> Note: This schema is based on [Demo Systems Next](https://docs.adobedemo.com/) schemas.

### XDM Mapping
 * <--tenantID-->
   * identification
     * ecid = %ECID%
   * productData
     * productName = %productContext.name%
     * productPageUrl = %page.Url%
     * productPrice = %productCoreCmp.xdm:listPrice%
 * commerce
   * productListAdds
     * value = %return1%
 * environment
   * browserDetails
     * acceptLanguage = %browser.language%
     * userAgent = %browser.userAgent%
 * web
   * webinteraction
     * linkClicks
       * value = %return1%
     * name = %page.name%
     * type = other
   * webPageDetails
     * name = %page.name%
     * siteSection = %page.siteSection%

## Add To Wish List event
**Data Element:** `xdm-addToWishList`

Schema: AEP Demo - Website Interactions Schema
> Note: This schema is based on [Demo Systems Next](https://docs.adobedemo.com/) schemas.

### XDM Mapping
 * <--tenantID-->
   * identification
     * ecid = %ECID%
   * productData
     * productName = %productContext.name%
     * productPageUrl = %page.Url%
     * productPrice = %productCoreCmp.xdm:listPrice%
 * commerce
   * saveForLaters
     * value = %return1%
 * environment
   * browserDetails
     * acceptLanguage = %browser.language%
     * userAgent = %browser.userAgent%
 * web
   * webinteraction
     * linkClicks
       * value = %return1%
     * name = %page.name%
     * type = other
   * webPageDetails
     * name = %page.name%
     * siteSection = %page.siteSection%

## Purchase Confirmation event
TBD