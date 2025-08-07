# Oefen de Theorie

Quick summary for access code API call

Before the API can be used the user needs to be added by us, pricing will be determined beforehand.

## API
Uses basic authentication with your LENS-ID and Password, failure to authenticate should return a 401 response.

The API (should) only use JSON requests/responses when data is posted or sent.
### Root
#### Test API
_Requires whitelisting before use._

- https://test.oefendetheorie.nl/api

#### Live API
- Netherlands: http://oefendetheorie.nl/api
- Belgium: http://oefendetheorie.be/api

The used domain does not really matter, but it is recommended to use the matching country when generating codes used for a course tied to said country to avoid any possible future refactors.

### Data types
#### String
If you don't know, you should find someone that does before we continue.
#### Ulid
String representation of ULID (very similar to [UUID V7](https://en.wikipedia.org/wiki/Universally_unique_identifier#Version_7_(timestamp_and_random))) mainly used for our identifiers for resources. An example would be `01K21T0WEWA1X4K3CMXW5ZQABN`.
#### Duration
Duration is a string formatted in a specific way, see [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Durations).

**Note;** we transform `year` and `month` to days as they have a variable amount of days depending on the year or month. To avoid angry customers, when provided, we always assume that a year is 366 days and a month is 31 days. Any response from us SHOULD NEVER return a value in `year` or `month`. If you want to represent it in that way you should convert it yourself.

**Note;** values in a duration do not automatically wrap, e.g. `P2M` (2 months) is the same as `P62D` or `P1M31D` (62 days).

**Note;** any value will be proportionally converted and multiplied to the agreed upon pricing at the moment of creating a purchase.

Some examples:

|Duration|Description|
|-|-|
|`P1D`|1 day|
|`P1M`|1 "month"|
|`P31D`|31 days|
|`P2M5DT3H`|2 months, 5 days, 3 hours. Seen as 67 days and 3 hours|

Parsing the string representation in php can be simplified using https://www.php.net/manual/en/class.dateinterval.php

## Endpoints
### `POST /access-code`
Used to purchase an access code.

#### Request
##### Body
|Property|Type|Description|
|-|-|-|
|`course`|[`ulid`](#string)|ID for the course the access code will be bound to, you will get this from us. **Note;** these values are different between test and live.|
|`period`|[`duration`](#duration)|(optional if duration is provided) A period is a stretch of time the user can freely access the platform starting when it uses the code for the first time.|
|`duration`|[`duration`](#duration)|(optional if period is provided) Duration is used to give the user an X amount of "seconds" of access, and is only used up when the user is activly practising and no period access is active.|
|`email`|[`string`](#string)|Email address of the user receiving the access code, the user can use this to recover their access code. We do not use this for anything else.|

###### Example
Create an access code with 31 days of free access when the users logs in for the first time.
```
POST https://test.oefendetheorie.nl/api/access-code
Authorization: Basic F00B4R=
Content-Type: application/json
```
```json
{
  "course": "01K21T0WEWA1X4K3CMXW5ZQABN",
  "period": "P1M",
  "email": "some-user@domain.com"
}
```

Create an access code with 10 days of free access and then after, 5 extra hours to use at any time the user wants.
```
POST https://test.oefendetheorie.nl/api/access-code
Authorization: Basic F00B4R=
Content-Type: application/json
```
```json
{
    "course": "01K21T0WEWA1X4K3CMXW5ZQABN",
    "period": "P10D",
    "duration": "PT5H",
    "email": "some-user@domain.com"
}
```

#### Response
```json
{
    "id": "01K222AK7830YFD599GQXJM0VV",
    "createdAt": "2025-08-07T12:48:13+02:00",
    "code": "43f86f76f9162065",
    "duration": "P0Y0M0DT5H0M0S",
    "period": "P0Y0M10DT0H0M0S",
    "expiresAt": "2028-08-01T00:00:00+02:00",
    "subscription": {
        "id": "01K222AKR45D1YFV5WGWJS40CK",
        "email": "some-user@domain.com",
        "course": {
            "id": "01K21T0WEWA1X4K3CMXW5ZQABN",
            "title": "Personenauto",
            "description": null,
            "country": "BE",
            "locale": "nl_BE"
        },
        "createdAt": "2025-08-07T12:48:14+02:00"
    },
    "login": "https://test.oefendetheorie.nl/be/inloggen?code=43f86f76f9162065",
    "loginPage": "https://test.oefendetheorie.nl/be/inloggen"
}
```

**Note;** login path can vary depending on the course selected/ country selected and between environments.
