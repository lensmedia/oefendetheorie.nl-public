### `POST /access-code/{accessCode}/enable`
Used to enable an access code. User can continue with practising exams/exercises, no reason is required.

#### Request
##### Parameters
| Name         | Type   | Description                                                                    |
|--------------|--------|--------------------------------------------------------------------------------|
| `accessCode` | string | The access code to enable, the 16 character long hex string they use to login. |

##### Example
Enable user with access code `bdbf9f26ee4ffc8e`
```
POST /api/access-code/bdbf9f26ee4ffc8e/enable HTTP/1.1
Host: https://test.oefendetheorie.nl
Authorization: Basic F00B4R=
Content-Type: application/json
```

#### Response
Returns generic access code result. Root object `subscription` has a key `disabledAt`. if this is a timestamp => user is disabled, if this is null => user is enabled.
**Note;** This is a local machine response. Access code and output may differ on test and live servers.
```json
{
    "id": "01K8AP604YVK3FZRZ9XHX8PSB6",
    "createdAt": "2025-10-24T10:43:21+02:00",
    "code": "bdbf9f26ee4ffc8e",
    "duration": "P0Y0M0DT10H0M0S",
    "period": null,
    "expiresAt": "2028-10-01T00:00:00+02:00",
    "subscription": {
        "id": "01K8AP609XDDF3XHNXV946AGS5",
        "email": null,
        "course": {
            "id": "01K8AP4ZG6Q3ASK3TKB2VKRGFY",
            "title": "Personenauto",
            "description": null,
            "country": "BE",
            "locale": "nl_BE",
            "slug": "auto",
            "href": "https://oefendetheorie.be/be/auto"
        },
        "createdAt": "2025-10-24T10:43:21+02:00",
        "activatedAt": "2025-10-24T10:44:39+02:00",
        "disabledAt": null,
        "timeLeft": 35985,
        "accessEndsAt": null
    },
    "login": "https://192.168.11.222:9310/be/inloggen?code=bdbf9f26ee4ffc8e",
    "loginPage": "https://192.168.11.222:9310/be/inloggen"
}
```
