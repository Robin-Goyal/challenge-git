# FAVR.TOWN

Specification
06/28/2021

Project repository:
[ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/favr.town.frontend.ssr](ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/favr.town.frontend.ssr)

Status:
**FINAL**



Recommended editor/viewer for markdown: [https://typora.io/](https://typora.io/)



## Summary

This project creates a simple unauthenticated website with the following user stories:

1. view favr.town homepage
2. view favr.town promotional content on [/about](/about)
3. view an item, e.g. merchant, news, or survey, that was shared by another user via QR code or direct link, referenced through query parameters in the url
4. create a user account for [https://app.favr.town](https://app.favr.town) ("join") either as merchant or patron
5. view terms of service and privacy policy
6. sign into an existing account (redirects to [https://app.favr.town/signin](https://app.favr.town/signin) - target is not yet available)



The site will provide the following flow:

![](https://img.favr.town/spec/flow.svg)



### Stack

This project will use preact for its smaller size, speed, and simplicity, and JavaScript (not TypeScript).

There's no external API integration or authentication, and all data is handled directly within lambda which integrates with the AWS backend services.



#### Navigation and Routing

**Routing** is critical. 

Do not use "hash" routes. 

The project must implement a simple but solid router that allows for:

- each view can be reached directly (if necessary with a redirect to [/signin](/signin) and back) 
- views can parse and use url and query parameters to update their state when reached
- views can survive a page reload
- standard browser navigation back/forward can be used, in addition to a "back" button/icon



#### Open Source

The project must use only permissively licensed third party components, ideally MIT.
The build script must create a license report that will be referenced from within the site.



#### Environments

The project must support 2 environments, "production" and "development". The project includes a `config.json` file to specify settings for each environment, and the `package.json` must support separate build scripts, e.g. `build` and `build:prod`.



#### Utility function

The project uses a standardized "utility" function. This function provides a mock backend for the project and will later be replaced.



#### Error handling

There are *expected* errors, where the backend returns an "error" without breaking the app, like "not found" or "expired", if the user tries to access something that is not/no longer available. Those should show as banner or modal without the need to re-render.

There can also be *unexpected* errors, caused by, e.g. bugs, backend issues, frontend issues, or network/infrastructure issues. Those must be handled gracefully with meaningful UI, and must not break the app.



If an error occurs (any error) show the error message (or, if no message is available, show "something went wrong") in a banner or modal center screen that can be closed by tapping it.

Show form validation errors directly underneath the form field that failed validation.



Errors must never break the app and must be handled gracefully, either through a custom class component [https://reactjs.org/docs/error-boundaries.html](https://reactjs.org/docs/error-boundaries.html), or a library implementation, e.g. [https://www.npmjs.com/package/react-error-boundary](https://www.npmjs.com/package/react-error-boundary).



#### Date/time

All dates are managed as epoch seconds (number) and in the client displayed based on local browser time zone settings.



### Style

The website must be mobile first, and render as well on tablet and desktop. The default view mode is portrait. On tablet and desktop the site content should be rendered in a container centered in the browser.

The website should use [material UI](https://github.com/material-components/material-components-web) and Google's [Baloo Chettan 2](https://fonts.google.com/specimen/Baloo+Chettan+2) font family (included as direct dependency, not via CDN).



#### Size and Measurements

Measurement and size of objects specified here are based on an example screen (Google Pixel 3 XL) (1440 x 2960 pixels) and must be scaled to fit the target (mobile) screen.

On tablet/desktop, show the mobile screen in a centered container.
Do no rearrange views based on screen size.



#### Forms and validation

A library may be used for form and validation, e.g. [https://github.com/react-hook-form/react-hook-form](https://github.com/react-hook-form/react-hook-form). There is also a form builder tool: [https://react-hook-form.com/form-builder](https://react-hook-form.com/form-builder).

Invalid input should change the cell's frame color to red, and show a red validation error underneath the cell.

![](https://img.favr.town/spec/invalid_input_example.jpg)



#### Colors

White background, with light/dark-grey to black fonts and icons. Background artwork will be often line-art, primarily grey-scale with some color accents.



#### View Transitions

Views should transition so that when going forward, the new view slides in from the right, and when going back, the current view slides away to the right. 

Simple example: [https://codesandbox.io/s/react-router-animation-working-06mrd](https://codesandbox.io/s/react-router-animation-working-06mrd)

Each view will have a line-art header asset, that is also used reversed and compressed in the footer. The asset will vary for each screen based on the continuous line, and the slide animation would create sense of continuity.



#### Client-side animations on [/](/)

The main view [/](/) should use the same client-side animation (sections fly in from below) as [https://thelovemaze.com](https://thelovemaze.com) (code example can be provided).

Reference: [https://github.com/michalsnik/aos](https://github.com/michalsnik/aos)



### Structure

The app structure must follow industry best practices, and should use clear and concise multi-layer architecture.



### Build

All fonts and image assets must be included with the build to minimize traffic. 

The build must be optimized for size and performance, with minimal dependencies, webpack is preferred.

The site must comply with Google's SEO guidelines (while each view includes a static JSONLD object, additional JSONLD may be returned with a function's result object.

The build must allow to include the following dependencies, and execute as AWS lambda function with the Node.js 14.x runtime:

```
"node-fetch": "2.6.1"
"omit-empty": "1.0.0",
"shortid": "2.2.16"
```

The build must create a JSON license report, e.g. using [npm-license-crawler](https://github.com/mwittig/npm-license-crawler),  that will be referenced from within the site.



## Delivery

This specification delivers a working and tested state-of-the-art web application, as described below, to serve the dynamic responsive site [https://favr.town](https://favr.town), using preactjs (ReactJS) (https://preactjs.com/guide/v10/server-side-rendering/), including all project source and build configuration files, 3rd party libraries, assets, and applicable documentation, optimized for current browsers (Chrome, Firefox, Safari), mobile first, with support for current mobile (Android and iOS), tablet, and desktop viewing modes, and following industry best practices.




## Sprints

### A


This sprint delivers a simple unauthenticated web application with 5 routes as stand-alone react app (using preact) without any server logic or framework, integrating the included utility function for dynamic data, that can be build with webpack for either environment using npm or yarn. 

The utility function will later be replaced with actual data connections - this is out of scope for this sprint.

The site will allow users to view public content, accept terms and conditions, and signup for a user account as either of 2 roles: "patron" or "merchant". The successful signup redirects the user to a separate authenticated site [https://app.favr.town](https://app.favr.town).



#### Tasks

##### Create App Structure

Create core application (non-PWA, no tests, minimal dependencies) from scratch with latest dependencies, layered architecture, prepare `constants` and `services` folders.



##### Create routes and views

Create all views as described in the "Routes in this sprint" section.



##### Optimize CSS

Make it pretty. The app should only render as mobile, and on tablet/desktop/phone horizontal, it should render inside centered container. The client must not allow zoom in/out.



#### Routes in this sprint

All routes in the application are unauthenticated. 
All paths can be reached directly.

*Any query parameters must be preserved throughout the site.*

To simplify the backend, the lambda will respond to all requests, and therefore must support additional "system paths", e.g. for icons and assets, either with content or redirect.

*allow for future extension to additional conditions*




***JSON-LD***

Include the following static jsonLd object with each view, plus any jsonLd returned with `utilityGet({ [m/n/s]: {value} })`:

```json
"jsonLd": {
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Favr Inc",
  "legalName": "Favr Inc",
  "url": "https://favr.town",
  "logo": "https://img.favr.town/app/logo.png",
  "contactPoint": {
    "@type": "ContactPoint",
    "contactType": "customer support",
    "email": "hello@favr.town"
  }
}
```

It's OK to use [https://github.com/nfl/react-helmet](https://github.com/nfl/react-helmet) or similar if needed.



##### [/](/)

The view has several sections aligned underneath each other. If the view does not fit the screen it allows scrolling. While scrolling, header and footer sections remain visible.



***header***

*(see section "standard header and footer")*



***content***

Aligned below the header, the content has several sections. 

The content scrolls over a static background image for the view. In addition, sections can bring their own background image (to be layered on top of the static background).

![](https://img.favr.town/spec/unauth_any-root.svg)



Elements:

| id   | element      | type  | source/asset | details |
| ---- | ------------ | ----- | ------------ | ----- |
| 1    | background | image |  | the background remains static for the view and does not scroll with the content |
| 2    | dynamic content | based on queryParams, if exist, or hide this section | (assets defined in dynamic content object) | (see merchant, news, survey below) |
| 3    | about section 1 | static text | `"What goes around, comes around./nA little FAVR goes a long way./nAppreciate, reciprocate, value, support../nWe could all use some, give some, and enjoy a happier town."` | |
| 4   | about section 2 | image | https://img.favr.town/app/root.png | |
| 5    | about section 3 | static text | `"FAVR.TOWN is a platform for patrons to engage with their favorite merchants and for merchants to reward their patron’s FAVR, such as likes, votes, suggestions, shares, purchases, etc.\nMerchants design their reward programs to return a patron’s FAVR."` | |

If query parameters exists,call the `utilityGet({ [m/n/s]: {value} })` function with the parameters, and render the result object as read-only form on top of the image.


*include existing query parameters*

Only call `utilityGet({ [m/n/s]: {value} })` if a parameter is present. If `utilityGet({ [m/n/s]: {value} })` does not return a success result object, tolerate, and simply hide the section.

If `utilityGet({ [m/n/s]: {value} })` returns `{ item, jsonLd }`, render section 2 as read-only form based on the item object, and include any jsonLd attributes.



###### merchant - query parameter m

example urls: 

- [https://favr.town?m=m_g56aDAS](https://favr.town?m=m_g56aDAS)
- [https://favr.town?m=m_g56aDAS&r=p_abcdefg](https://favr.town?m=m_g56aDAS&r=p_abcdefg)

*In the second url example, "r" query parameter stands for "reference" - the user who shared this item.*

Example result for  `utilityGet({ "m": "m_g56aDAS" })`:

```json
{
  "item": {
    "about": "Little Cupcake Shop is a Gourmet Bakery in New York City. Our products include specialty cupcakes including spicy pumpkin, green tea, and chocolate cupcakes. We have been been around since 1998.",
    "businessName": "Little Cupcake Shop",
    "website": "https://littlecupcakeshop.com",
    "email": "hello@littlecupcakeshop.com",
    "tags": ["##BAKERY", "#BREAD", "#CUPCAKE"],
    "logo": "https://img.favr.town/content/cupcake.png",
    "album": [
      "https://img.favr.town/content/cc-1.jpg",
      "https://img.favr.town/content/cc-2.jpg",
      "https://img.favr.town/content/cc-3.jpg"
    ]
  },
  "jsonLd": {
    "@context": "https://schema.org",
    "@type": "Organization",
    "name": "Little Cupcake Shop",
    "legalName": "Little Cupcake Shop",
    "url": "https://littlecupcakeshop.com",
    "logo": "https://img.favr.town/content/cupcake.png",
    "contactPoint": {
      "@type": "ContactPoint",
      "contactType": "customer support",
      "email": "hello@littlecupcakeshop.com"
    }
  }
}
```

*mockup*

![](https://img.favr.town/spec/unauth_any_m.svg)


*elements*

Don't show errors for missing attributes, just leave out the affected section, e.g. don't render the album at all if the array is not present or empty.

| id | element/source | type | format as |
| -------- | -------- | -------- | -------- |
| 1 | logo | string (url)    | image/png, zoom |
| 2 | businessName | string  |         |
| 3 | tags[] | array of string | list of string separated by ","<br /><br />if an item starts with "##", replace "##" with "#", and format differently from other items (emphasize).<br /><br />(lanes start with "##", specialties start with "#") |
| 4 | about | string    | honor line breaks |
| 5 | website | string | url, allow click to open in new tab. leave empty if not exist |
| 6 | album | image/png | zoom so that 2 images fit next to each other, allow vertical scrolling as needed, only show if exist |



###### deal - query parameter d

example urls: 

- [https://favr.town?d=d_NFPK88TcL](https://favr.town?d=d_NFPK88TcL)
- [https://favr.town?d=d_NFPK88TcL&r=p_abcdefg](https://favr.town?d=d_NFPK88TcL&r=p_abcdefg)

example result for  `utilityGet({ "d": "d_NFPK88TcL" })`:

```json
{
  "item": {
    "profile": {
      "about": "Little Cupcake Shop is a Gourmet Bakery in New York City. Our products include specialty cupcakes including spicy pumpkin, green tea, and chocolate cupcakes. We have been been around since 1998.",
      "businessName": "Little Cupcake Shop",
      "logo": "https://img.favr.town/content/cupcake.png",
      "tags": ["##BAKERY", "#BREAD", "#CUPCAKE"]
    },
    "image": "https://img.favr.town/content/cc-1.jpg",
    "text": "Buy 4 get 5 limited event!",
    "url": "https://littlecupcakeshop.com",
    "redeem": "available at our UES and Hell's Kitchen locations",
    "redeem_by": 1616878800
  },
  "jsonLd": {}
}
```

*mockup*

![](https://img.favr.town/spec/unauth_any_n-d.svg)

*elements*

Don't show errors for missing attributes, just leave out the affected section, e.g. don't render the image at all if the attribute is not present or empty.

| id | element | type | format as |
| -------- | -------- | -------- | -------- |
| 1 | logo | string (url) | image/png, zoom |
| 2 | businessName | string |emphasize businessName|
| 3 | tags[] | array of string | list of string separated by ","<br /><br />if an item starts with "##", replace "##" with "#", and format differently from other items (emphasize).<br /><br />(lanes start with "##", specialties start with "#") |
| 4 | icon | image/svg, included ||
| 5 | text | string ||
| 6 | image | string (url) |image/png, zoom|
| 7 | url | string |web, `onClick()` open in new tab|
| 8 | redeem_by | number |local short date/time|
| 9 | redeem | string ||



**allow for future extension to additional "type" values**


###### news - query parameter n

example urls: 

- [https://favr.town?n=n_VBQnEpBCO](https://favr.town?n=n_VBQnEpBCO)
- [https://favr.town?n=n_VBQnEpBCO&r=p_abcdefg](https://favr.town?n=n_VBQnEpBCO&r=p_abcdefg)


Example result for  `utilityGet({ "n": "n_VBQnEpBCO" })`:

```json
{
  "item": {
    "profile": {
      "about": "Little Cupcake Shop is a Gourmet Bakery in New York City. Our products include specialty cupcakes including spicy pumpkin, green tea, and chocolate cupcakes. We have been been around since 1998.",
      "businessName": "Little Cupcake Shop",
      "logo": "https://img.favr.town/content/cupcake.png",
      "tags": ["##BAKERY", "#BREAD", "#CUPCAKE"]
    },
    "image": "https://img.favr.town/content/cc-3.jpg",
    "text": "Post-pandemic reopening! Welcome back June 1st!",
    "url": "https://littlecupcakeshop.com"
  },
  "jsonLd": {}
}
```


*mockup*

![](https://img.favr.town/spec/unauth_any_n-n.svg)

*elements*

Don't show errors for missing attributes, just leave out the affected section, e.g. don't render the image at all if the attribute is not present or empty.

| id | element | type | format as |
| -------- | -------- | -------- | -------- |
| 1 | logo | string (url) | image/png, zoom |
| 2 | businessName | string | emphasize businessName |
| 3 | tags[] | array of string | list of string separated by ","<br /><br />if an item starts with "##", replace "##" with "#", and format differently from other items (emphasize).<br /><br />(lanes start with "##", specialties start with "#") |
| 4 | icon | image/svg, included ||
| 5 | text | string ||
| 6 | image | string (url) |image/png, zoom|
| 7 | url | string |web, `onClick()` open in new tab|



###### survey query parameter s

example urls: 

- [https://favr.town?s=s_abc](https://favr.town?s=s_abc)
- [https://favr.town?s=s_abc&r=p_abcdefg](https://favr.town?s=s_abc&r=p_abcdefg)

example result for  `utilityGet({ "s": "s_abc" })`:

```json
{
  "item": {
    "profile": {
      "about": "Little Cupcake Shop is a Gourmet Bakery in New York City. Our products include specialty cupcakes including spicy pumpkin, green tea, and chocolate cupcakes. We have been been around since 1998.",
      "businessName": "Little Cupcake Shop",
      "logo": "https://img.favr.town/content/cupcake.png",
      "tags": ["##BAKERY", "#BREAD", "#CUPCAKE"]
    },
    "question": "how do you order your food?",
  },
  "jsonLd": {}
}
```

*mockup*

![](https://img.favr.town/spec/unauth_any_s.svg)

*elements*

| id | element | type | format as |
| -------- | -------- | -------- | -------- |
| 1 | logo | string (url) | image/png, zoom |
| 2 | businessName | string              | emphasize businessName |
| 3 | tags[] | array of string | list of string separated by ","<br /><br />if an item starts with "##", replace "##" with "#", and format differently from other items (emphasize).<br /><br />(lanes start with "##", specialties start with "#") |
| 4 | icon | image/svg, included ||
| 5 | question | string, multi-line ||



***footer***







Default footer (see section "standard header and footer") plus the following elements:

| id   | element      | type  | source/asset | details |
| ---- | ------------ | ----- | ------------ | ----- |
| 1    | footer image | image/included | |  |
| 2    | join button | button | `root-join.png` | `onClick()`, route to [/join](/join) |
| 3    | signin button | button | `root-signin.png` | `onClick()` route to [https://app.favr.town/signin](https://app.favr.town/signin) (external), and *include existing query parameters*. |
| 4    | copyright | string | "(c) 2021, favr.town. terms, privacy, acknowledgements."<br /><br />year must be dynamic based on client browser local date, "terms" links to [/terms](/terms), "privacy" links to [/privacy](/privacy), and "acknowledgements" opens a modal and list direct dependencies with their licenses (see section "Build")<br /><br />align on the bottom above the footer |redirects/modal|

When "acknowledgements" is clicked, open a modal and list direct dependencies with their licenses. 


![](https://img.favr.town/spec/unauth_any-acknowledgements.svg)


When a dependency is clicked, open the repository url (if available) in a new tab. When a dependency's license is clicked, open the license url (if available) in a new tab.




##### [/join](/join)

This view allows the user to join favr.town as either a patron, or a merchant, and explains each role.



***header***

*(see section "standard header and footer")*




***content***

![](https://img.favr.town/spec/unauth_any-join.svg)



This section contains the following elements:

| id | element | source | event |
| -------- | -------- | -------- | -------- |
| 1 | join as patron, button| label, static text, image | `onClick()`, route to [/signup?role=patron](/signup?role=patron) | route to [/signup?role=patron](/signup?role=patron) |
| 2 | cup icon | (included) |  |  |
| 3 | text | static | `"Likes, shares, suggestions, purchases.. Your fave merchant values your input. Favr them and initiate a virtuous cycle."` |  |
| 4 | join as merchant, button | label, static text, image | `onClick()`, route to [/signup?role=merchant](/signup?role=merchant) |
| 5 | kettle icon | (included) |  |  |
| 6 | text | static | `"Reward your loyal patrons for their engagement. Empower them to be your brand ambassadors."` |  |

*include existing query parameters*



***footer***

*(see section "standard header and footer")*



##### [/terms](/terms)

This view allows the user to review and accept the site's "terms of service"  (same as [/privacy](/privacy).



***header***

*(see section "standard header and footer")*



***content***

To retrieve the terms and conditions (string, honor line breaks) call `utilityTerms()` without any parameters.

Example result for `utilityTerms()`:

```json
{
  "item": {
    "service": "Terms of Service\r\n\r\nLast revised on October 30, 2019\r\n\r\nWelcome to favr.town, a loyalty solution for local small businesses through its web applications (the \"Applications\"), located at favr.town (the \"Site\") and operated by Favr Inc, a Delaware corporation (\"Favr\" or \"The Company\" or \"we\" or \"us\" or \"our\").\r\nThese Terms of Service (\"Terms,\" \"Agreement\") govern your access or use of the applications, websites, content, products, functions, and services (the \"Service,\" or \"Services\") made available by Favr Inc, its affiliates, and third party providers.",
    "privacy": "Privacy Policy\r\n\r\nLast revised on October 30, 2019\r\n\r\nFavr Inc (\"Favr,\" \"we,\" \"our,\" or \"us\") respects the privacy of its users (\"you,\" \"your,\" or \"User\") and hereby discloses our privacy practices. We encourage you to read this Privacy Policy carefully when using our applications or services or transacting business with us. By using our websites, applications or other online services (our \"Service,\" or \"Services\"), you are accepting the practices described in this Privacy Policy."
  },
  "json": {}
}
```


![](https://img.favr.town/spec/terms.svg)


Elements

| id   | element | source | event |
| ---- | ------- | ------ | ----- |
| 1 | label | "TERMS OF SERVICE" | |
| 2 | terms of service text rendered honoring line breaks | item.service |

*include existing query parameters*



***footer***

*(see section "standard header and footer")*



##### [/privacy](/privacy)

This view allows the user to review and accept the site's "privacy policy" (same as [/terms](/terms).




***header***

*(see section "standard header and footer")*



***content***

To retrieve the terms and conditions (string, honor line breaks) call `utilityTerms()` without any parameters.

Example result for `utilityTerms()`:

```json
{
  "item": {
    "service": "Terms of Service\r\n\r\nLast revised on October 30, 2019\r\n\r\nWelcome to favr.town, a loyalty solution for local small businesses through its web applications (the \"Applications\"), located at favr.town (the \"Site\") and operated by Favr Inc, a Delaware corporation (\"Favr\" or \"The Company\" or \"we\" or \"us\" or \"our\").\r\nThese Terms of Service (\"Terms,\" \"Agreement\") govern your access or use of the applications, websites, content, products, functions, and services (the \"Service,\" or \"Services\") made available by Favr Inc, its affiliates, and third party providers.",
    "privacy": "Privacy Policy\r\n\r\nLast revised on October 30, 2019\r\n\r\nFavr Inc (\"Favr,\" \"we,\" \"our,\" or \"us\") respects the privacy of its users (\"you,\" \"your,\" or \"User\") and hereby discloses our privacy practices. We encourage you to read this Privacy Policy carefully when using our applications or services or transacting business with us. By using our websites, applications or other online services (our \"Service,\" or \"Services\"), you are accepting the practices described in this Privacy Policy."
  },
  "json": {}
}
```


![](https://img.favr.town/spec/privacy.svg)


Elements

| id   | element | source | event |
| ---- | ------- | ------ | ----- |
| 1 | label | "PRIVACY POLICY" | |
| 2 | privacy text rendered honoring line breaks | item.privacy |


*include existing query parameters*



***footer***

*(see section "standard header and footer")*



##### [/signup](/signup)

This view consists of a form with validation, and a "signup" button. 

**This view requires the "role" query parameter. If no "role" parameter is present, redirect to [/](/).**



***header***

*(see section "standard header and footer")*



***content***

This view shows a new user signup form. 

The form will validate the input. Additional validation happens through the `utilityValidate()` function, e.g. availability of a login or email, once the form validates and the user clicks "signup".

**Based on the selected role, "patron" or "merchant", the form only shows the "businessName" attribute for the "merchant" role. All other attributes are the same.**



![](https://img.favr.town/spec/signup.svg)



This section contains the following elements:

| id | element | type | source/asset | event |
| -------- | -------- | -------- | -------- | -------- |
| 1 | businessName | string |  | hide for "merchant" role, else validate min_length=4 |
| 2 | login | string, force lower-case | | validate min_length=4 |
| 3 | password | string | | validate min_length=6 |
| 4 | email | string | | validate as `^(?=.{1,64}@)[A-Za-z0-9_-]+(\\.[A-Za-z0-9_-]+)*@[^-][A-Za-z0-9-]+(\\.[A-Za-z0-9-]+)*(\\.[A-Za-z]{2,})$` (email) |
| 5 | accept terms | checkbox |  | validate checked |
| 6 | terms | link |  | route to [/terms](/terms) |
| 7 | privacy | link |  | route to [/privacy](/privacy) |

Once the form is complete and validates, enable the "signup" button in the footer. 



A library may be used for form and validation, e.g. [https://github.com/react-hook-form/react-hook-form](https://github.com/react-hook-form/react-hook-form). There is also a form builder tool: [https://react-hook-form.com/form-builder](https://react-hook-form.com/form-builder).



***footer***

Default footer (see section "standard header and footer") plus the following elements:

| id   | element      | type  | source/asset | details |
| ---- | ------------ | ----- | ------------ | ----- |
| 1    | footer image | image/included | |  |
| 2    | signup | button | disabled unless form validates | `onClick()` call `utilityValidate({ params: { role, login, email, businessName }}) (only include "businessName" for the "merchant" role) |

If the validation succeeds (all attributes are returned with value: true), call `utilitySignup()` and include all form attributes *and url query parameters* with the request.

`utilitySignup()` will internally perform another validation using `utilityValidate()`.

On success, this will return the AWS cognito account details. Route to [https://app.favr.town/signin?login=login](https://app.favr.town/signin?login=login) (external, same tab) and *include any other query parameters*.



##### assets (system path)

validate path: 
`event.rawPath.endsWith('.png') || event.rawPath.endsWith('.svg') || event.rawPath.endsWith('.jpg')`

return: 
```
{
  statusCode: '301',
  statusDescription: 'Redirecting to asset domain',
  headers: {
	'Access-Control-Allow-Origin': '*',
	'Content-Type': 'text/html',
	'Cache-Control': 'max-age=3600',
	'Referrer-Policy': 'no-referrer-when-downgrade',
	location: `https://img.favr.town/app${event.rawPath}`,
  },
}
```



##### index.html (system path)

validate path: 
`event.rawPath.startsWith('/index.html')`

return: 
```
{
  statusCode: '301',
  statusDescription: 'Redirecting to root',
  headers: {
	'Access-Control-Allow-Origin': '*',
	'Content-Type': 'text/html',
	'Cache-Control': 'max-age=3600',
	'Referrer-Policy': 'no-referrer-when-downgrade',
	location: `https://favr.town`,
  },
}
```



##### uptime (system path)

This path is relevant for automated monitoring and also keeps the lambda "warm"

validate path: 
`event.rawPath === '/uptime'`

return: 
```
{
  statusCode: 200,
  headers: {
	'Access-Control-Allow-Origin': '*',
	'Content-Type': 'application/json',
	'Cache-Control': cache_control_noCache,
	'x-lambda-memory': get_current_memory(),
  },
  body: JSON.stringify({}),
}
```



##### files (system path)

The function will need to return a few files, e.g. sitemap.xml, robots.txt, google/bing validation, etc. This is out of scope for this sprint and will be added later.





#### standard header and footer

This header is used for views that may not provide a `/a/init GET` result.



The background image for each header is a static asset associated with the view's url. 

The header is fixed, does not move on scroll.



*Assets are temporary and will be replaced with .svg artwork.*



Measurements:

| measurement | pixel | % screen width | horizontal align | vertical align |
| --- | --- | --- | --- | --- |
| header width | 1440 | 100 | center | top |
| header height | 331 | 23 | | |
|  |  |  | | |
| background image width | 1440 | 100 | center | bottom |
| background image height | 302 | 21 | | |



*Other elements fit relative to the background image.*



##### header for [/](/)


![](https://img.favr.town/spec/neutral-header.svg)


Elements:

| id | element | type | source/asset | event |
| ---- | ---- | ---- | ---- | ---- |
| 1 | favr.town logo | image |  | `onClick()` route to [https://favr.town](https://favr.town) |
| 2 | header image | image | (see "header and footer image") | n/a  |



##### header for all views except [/](/)

This header is the same as the header for [/](/) plus the following element::

| id | element | type | source/asset | event |
| -------- | -------- | -------- | -------- | -------- |
| 3 | back button | button | icon | route to [/](/), *keep query parameters* |



![](https://img.favr.town/spec/header_with_back.jpg)

For the back button, simply use a standard   X   icon, aligned opposite of the logo on the right.



##### standard footer

The footer mirrors the header background image, and may have additional elements based on each view. The footer is fixed and aligned with the bottom of the screen, does not move on scroll.

Elements:

| id | element | type | source/asset | details |
| --- | ------ | ---- | ------------ | ------- |
| 1 | footer image | image/included | | |
| 2 | (optional buttons) | | | (view specific) |



Measurements:

| measurement | pixel | % screen width | horizontal align | vertical align |
| --- | --- | --- | --- | --- |
| footer width | 1440 | 100 | center | top |
| footer height | 259 | 18 | | |
|  |  |  | | |
| background image width | 1440 | 100 | center | bottom |
| background image height | 245 | 17 | | |

*Other elements fit relative to the background image.*



The footer may include additional elements based on the view.



#### Images

*Note: Because the site is server-rendered through https://favr.town or https://dev.favr.town, all static assets (fonts, icons, images) will be served through a CDN with a separate domain (https://img.favr.town/app/) (see redirection paths in "Routes in this sprint").*

*Assets for images are temporary and will be replaced with final artwork later (png). **



root url: `https://img.favr.town/ssr`

| view url | header | content | footer |
| ---- | ---- | ---- | ---- |
| / | /root-hd.png | /root-bg.png | /root-ft.png |
| /join | /join-hd.png | /join-bg.png | /join-ft.png |
| /terms | /terms-hd.png | /terms-bg.png | /terms-ft.png |
| /privacy | /privacy-hd.png | /privacy-bg.png | /privacy-ft.png |
| /signup | /signup-hd.png | /signup-bg.png | /signup-ft.png |



#### Icons

Standard bootstrap icons are used with the exception of a few custom icons, mostly in the footer. 

Icons should be included with the build, and svg-formatted icons should (custom svg: later) be included as code.



*Assets for custom icons are temporary and will be replaced with final artwork later (svg).*



root url: `https://img.favr.town/ssr`

| view url or modal | header | content | footer |
| ---- | ---- | ---- | ---- |
| / | /logo.png |  | /root-signin.png<br/>/root-join.png |
| /join | /logo.png | /join-patron.png<br/>/join-merchant.png |  |
| /terms | /logo.png |  |  |
| /privacy | /logo.png |  |  |
| /signup | /logo.png |  |  |



## Resources and Reference

preactjs
[https://preactjs.com/guide/v10/server-side-rendering/](https://preactjs.com/guide/v10/server-side-rendering/)

server side rendering
[https://aws.amazon.com/blogs/compute/building-server-side-rendering-for-react-in-aws-lambda/](https://aws.amazon.com/blogs/compute/building-server-side-rendering-for-react-in-aws-lambda/)

form validation
[https://codeburst.io/how-to-use-html5-form-validations-with-react-4052eda9a1d4](https://codeburst.io/how-to-use-html5-form-validations-with-react-4052eda9a1d4)

style
[material UI](https://github.com/material-components/material-components-web)
[Baloo Chettan 2](https://fonts.google.com/specimen/Baloo+Chettan+2)

icons
[https://medium.com/swlh/are-you-using-svg-favicons-yet-a-guide-for-modern-browsers-836a6aace3df](https://medium.com/swlh/are-you-using-svg-favicons-yet-a-guide-for-modern-browsers-836a6aace3df)
