# Oefen de Theorie

Before the API can be used the user needs to be added by us, pricing will also be determined beforehand.

## API
Uses [basic authentication](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Authentication) with your LENS-ID and password, failure to authenticate should return a [401 Unauthorized](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status/401) response.

The API (should) only use JSON requests/responses when data is posted or sent. Empty responses will have the status code [204 No Content](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/204).

### Root
#### Test
_Requires whitelisting before use._

_Test API only has a NL domain, but the usage stays the same for live._
 
- https://test.oefendetheorie.nl/api

Test credentials are provided by us, these can not be changed as they are reset regularly.

#### Live
- Netherlands: http://oefendetheorie.nl/api
- Belgium: http://oefendetheorie.be/api

_The used domain does not really matter when creating an access code with a preselected course (usually the case). Otherwise the domain decides the domain and accept-language header the preferred locale for the login routes (there is an example in the new access code endpoint response)._

### Data types
#### String
If you don't know, you should find someone that does before we continue.

#### Ulid
String representation of ULID (similar to UUID's) mainly used for our identifiers for resources. An example could be `01K21T0WEWA1X4K3CMXW5ZQABN`.

#### Duration
Duration is a string formatted in a specific way, see [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Durations).

**Note;** we transform `year` and `month` to days as they have a variable amount of days depending on the year or month. To avoid angry customers, when provided, we always assume that a year is 366 days and a month is 31 days. Any response from us SHOULD NEVER return a value in `year` or `month`. If you want to represent it in that way you should convert it yourself.

**Note;** any value will be proportionally converted and multiplied to the agreed upon pricing at the moment of creating a purchase.

Some examples:

| Duration   | Description                                            |
|------------|--------------------------------------------------------|
| `P1D`      | 1 day                                                  |
| `P1M`      | 1 "month"                                              |
| `P31D`     | 31 days, also 1 "month"                                |
| `P2M5DT3H` | 2 months, 5 days, 3 hours. Seen as 67 days and 3 hours |

Parsing the string representation in php can be simplified using https://www.php.net/manual/en/class.dateinterval.php

**Note;** values in a duration are additive, e.g. `P1M31D` is the same as `P2M` (62 days), setting 93 days does not automatically convert it to 3 months in the notation, it remains `P93D`.

## Endpoints
### `POST /access-code`
Used to purchase an access code.

#### Request
##### Body
| Property   | Type                    | Description                                                                                                                                                                                     |
|------------|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `course`   | [`ulid`](#string)       | ID for the course the access code will be bound to, YOU WILL GET THESE FROM US. **Note;** these values are different between test and live.                                                     |
| `period`   | [`duration`](#duration) | (optional if duration is provided) A period is a stretch of time the user can freely access the platform starting when it uses the code for the first time.                                     |
| `duration` | [`duration`](#duration) | (optional if period is provided) Duration is used to give the user an X amount of "seconds" of access, and is only used up when the user is actively practising and no period access is active. |
| `email`    | [`string`](#string)     | Email address of the user receiving the access code, the user can use this to recover their access code. We do not use this for anything else.                                                  |

###### Example
Create an access code with 31 days of free access when the users logs in for the first time.
```
POST /api/access-code HTTP/1.1
Host: https://test.oefendetheorie.nl
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
POST /api/access-code HTTP/1.1
Host: https://test.oefendetheorie.nl
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
