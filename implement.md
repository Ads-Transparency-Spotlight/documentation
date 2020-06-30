# Implement the Ads Transparency Spotlight(Alpha) Data Disclosure schema

The Ads Transparency Spotlight extension for Chrome shows website visitors information about the ads on a web page. However, the extension can only display information about ads displayed using the Ad Disclosure Schema.

At the time of this alpha release (July 2020), the extension only shows information about those ads purchased through Google Ads that have implemented the Ads Transparency Spotlight(Alpha) Data Disclosure schema. As additional ad tech providers implement this schema, information about these ads will also appear in the extension. Over time, we hope the industry will incorporate the Ads Transparency Spotlight(Alpha) Data Disclosure schema into their ads.

## How the schema works

In this initial release, the Ads Transparency Spotlight extension scans the DOM tree of a web page. It searches for the metadata in a DOM element with the following special characters: 

```html
<meta name="AdsMetadata" content="{{ADS_METADATA}}"/>
```

`{{ADS_METADATA}}` is a JSON message that conforms to `ads_metadata.schema.json`.

The Ads Transparency Spotlight extension then picks up the tag from the DOM tree and parses the JSON message to present to the end user.

For more information about the schema’s JSON structure, you can review the Ad Disclosure Schema JSON file.

## Fields descriptions

Review the following descriptions to learn more about the enum string fields used in the Ads Transparency Spotlight (Alpha) Data Disclosure schema.

### `atps/id_type and advertising_platform/id_type`

- `GOOGLE_ATP_ID`: Google internal ID for ad technology providers.
- `IAB_GVL_ID`:  IAB ID for ad technology providers. For more details read the [IAB's website "TCF – Transparency & Consent Framework"](https://iabeurope.eu/transparency-consent-framework/).

### `targeting_category/geo_location`

- `GEO_LOCATION_NOT_DISCLOSED`: Default value. Unclear whether geolocation data was used.
- `GEO_LOCATION_NOT_USED`: Geolocation data wasn’t used.
- `GEO_LOCATION_APPROXIMATE`: Geolocation data is based on either fuzzified lat-long or IP-derived location.
- `GEO_LOCATION_PRECISE`: Precise geolocation data, as defined by the IAB TCF v2.0.
- `GEO_LOCATION_UNKNOWN_TYPE`: Other geolocation types, including cases where both APPROXIMATE and PRECISE are used or where APPROXIMATE or PRECISE cannot be determined.

### `targeting_category/remarketing`

- `REMARKETING_NOT_DISCLOSED`: Default value. Unclear whether remarketing is used.
- `REMARKETING_NOT_USED`: Remarketing not used.
- `REMARKETING_THIRD PARTY`: The ad targeting is based on online or offline data about you from someone who may not be the advertiser.
- `REMARKETING_WEBSITE_VISIT`: The ad targeting is based on a previous visit to an advertiser’s website.
- `REMARKETING_UNKNOWN_TYPE- `: Other remarketing types not listed or undetermined. For example, `UPLOADED` or `WEBSITE_VISIT` cannot be determined.

### `targeting_category/user_characteristics`

- `<empty repeated field = NOT_DISCLOSED>`
- `USER_CHARACTERISTICS_NOT_USED`: User characteristics weren’t used.
- `USER_CHARACTERISTICS_GENDER`: The ad targeting criteria is (partially) based on either declared or inferred gender.
- `USER_CHARACTERISTICS_AGE_GROUP`: The ad targeting criteria is (partially) based on either declared or inferred age group.
- `USER_CHARACTERISTICS_UNKNOWN_TYPE`: Other user characteristics not listed.

### `targeting_category/user_interests`

- `USER_INTERESTS_NOT_DISCLOSED`: Default value. Unclear whether user interests are used.
- `USER_INTERESTS_NOT_USED`: User interests weren’t used.
- `USER_INTERESTS_USED`: The ad targeting criteria is (partially) based on either declared or inferred user interests.

### `targeting_category/context`

- `CONTEXT_NOT_DISCLOSED`: Default value. Unclear whether context is used.
- `CONTEXT_NOT_USED`: Context wasn’t used.
- `CONTEXT_USED`: The ad targeting criteria is (partially) based on either declared or inferred context.

### `targeting_category/other`

- `OTHER_NOT_DISCLOSED`: Default value. Unclear whether other information is used.
- `OTHER_NOT_USED`: Other information wasn’t used.
- `OTHER_USED`: The ad targeting criteria is (partially) based on other information, either declared or inferred.

## Formatting the JSON messages and metadata tag

JSON messages should be minified and escaped. The following example applies to AdsMetadata JSON:

```json
{
  "atps": [ {
    "id_type": "GOOGLE_ATP_ID",
    "id": 2,
    "name": "ATP with Google ID"
  }, {
    "id_type": "IAB_GVL_ID",
    "id": 3,
    "name": "ATP with IAB Global Vendor ID"
  } ],
  "advertising_platform": {
    "id_type": "GOOGLE_ATP_ID",
    "id": 4,
    "name": "Some Advertising Platform"
  },
  "targeting_category": {
    "geo_location_type": "APPROXIMATE",
    "remarketing_type": "WEBSITE_VISIT",
    "user_interests": true,
    "user_characteristic_types": [ "GENDER", "AGE_GROUP" ],
    "contextual": true,
    "other": true
  }
}
```

The metadata tag should be rendered as follows:

```html
<meta name="AdsMetadata" content="{\"atps\":[{\"id_type\":\"GOOGLE_ATP_ID\",\"id\":2,\"name\":\"ATP with Google ID\"},{\"id_type\":\"IAB_GVL_ID\",\"id\":3,\"name\":\"ATP with IAB Global Vendor ID\"}],\"advertising_platform\":{\"id_type\":\"GOOGLE_ATP_ID\",\"id\":4,\"name\":\"Some Advertising Platform\"},\"targeting_category\":{\"geo_location_type\":\"APPROXIMATE\",\"remarketing_type\":\"WEBSITE_VISIT\",\"user_interests\":true,\"user_characteristic_types\":[\"GENDER\",\"AGE_GROUP\"],\"contextual\":true,\"other\":true}}"
/>
```

---

## Table of contents

- [README](README.md)
- [Overview of the Ads Transparency Spotlight (Alpha) extension](overview.md)
- **Implement the Ads Transparency Spotlight (Alpha) Data Disclosure schema**
