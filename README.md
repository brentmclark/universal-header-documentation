# Universal Header
The header (and footer) that all SportsHub Network sites use.

- [Configuring Your Site](#Configuring-Your-Site)
- [Running the Code Locally](#Running-the-Code-locally)

## Configuring Your Site

- [Initial Setup](#Initial-Setup)
- [Advanced Configuration Options](#Advanced-Configuration-Options)


### Initial Setup

1. Add a configuration object to the `window`.  Here's an example:
    ``` html
    <script>
      var config = {
        theme: 'leaguesafe',
        navItems: [
          { text: 'Nav Item 1',
            url: '#',
            subItems: [
              {text: 'Subnav Item 1', url: '#', description: 'I am helper text for this link.'},
              {text: 'Subnav Item 2', url: '#'},
              {text: 'Subnav Item 3', url: '#'},
              {text: 'Subnav Item 4', url: '#'},
            ]
          },
          { text: 'Nav Item 2', url: '#'},
          { text: 'Nav Item 3', url: '#'},
        ],
        helpUrl: '#',
        loginUrl: '#',
        logoutUrl: '#',
        signupUrl: '#',
        onLinkClick: function(event, href) {
          // Perform any onClick actions you'd like in here.
        },
        accountSettingsSubItems: [
          { text: 'Account Settings SubItem 1', url: '#'}, // see Links section below
        ]
        tickets: {
          label: 'Tickets',
          count: 1,
          url: '/account/tickets'
        },
        contentMaxWidth: '1280px',
        accessToken: null,
        idToken: null,
      };
      window.shgn = window.shgn || function() {(shgn.q = shgn.q || []).push(arguments)}
      shgn('init', config)
    </script>
    ```

2. Drop the script tag below into the `<head>` of your site.
    ``` html
    <script async src="https://assets.shub.dog/static/js/shgn-init.js"></script>
    ```
    **Note:** The `shgn-init.js` file is a bootstrapper and loads in the header asynchronously.  It should be placed as high up in the `head` of the document as possible.  If it's not placed in the `head` the universal header code will load in later than desired.

3. Add a placeholder div where you want the header to render.
    ``` html
    <div id="universal-header"></div>
    ```

4. Drop in a style block to prop open the header area while it loads in.
    ``` html
    <style>
      #universal-header {
          background-color: #222;
          height: 48px;
          width: 100%;
      }

      @media (min-width: 1000px){
        #universal-header {
          height: 145px;
        }
      }
    </style>
    ```

5. Pick a theme from the [available options](https://github.com/sportstech/universal-header/blob/dev/src/contexts/theme-context.js#L5)
    - leaguesafe
    - fanball
    - mfl10s
    - nfbc
    - nffc
    - nfbkc
    - cdm
    - wis
    - shgn
    - bid2play

6.  Handle authenticated users
    in the config section above, set the `accessToken` and the `idToken` to the values provided by the identity server.

### Advanced Configuration Options

- [Config Object](#Config-Object)
- [Links](#Links)
- [Wallet API URL](#Wallet-API-URL)

---
#### Config Object
The `config` object allows for the following options:
* [`theme`](#theme) :: `string`
* [`navItems`](#navItems) :: `array`
* [`helpUrl`](#helpUrl) :: `string`
* [`loginUrl`](#loginUrl) :: `string`
* [`logoutUrl`](#logoutUrl) :: `string`
* [`signupUrl`](#signupUrl) :: `string`
* [[`onLinkClick`]](#onLinkClick) :: `function`
* [[`accountSettingsSubItems`]](#accountSettingsSubItems) :: `array`
* [[`tickets`]](#tickets) :: `object`
* [[`contentMaxWidth`]](#contentMaxWidth) :: `string`
* [`accessToken`](#accessToken) :: `string`
* [`idToken`](#idToken) :: `string`

##### theme

options :: one of the following
  * `cdm`
  * `nffc`
  * `wis`
  * `nfbc`
  * `nfbkc`
  * `fanball`
  * `mfl10s`
  * `leaguesafe`
  * `bid2play`

##### navItems

An `array` of [`Link`](#Links) objects

##### helpUrl

The fully qualified URL of the help page for this site.

##### loginUrl

The login URL for this site.

##### logoutUrl

The logout URL for this site.

##### signupUrl

The signup URL for this site.

##### onLinkClick

A `function` to be called on main nav link clicks.  The intended purpose of this function is to allow SPAs (e.g. React, Angular) to engage their client-side router instead of a hard page transition.

The function will receive the following arguments:
* `event` - The DOM event object
* `href` - The `href` of the clicked anchor.  This is identical to `event.target.href`

> *`optional` - If not provided, no function will be called on link clicks*

##### accountSettingsSubItems

An `array` of [`Link`](#Links) objects

> *`optional` - If not provided, no sublinks will be rendered*

##### tickets

sample object:

``` javascript
{
  label: 'Tickets',
  count: 1,
  url: '/account/tickets'
},
```

> *`optional` - If not provided, no ticket section will be rendered*

##### contentMaxWidth

The maximum width for the internal elements of the universal header.

> *`optional` - If not provided, the default of `1280px` will be used*

##### accessToken

An `access_token` as provided by the `IDSRV`

> *`optional` - If not provided, user is not considered logged in*

##### idToken

An `id_token` as provided by the `IDSRV`

> *`optional` - If not provided, user is not considered logged in*

---
#### Links

All links in the universal nav support the following properties:
- text :: `string`
- url :: `string` -- *(either a relative path or a complete URI)*
- description :: `string` -- *(only available on `subItems`)*
- data :: `object` -- *(wrap keys in quotes)*

*example*:

``` javascript
// Sample Link
{
  text: 'Home',
  url: '/',
  description: 'I am helper text for this link',
  data: { 'data-id':'my-object-id' }
}
```
---
### Wallet API URL

The Wallet API URL can be set in the `config` object.  This is intended for local testing only, and should not be leveraged for production.

*example*:

``` javascript
var config = {
  // ... additional config options

  walletApiUrl = 'http://localhost:7000',

  // ... additional config options
}
```


## Running the Code Locally
- [Requirements](#Requirements)
- [Getting Started](#Getting-Started)
- [Common Tasks](#Common-Tasks)
  - [Run Code in Dev Mode](#Run-Code-in-Dev-Mode)
  - [Triggering a logged-in state](#Triggering-a-logged-in-state)
- [Uncommon Tasks](#Uncommon-Tasks)
  - [Run Production Build](#Run-Production-Build)
  - [Serve Production Build Locally](#Serve-Production-Build-Locally)
  - [Serve Sample Site Locally](#Serve-Sample-Site-Locally)

### Requirements
In order to run this project successfully you'll need the following:

- `Node`
- `yarn`
- `serve` :: `yarn add serve -g`

### Getting Started
---
Make sure you have [installed all the requirements](#Requirements) and then:

`yarn`

After that, you'll probably want to [run the code in dev mode](#Run-Code-In-Dev-Mode)

### Common Tasks
---
#### Run Code in Dev Mode

`yarn start`

####  Triggering a logged-in state

1. Head over to the [LeageSafe QA Claims](https://consumer-qa.shub.dog/home/claims) section and grab both your `id_token` and `access_token`.
2. Then, open the `/public/index.html` file and paste in the new values.

### Uncommon Tasks
---

#### Run Production Build
`yarn build`

#### Serve Production Build Locally
1. Make sure you [run the production build](#Run-Production-Build) first
2. run `serve -s build`

#### Serve Sample Site Locally
1. Make sure you [serve the production build locally](#Serve-Production-Build_locally) first
2. Open another terminal window
3. `serve -s sample-site -p 5001`
