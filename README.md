# FAVR.TOWN

Specification
07/27/2021

Project repository:
[ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/favr.town.frontend.ssr](ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/favr.town.frontend.ssr)

Status:
**FINAL**



Recommended editor/viewer for markdown: [https://typora.io/](https://typora.io/)



## Summary

This sprint enhances the application specified in SPRINT A-D



## Modifications

Implement the following modifications. 



### API

Currently, the application uses `utility.js` for all data models (public, terms, signup).
Implement axios and a spinner component to use a REST API for the "signup" data model.



API endpoints are

`https://u-dev-api.favr.town` (development)
`https://u-api.favr.town` (production)



#### utilityGet()

Returns dynamic content (merchant, news, coupon, and survey)

**this remains with utility.js**



#### utilityTerms()

Returns terms of service and privacy policy.

**this remains with utility.js**



#### utilityValidate()

Returns form validation results

**change to axios with `signup POST`**



#### utilitySignup()

Account creation, returns success message

**change to axios with `signup PATCH`**



### Rendering

Currently the app uses client-side rendering, a dynamic router, and is build (babel, webpack) for the browser.

Change to server-rendering inside a NodeJS function (AWS Lambda) using a static router, and preact render-to-string. This will require updating the build scripts to produce "commonjs" for a "node" target instead of "umd" for browsers.



### Optimization

- include custom routes to support sitemap, google/bing auth, robots, and favicon
- update/minimize dependencies
- resolve/minimize build warnings





## Deliverable

This specification delivers a lint-free (eslint), working, and tested state-of-the-art web application, as described in this document, to serve the dynamic responsive site [https://favr.town](https://favr.town), with server-side rendering for AWS Lambda (NodeJS), including all project source and build configuration files, 3rd party libraries, assets, and applicable documentation, optimized for current browsers (Chrome, Firefox, Safari), mobile first, with support for current mobile (Android and iOS), tablet, and desktop viewing modes, and following industry best practices.

