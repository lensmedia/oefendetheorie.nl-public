## Webhooks

Webhook URLs are set up by us; send us the URL you want to receive them on.

Each webhook is a `POST` request with a JSON body to your URL. Respond with any `2xx` status code within 5 seconds. Any other status code, a redirect, or no response in time counts as a failed delivery and is retried 3 times, after roughly 1, 2 and 4 seconds.

Every request carries these headers:

| Name                | Description                                                                                   |
|---------------------|-----------------------------------------------------------------------------------------------|
| `Webhook-Event`     | The event name, the same as `event` in the body.                                              |
| `Webhook-Id`        | Unique id of the event (Ulid). A retried delivery has the same id, use it to skip duplicates. |
| `Webhook-Signature` | Signature of the request, see [Verifying the signature](#verifying-the-signature).            |

### Verifying the signature
Verifying is optional; it proves a request comes from us and was not changed on the way. We give you a secret for it, ask us for a new one if it leaks.

The signature is `sha256=` followed by the HMAC-SHA256, in lowercase hex, of `Webhook-Event`, `Webhook-Id` and the raw request body joined together, with your secret as key. Compute it over the body exactly as received, before decoding the JSON, and compare it in constant time.

```php
$body = file_get_contents('php://input');
$expected = 'sha256='.hash_hmac('sha256', $_SERVER['HTTP_WEBHOOK_EVENT'].$_SERVER['HTTP_WEBHOOK_ID'].$body, $secret);

if (!hash_equals($expected, $_SERVER['HTTP_WEBHOOK_SIGNATURE'] ?? '')) {
    http_response_code(406);
    exit;
}
```

In Symfony, `Symfony\Component\Webhook\Client\RequestParser` does the same: `(new RequestParser())->parse($request, $secret)`.

### Item completed
Sent each time a user completes an item (exercise or exam) with an access code created by you.

| Name            | Type            | Description                                                                                     |
|-----------------|-----------------|-------------------------------------------------------------------------------------------------|
| `event`         | string          | Always `item_completed`.                                                                        |
| `accessCode`    | string          | The access code of the user, the 16 character long hex string they use to login.               |
| `completedAt`   | string          | When the item was completed, ISO 8601 with time zone offset.                                    |
| `item`          | object          | The completed item.                                                                             |
| `item.id`       | Ulid            | The item, the same as `category_item.id` in [the results](endpoints/access_code_results_get.md). |
| `item.title`    | string          | The title of the item.                                                                          |
| `passed`        | boolean or null | Whether the item was passed, `null` for items without a pass mark.                              |
| `mappedScore`   | integer or null | The number of questions, `null` for items without a score.                                      |
| `achievedScore` | integer or null | The number of correct answers, `null` for items without a score.                                |

Mistakes per subject are in [GET /access-code/{accessCode}/results](endpoints/access_code_results_get.md).

#### Example
```
POST /your/webhook/url HTTP/1.1
Host: your-domain.com
Content-Type: application/json
Webhook-Event: item_completed
Webhook-Id: 01KB028F3N7Q8ZK2V5X1C9D4HT
Webhook-Signature: sha256=4c0f6a3e9b1d7f25c8e04a6b3d9f1e72a5c8b0d4e6f3a1c7b9d2e5f8a0c3b6d1
```
```json
{
    "event": "item_completed",
    "accessCode": "bdbf9f26ee4ffc8e",
    "completedAt": "2025-11-26T13:29:55+01:00",
    "item": {
        "id": "01K8AP5798K44N895D0258EDM8",
        "title": "Deeltoets 2.2"
    },
    "passed": true,
    "mappedScore": 8,
    "achievedScore": 8
}
```
