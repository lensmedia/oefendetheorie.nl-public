### `POST /access-code/{accessCode}/disable`
Used to disable an access code. User can not continue with practising exams/exercises and is told to contact supplier.

#### Request
##### Parameters
| Name         | Type   | Description                                                                     |
|--------------|--------|---------------------------------------------------------------------------------|
| `accessCode` | string | The access code to disable, the 16 character long hex string they use to login. |
##### Body
| Property         | Type   | Description                                   |
|------------------|--------|-----------------------------------------------|
| `disabledReason` | string | Reason for disabling/blocking an access code. |
**Note;** disabledReason is required. Reason will not be shown to access code, he/she will be told to contact supplier. 
Reason is for our admin environment, so we also have some context in case we get contacted or whatsoever.

##### Example
Disable user with access code `bdbf9f26ee4ffc8e`
```
POST /api/access-code/bdbf9f26ee4ffc8e/disable HTTP/1.1
Host: https://test.oefendetheorie.nl
Authorization: Basic F00B4R=
Content-Type: application/json
```
```json
{
    "disabledReason": "Misbruik van het platform"
}
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
        "disabledAt": "2025-12-03T10:10:59+01:00",
        "timeLeft": 35985,
        "accessEndsAt": null
    },
    "login": "https://192.168.11.222:9310/be/inloggen?code=bdbf9f26ee4ffc8e",
    "loginPage": "https://192.168.11.222:9310/be/inloggen"
}
```
