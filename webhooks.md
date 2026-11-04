## Webhooks

Webhook URLs are set up by us; send us the URL you want to receive them on.

Each webhook is a `POST` request with a JSON body to your URL. Respond with any `2xx` status code within 5 seconds. Any other status code, or no response in time, counts as a failed delivery and is retried 3 times, after roughly 1, 2 and 4 seconds.

### Item completed
Sent each time a user completes an item (exercise or exam) with an access code created by you.

| Name         | Type   | Description                                                                                                                                       |
|--------------|--------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| `event`      | string | Always `item_completed`.                                                                                                                          |
| `accessCode` | string | The access code of the user, the 16 character long hex string they use to login.                                                                |
| `result`     | object | The result, in the same format as one entry of [GET /access-code/{accessCode}/results](endpoints/access_code_results_get.md). |

#### Example
```
POST /your/webhook/url HTTP/1.1
Host: your-domain.com
Accept: application/json
Content-Type: application/json
```
```json
{
    "event": "item_completed",
    "accessCode": "bdbf9f26ee4ffc8e",
    "result": {
        "createdAt": "2025-11-26T13:29:55+01:00",
        "context": {
            "passed": true,
            "session": "01KB028EYKE3CMHR2W61SNEJ2A",
            "mapped_score": 8,
            "category_item": {
                "id": "01K8AP5798K44N895D0258EDM8",
                "item": {
                    "id": "58692b4f-f66e-40df-a6d0-70e062daa0e1",
                    "type": "exercise",
                    "title": "Deeltoets 2.2",
                    "modifiedAt": "2025-08-07T13:45:32+02:00",
                    "originalId": 922
                },
                "slug": "2-2",
                "title": "Deeltoets 2.2",
                "category": {
                    "id": "01K8AP4ZGBNV7ZB21Y9WEQDZQQ",
                    "slug": "opdrachten",
                    "title": "Opdrachten",
                    "description": null
                }
            },
            "subject_count": {
                "BE2": 8
            },
            "achieved_score": 8,
            "subject_errors": []
        }
    }
}
```
