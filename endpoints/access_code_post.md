### `POST /access-code`
Used to purchase an access code.

#### Request
##### Body
| Property   | Type                                | Description                                                                                                                                                                                     |
|------------|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `course`   | [`ulid`](../readme.md#string)       | ID for the course the access code will be bound to, YOU WILL GET THESE FROM US. **Note;** these values are different between test and live.                                                     |
| `period`   | [`duration`](../readme.md#duration) | (optional if duration is provided) A period is a stretch of time the user can freely access the platform starting when it uses the code for the first time.                                     |
| `duration` | [`duration`](../readme.md#duration) | (optional if period is provided) Duration is used to give the user an X amount of "seconds" of access, and is only used up when the user is actively practising and no period access is active. |
| `email`    | [`string`](../readme.md#string)     | Email address of the user receiving the access code, the user can use this to recover their access code. We do not use this for anything else.                                                  |

##### Example
Create an access code with 31 days of free access when the users logs-in for the first time.
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
Returns generic access code result. Root access code object `duration` and `period` contain the total sum of time granted. Subscription `timeLeft` (duration in seconds) and `accessEndsAt` (period end as datetime) contain the curren time left WHEN ACTIVATED.
**Note;** login path can vary depending on the course selected/ country selected and between environments.
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
        "timeLeft": 0,
        "accessEndsAt": null,
        "course": {
            "id": "01K21T0WEWA1X4K3CMXW5ZQABN",
            "title": "Personenauto",
            "description": null,
            "country": "BE",
            "locale": "nl_BE"
        },
        "createdAt": "2025-08-07T12:48:14+02:00",
        "activatedAt": null
    },
    "login": "https://test.oefendetheorie.nl/be/inloggen?code=43f86f76f9162065",
    "loginPage": "https://test.oefendetheorie.nl/be/inloggen"
}
```
