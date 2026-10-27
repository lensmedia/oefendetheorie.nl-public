### `GET /access-code/{accessCode}`
Used to gain information from an access code.

#### Request
##### Parameters
| Name         | Type   | Description                                                                    |
|--------------|--------|--------------------------------------------------------------------------------|
| `accessCode` | string | The access code to look up, the 16 character long hex string they use to login. |

##### Example
Look up the details of access code: 01a23b45c67d89ef 
```
GET /api/access-code/01a23b45c67d89ef HTTP/1.1
Host: https://test.oefendetheorie.nl
Authorization: Basic F00B4R=
Content-Type: application/json
```

#### Response
Returns generic access code result.
Root access code object `duration` and `period` contain the total sum of time granted. Subscription `timeTotal`/`timeLeft` (duration in seconds) and `accessEndsAt` (period end as datetime) contain the current time left WHEN ACTIVATED.
`timeTotal` is the sum of all purchased/granted hours. Seconds are extracted from `timeLeft` if the user's `accessEndsAt`
is expired or non-existent. 
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
        "timeTotal": 18000,
        "timeLeft": 17251,
        "accessEndsAt": "2025-09-06T12:48:13+02:00",
        "course": {
            "id": "01K21T0WEWA1X4K3CMXW5ZQABN",
            "title": "Personenauto",
            "description": null,
            "country": "BE",
            "locale": "nl_BE"
        },
        "createdAt": "2025-08-07T12:48:14+02:00",
        "activatedAt": "2025-12-07T20:31:12+02:00"
    },
    "login": "https://test.oefendetheorie.nl/be/inloggen?code=43f86f76f9162065",
    "loginPage": "https://test.oefendetheorie.nl/be/inloggen"
}
```
