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
- [GET /course](endpoints/course_get.md) - Get all courses
- [POST /access_code](endpoints/access_code_post.md) - Create an access code
- [POST /access_code/{accessCode}/top-up](endpoints/access_code_top_up_post.md) - Top up an access code
