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


## My Approach:

Currently utility.js is made up of Vanila Javascript so I will have to use ASYNC function to get response from api endpoints.
```javascript

export function async requestUrl({url, method="GET", body={}}, options){
    if(method==="POST") {
        /**
         * then body is parsed
         */
    }
    /**
     * @todo initiate http request using axios
     */
    const response  = await axios({...});
    if (response === 200) {
        status = 200;
    }
    const jsonPayload = await response.json();

    // Status: it holds the status of API response containing [error, success, loading],
    // jsonPayload: it will contain the response object from API endpoints
    return [status, jsonPayload];
}

```

Note:- If the request is initiated from preact components then I will use custom hook to get data from API endpooints

```javascript

export function useBaseHook({url, method="GET", body={}}, options){
    const [status, setState] = useState();
    const [response, setResponse] = useState()

    useEffect(()=> {
        if(method==="POST") {
            /**
             * then body is parsed
             */
        }
        /**
         * @todo initiate http request using axios
         */
    });

    // Status: it holds the status of API response containing [error, success, loading],
    // response: it will contain the response object from API endpoints
    return [ status, response];

}


```



API endpoints are

`https://u-dev-api.favr.town` (development)
`https://u-api.favr.town` (production)



#### utilityGet()

Returns dynamic content (merchant, news, coupon, and survey)


### My Approach

instead of passing raw data from utility.js I will fetch data from API enpoints using custom (hooks or Async/wait) function depending upon my request.

```javascript
if (event.params.m) {
	if (event.params.m === "m_g56aDAS")
		return await requestUrl({url:'', method:'GET', ...}...)

	throw "not found";
}
```

**this remains with utility.js**



#### utilityTerms()

Returns terms of service and privacy policy.

instead of passing raw data from utility.js I will fetch data from API enpoints using custom async/wait function.

```javascript
async function utilityTerms() {
	return await requestUrl({url:''})
}
```

**this remains with utility.js**



#### utilityValidate()
Returns form validation results

I will use custom hook/async function to avail axois request to get data from API endpoints

**change to axios with `signup POST`**



#### utilitySignup()

Account creation, returns success message

I will use custom hook/async function to avail axois request to get data from API endpoints

**change to axios with `signup PATCH`**



### Rendering

Currently the app uses client-side rendering, a dynamic router, and is build (babel, webpack) for the browser.

Change to server-rendering inside a NodeJS function (AWS Lambda) using a static router, and preact render-to-string. This will require updating the build scripts to produce "commonjs" for a "node" target instead of "umd" for browsers.


### My Approach
I will create a server file which will implements a shallow rendering of preact node tree. and return that generated string as a html file.

```javascript
import { shallow } from 'preact-render-to-string';
import { h } from 'preact';

import App from './App.jsx';
/**
 * @todo return this as a html file for every request
 */
const htmlAsAString = shallow(App);
```

### Optimization

- include custom routes to support sitemap, google/bing auth, robots, and favicon
- update/minimize dependencies
- resolve/minimize build warnings

Previous developer is currently using preact-roter in this project it provides dynamic and custom routes already, so I will create user endpoints for sitemap, robots.
I will create preact components which will initae the OAuth 2.0 once the endpoints are provided. 
I will honor eslint configuration through code and build process and eliminate warnings and errors caused during build and deployment process.  



## Deliverable

This specification delivers a lint-free (eslint), working, and tested state-of-the-art web application, as described in this document, to serve the dynamic responsive site [https://favr.town](https://favr.town), with server-side rendering for AWS Lambda (NodeJS), including all project source and build configuration files, 3rd party libraries, assets, and applicable documentation, optimized for current browsers (Chrome, Firefox, Safari), mobile first, with support for current mobile (Android and iOS), tablet, and desktop viewing modes, and following industry best practices.

