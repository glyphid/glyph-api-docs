---
title: Glyph ID API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - node

toc_footers:

includes:
  - errors

search: true
---

# Introduction

Welcome to the **Glyph ID API** documentation.

You can use the following endpoints to interact with Glyph ID user data, including requesting, checking and accessing KYC verification data, as well as different types of identity particles (personal data) for our users.

We have language bindings in Shell and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.


# Integration Guide

<!-- Glyph ID Login enables people to sign into your webpage with their Glyph Identity. -->

<!-- The integration process is designed to fit with minimal effort into your website/app, since most of the steps are automatically taken care of by the Glyph Widget that comes with our JavaScript snippet. -->

To get started with Glyph API integration, follow the steps below:

1. Register for a Partner account at [Glyph ID Partner Portal](#), or sign into your account if you already have one.

2. Configure your integration settings by doing the following:
  - Specify preferred user identification method in the account. Choose one of **User ID** or **ETH Address**.
  - From the list of available identity particles check the ones you are interested in *checking* or *reading* in order to enable them for your integration.
  - *(Optional)* Next to each particle you enabled, select either **check** or **read**, if you want to request the particle data to be shared with you.
  - *(Optional)* You can provide a **webhook URL** that we'll use to send identity/particle updates to your servers, as they happen on our system.

<br />
3. Copy your **Secret API key** to use use for API authorization.

<!-- Implement Login with the following steps:

1. Log into [Glyph Partner Portal](#) and set a *Callback URL*, which we'll use to return user authentication data.
2. Copy your Glyph JavaScript snippet code and add it to the `<head>` of your website.
2. Install it on your websites header.

In order to use the **Glyph ID Login** feature, you need to specify a *callback* URL (and, optionally, a *webhook* URL) in the [Partner Portal](#).


1. Enter Your Redirect URL to take people to your successful-login page.
2. Check the login status to see if someone's already logged into your app. During this step, you should also check to see if someone has previously logged into your app, but is not currently logged in.
3. Log people in, either with the login button or with the login dialog, and ask for a set of **particle permissions**.
4. Log people out to allow them to exit from your app.

<aside class="notice">
example:
<br />
Callback URL: <code>https://partnerdomain.com/glyph_id_callback</code>
<br />
Webhook URL: <code>https://partnerdomain.com/glyph_id_webhook</code>
</aside> -->

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "<API_ENDPOINT_HERE>"
  -H "Authorization: <YOUR_API_KEY_HERE>"
```

```javascript
import glyph from 'glyph-id';

glyph.authorize('<YOUR_API_KEY_HERE>');
```

> Make sure to replace `YOUR_API_KEY_HERE` with your Secret API key.

Glyph API expects for the Secret API key to be included in all API requests to the server in a header that looks like the following:

If you're using Node.js, you can use Glyph API wrapper by running:

`yarn add glyph-id`

or

`npm install glyph-id --save`

<!-- <aside class="notice">
</aside> -->

# Identities

## Get All Identities

> Example Request

```shell
curl "http://api.glyph.id/identities"
  -H "Authorization: <YOUR_AUTH_CODE>"
```

```javascript
let identityList = glyph.identities();
```

> Example Response

```json
[
  {
    "id": 14551,
    "eth_address": "0x9b2f18d0d1d65f3c733c97f5df626b17917590d9"
  },
  {
    "id": 4818,
    "eth_address": "0x9b2f18d0l1d95f3c733c97f5df626b17917590d9"
  },
  {
    "id": 40091,
    "eth_address": "0x9b2f18d0d1d65f3c733e97f5df626b17917590d9"
  },
  ...
]
```

This endpoint retrieves the list of all identities specific to a partner (either user IDs or ETH addresses based on partner choice).

### HTTP Request

`GET http://api.glyph.id/identities`

### Query Parameters

This endpoint has no parameters.

<aside class="notice">
The API will determine the partner and return the identity list based on the authentication code provided in the request header.
</aside>

## Get a Specific Identity

```shell
curl "http://api.glyph.id/identities/0x9b2f18d0d1d65f3c733c97f5df626b17917590d9"
  -H "Authorization: 3b732HfjFHJ385J91mbbB934kakka4M3lK"
```

```javascript
let identity = glyph.identity.get('0x9b2f18d0d1d65f3c733c97f5df626b17917590d9');
```

> Example Response

```json
{
  "id": 53772,
  "eth_address": "0x9b2f18d0d1d65f3c733c97f5df626b17917590d9",
  "eth_address_verified": true,
  "KYC_status": "passed",
  "particles": {
    "first_name": "Max",
    "last_name": "Broon",
    "SSN": "0000",
    "birth_date": "02.04.1983",
    ...
  },
  ...
}
```

This endpoint retrieves a specific identity, including all the particle data that has been shared with the partner.

<!-- <aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside> -->

### HTTP Request

`GET http://api.glyph.id/identities/<ID|ETH_ADDRESS>`

### URL Parameters

Parameter | Description
--------- | -----------
ID or ETH_ADDRESS | The ID or ETH address of the identity to retrieve

# Particles

## Get data and verification status for a particle

> Example Request

```shell
curl "http://api.glyph.id/particles/kyc/0x9b2f18d0d1d65f3c733c97f5df626b17917590d9"
  -H "Authorization: 3b732HfjFHJ385J91mbbB934kakka4M3lK"
```

```javascript
let passedKYC = glyph.particle('kyc', '0x9b2f18d0d1d65f3c733c97f5df626b17917590d9');
```

> Example Response

```json
{
  "id": 53772,
  "eth_address": "0x9b2f18d0d1d65f3c733c97f5df626b17917590d9",
  "KYC": {
    "status": "processing",
    "verified": false
  }
}
```

This endpoint checks the verification data for the requested particle for a specific identity.

<aside class="notice">
If this particle has also been shared with you, the response will also include the particle data.
</aside>

### HTTP Request

`GET http://api.glyph.id/particles/<PARTICLE_NAME>/<ID|ETH_ADDRESS>`

### URL Parameters

Parameter | Description
--------- | -----------
PARTICLE_NAME | The name of the requested particle (must be enabled in the partner configuration) [Check the list of available particles](#)
User ID or ETH_ADDRESS | The User ID or ETH address of the identity to retrieve
