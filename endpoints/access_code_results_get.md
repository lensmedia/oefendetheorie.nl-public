### `GET /access-code/{accessCode}/results`
Used to get the study results of an access code.

#### Request   
##### Parameters
| Name         | Type   | Description                                                                                  |
|--------------|--------|----------------------------------------------------------------------------------------------|
| `accessCode` | string | The access code to get the results from, the 16 character long hex string they use to login. |

##### Example 
Get the results from access code `bdbf9f26ee4ffc8e`
```
GET /api/access-code/bdbf9f26ee4ffc8e/results HTTP/1.1
Host: https://test.oefendetheorie.nl
Authorization: Basic F00B4R=
Content-Type: application/json
```

#### Response
Returns an array with all results from access code.

**Note:** This is a local machine response, access code and output may differ on test and live servers. 
```json
[
    {
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
    },
    {
        "createdAt": "2025-11-26T11:04:55+01:00",
        "context": {
            "passed": false,
            "session": "01KAZSYW1DT86VV6KJT0MCRY7M",
            "mapped_score": 10,
            "category_item": {
                "id": "01K8AP5201WBMKY7V31VVPBN2X",
                "item": {
                    "id": "245de5b3-67ef-4077-a39c-faaddae1ea91",
                    "type": "exercise",
                    "title": "Deeltoets 1.2",
                    "modifiedAt": "2025-08-07T13:45:12+02:00",
                    "originalId": 918
                },
                "slug": "1-2",
                "title": "Deeltoets 1.2",
                "category": {
                    "id": "01K8AP4ZGBNV7ZB21Y9WEQDZQQ",
                    "slug": "opdrachten",
                    "title": "Opdrachten",
                    "description": null
                }
            },
            "subject_count": {
                "BE1": 10
            },
            "achieved_score": 2,
            "subject_errors": {
                "BE1": 8
            }
        }
    },
    {
        "createdAt": "2024-03-12T19:41:07+01:00",
        "context": {
            "exam": 3,
            "passed": false,
            "mapped_score": 65,
            "achieved_score": 51,
            "subject_errors": {
                "2": 5,
                "4": 3,
                "7": 6
            }
        }
    }
]
```

#### Results
There are two kinds of results, told apart by their `context`.

##### Item results
Results of items completed on oefendetheorie.nl have a `category_item`.

| Name             | Type    | Description                                                         |
|------------------|---------|---------------------------------------------------------------------|
| `passed`         | boolean | Whether the item was passed.                                        |
| `session`        | Ulid    | The session the item was completed in.                              |
| `category_item`  | object  | The item that was completed.                                        |
| `mapped_score`   | integer | The number of questions.                                            |
| `achieved_score` | integer | The number of correct answers.                                      |
| `subject_count`  | object  | The number of questions per subject.                                |
| `subject_errors` | object  | The number of mistakes per subject, only subjects with mistakes.    |

##### Theorieboek.nl exam results
Exam results from theorieboek.nl have an `exam` instead of a `category_item`.

| Name             | Type    | Description                                                                                                          |
|------------------|---------|----------------------------------------------------------------------------------------------------------------------|
| `exam`           | integer | The number of the exam.                                                                                              |
| `passed`         | boolean | Whether the exam was passed.                                                                                         |
| `mapped_score`   | integer | The number of questions.                                                                                             |
| `achieved_score` | integer | The number of correct answers.                                                                                       |
| `subject_errors` | object  | The number of mistakes, keyed by subject letter (`{"A": 2}`) or chapter number (`{"1": 2}`). Only those with mistakes. |
