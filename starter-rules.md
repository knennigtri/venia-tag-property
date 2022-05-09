# Starter Rules
Thhe starter rules below listen for specific events in the ACDL and then fire. Once the rule fires, the Rule Actions can be defined for your specific implementation of AEP, Target, Analytics or other 3rd party endpoint configured in AEP Tags.

| Rule Name          | <!--TODO--> Event Ext | Specific Event     | Condition                         | Exclusive Data Elements                    |
| ------------------ | ---------------- | ------------------ | --------------------------------- | ------------------------------------------ |
| Page View          | Core (Page Load) |                    |                                   |                                            |
| Product Page View  | ACDL ()          | product-page-view  |                                   | productPageCmp.                            |
| On Item Click      | ACDL ()          | cmp:click          |                                   | coreCmp.                                   |
| Add To Cart        | ACDL ()          | cif:addToCart      |                                   | cifAddEvent.                               |
| Add to Wish List   | ACDL ()          | cif:addToWishList  |                                   | cifAddEvent.                               |
| Checkout Viewed    | Core (Page Load) |                    | URL contains 'checkout.html'      |                                            |
| Category Page View | Core (Page Load) |                    | URL contains 'category-page.html' | categoryContext.                           |
| Search             | Core (Page Load) |                    | URL contains 'search.html'        | searchInputContext., searchResponseContext |
| Login*             | ACDL ()          | cif:userSignIn     |                                   |                                            |
| Logout*            | ACDL ()          | cif:userSignOut    |                                   |                                            |
| Purchase Action*   | ACDL ()          | cif:placeOrder     |                                   |                                            |
| Cart Removal*      | ACDL ()          | cif:removeFromCart |                                   |                                            |


> \* These rules are found in the open source projects, but as of this writing are not triggering in the Venia project. These will hopefully trigger in the future releases of Venia and similar projects.