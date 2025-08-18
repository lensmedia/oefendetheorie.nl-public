### `GET /course`
Used to get all courses available. Mainly used when submitting an [POST /access-code](access_code_post.md) request for the ID.

#### Request
###### Example
```
GET /api/course HTTP/1.1
Host: https://test.oefendetheorie.nl
Authorization: Basic F00B4R=
```

#### Response
* **Note;** this is a simplified example, the actual response will contain more courses/items.
* **Note;** using the course href and optionally the slug for category and an optionally item you can construct a URL to the specific course/category/item as entry point for the end-user. These should remain quite stable as they are also used in print.
```
[
    ...,

    {
        "id": "01K22CWRGEFNWRP7ARHZF2CWDC",
        "title": "Personenauto",
        "description": null,
        "country": "BE",
        "locale": "nl_BE",
        "slug": "auto",
        "href": "https://test.oefendetheorie.nl/be/auto",
        "publishedAt": "2025-08-07T15:52:54+02:00",
        "createdAt": "2025-08-07T15:52:54+02:00",
        "updatedAt": "2025-08-07T15:52:54+02:00",
        "curriculum": {
            "licence": "b"
        },
        "categories": [
            {
                "id": "01K22CWRGK2F9TG421WST9P7JD",
                "title": "Opdrachten",
                "description": null,
                "slug": "opdrachten",
                "items": [
                    {
                        "id": "01K22CWTV21M4PQP29N4FP8EJ3",
                        "slug": "1-1",
                        "title": "Deeltoets 8.4"
                    },

                    ...
                ]
            },
            {
                "id": "01K22CXDW0B5KF1B74W86TD08M",
                "title": "Examens",
                "description": null,
                "slug": "theorie-examens",
                "items": [
                    {
                        "id": "01K22CXKXAWPWMDQ6VKGCHT31N",
                        "slug": "1",
                        "title": "Examen 39"
                    },
                    {
                        "id": "01K22CYAC8MDKFFE5KPHB868MJ",
                        "slug": "2",
                        "title": "Examen 6"
                    },

                    ...
                ]
            }
        ]
    },

    ...
]
```

