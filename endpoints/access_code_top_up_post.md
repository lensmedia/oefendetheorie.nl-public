### `POST /access-code/{accessCode}/top-up`
Used to top up an access code.

* If the user has already activated the access code, the new period will be appended to the end of the remaining period.
* If the user has no period left, the new period starts at the moment of this request with the duration provided.

#### Request
##### Parameters
| Name     | Type   | Description                                                                 |
|----------|--------|-----------------------------------------------------------------------------|
| `accessCode` | string | The access code to top up. |
##### Body
| Property   | Type                    | Description                                                                                                                                                                                     |
|------------|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `period`   | [`duration`](readme.md#duration) | (optional if duration is provided) A period is a stretch of time the user can freely access the platform starting when it uses the code for the first time.                                     |
| `duration` | [`duration`](readme.md#duration) | (optional if period is provided) Duration is used to give the user an X amount of "seconds" of access, and is only used up when the user is actively practising and no period access is active. |

##### Example
Adds an extra 31 days of free access to the user.
```
POST /api/access-code/01a23b45c67d89ef/top-up HTTP/1.1
Host: https://test.oefendetheorie.nl
Authorization: Basic F00B4R=
Content-Type: application/json
```
```json
{
  "period": "P1M"
}
```

#### Response
Returns generic access code result. Access code duration and period contain the total sum of time granted. Subscription timeLeft (duration in seconds) and accessEndsAt (period end as datetime) contain the current (after top-up) time left WHEN ACTIVATED.
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
        "timeLeft": 432551,
        "accessEndsAt": "2025-09-06T12:48:13+02:00",
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
