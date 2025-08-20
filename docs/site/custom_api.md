# API Documentation

## Authentication

### `POST /api/token/`

#### Request Payload

```json
{
  "username": "<username>",
  "password": "<password>"
}
```

#### Object Response

```json
{
  "access": "<access_token>",
  "refresh": "<refresh_token>"
}
```

Use the `access_token` in the `Authorization` header for authenticated requests:

```http
Authorization: Bearer <access_token>
```

### `POST /api/token/refresh/`

#### Request Payload

```json
{
  "refresh": "<refresh_token>"
}
```

#### Object Response

```json
{
  "access": "<access_token>",
  "refresh": "<refresh_token>"
}
```

# Problem Management

## Problem List View

### `POST /api/v2/problems`

#### Request Payload

```json
{
  "code": "newproblem",
  "name": "Sample Problem",
  "description": "Solve this simple problem.",
  "authors": [1], // Optional
  "curators": [], // Optional
  "testers": [], // Optional
  "types": [], // Optional, should be required after update
  "group": 1,
  "time_limit": 2.0,
  "memory_limit": 262144,
  "short_circuit": false, // Optional, default false
  "points": 100,
  "partial": true, // Optional, default false
  "allowed_languages": [1, 2], // Optional
  "is_public": false, // Optional, default true, create private problems permission required
  "is_manually_managed": false, // Optional, default false
  "date": "2025-03-29T12:00:00Z", // Optional
  "og_image": "", // Optional
  "summary": "A test problem for the API.", // Optional
  "user_count": 0, // Optional, system-generated
  "ac_rate": 0, // Optional, system-generated
  "is_full_markup": false, // Optional, default false
  "submission_source_visibility_mode": "F", // 4 modes: F - Follow global setting, A - Always visible, S - Visible if problem solved, O - Only own submissions
  "license": null // Optional
}
```

#### Object Response

**201 Created**

```json
{
  "id": 8,
  "authors": [1],
  "curators": [],
  "testers": [],
  "types": [],
  "group": 1,
  "allowed_languages": [2, 1],
  "organizations": [],
  "license": null,
  "is_public": false,
  "code": "newproblem",
  "name": "Sample Problem",
  "description": "Solve this simple problem.",
  "time_limit": 2.0,
  "memory_limit": 262144,
  "short_circuit": false,
  "points": 100.0,
  "partial": true,
  "is_manually_managed": false,
  "date": "2025-03-29T08:00:00-04:00",
  "og_image": "",
  "summary": "A test problem for the API.",
  "user_count": 0,
  "ac_rate": 0.0,
  "is_full_markup": false,
  "submission_source_visibility_mode": "F",
  "is_organization_private": false,
  "banned_users": []
}
```

## Problem Detail View

### `PUT /api/v2/problem/<problem code>`

#### Info

- Follow Question POST Format
- Need Token

#### Object response

**200 OK**

- Changed data similiar to a GET

```json
{
  "code": "<problem code>",
  "name": "<problem name>",
  "authors": ["<list of author id>"],
  "types": ["<list of type id>"],
  "group": "<problem group id>",
  "time_limit": "<problem time limit>",
  "memory_limit": "<problem memory limit>",
  "language_resource_limits": [
    {
      "language": "<language key>",
      "time_limit": "<language-specific time limit>",
      "memory_limit": "<language-specific memory limit>"
    }
  ],
  "points": "<problem points>",
  "partial": "<whether partials are enabled for this problem>",
  "short_circuit": "<whether short circuit is enabled for this problem>",
  "languages": ["<list of language key>"],
  "is_organization_private": "<whether the problem is private to organizations>",
  "organizations": ["<list of organization id>"],
  "is_public": "<whether the problem is publicly visible>"
}
```

### `DELETE /api/v2/problem/<problem code>`

- Need Token
- No Payload

#### Object response

**204 No Content**

```json
{
  "detail": "Problem deleted successfully."
}
```
## Problem Data
Problem data contains the test cases for a problem

### `GET /api/v2/problem/<problem code>/test_data`

- Need Token of a user that is either the authour or the curator of this problem

#### Object response

**200 OK**
```json
{
  "zipfile_exists": true,
  "data": {
    "problem_data": {
      "zipfile": "<zipfile download url, you need token for this>",
      "generator": "<path to generator or null>",
      "output_prefix": "<output filename prefix or null>",
      "output_limit": "<limit on output size or null>",
      "feedback": "<feedback type or empty string>",
      "checker": "<checker type, e.g., 'standard'>",
      "unicode": "<true or false>",
      "nobigmath": "<true or false>",
      "checker_args": "<arguments to the checker or empty string>"
    },
    "test_cases": [
      {
        "id": "<test case ID>",
        "input_file": "<input filename>",
        "output_file": "<output filename>",
        "type": "<type of test case 'C', 'S', 'E'>",
        "order": "<test case order>",
        "points": "<points>",
        "is_pretest": "<true or false>"
      }
    ]
  }
}
```
#### Ad
### `PUT /api/v2/problem/<problem code>/test_data`

- Need Token of a user that is either the authour or the curator of this problem

Headers:
- Authorization: Bearer <your_token>
- Content-Type: multipart/form-data

#### Request Payload
| Field                        | Type    | Description                                          |
| ---------------------------- | ------- | ---------------------------------------------------- |
| `problem_data.zipfile`       | File    | Zip file containing the problem data                 |
| `problem_data.generator`     | File    | (Optional) Generator file                            |
| `problem_data.output_prefix` | String  | (Optional) Prefix for output files                   |
| `problem_data.output_limit`  | Integer | (Optional) Output limit in bytes                     |
| `problem_data.checker`       | Enum    | `'C'`, `'S'`, or `'E'`                               |
| `problem_data.checker_args`  | String  | (Optional) Arguments passed to the checker           |
| `problem_data.unicode`       | Boolean | (Optional) `true` or `false`, default `false`        |
| `problem_data.nobigmath`     | Boolean | (Optional) `true` or `false`, default `false`        |
| `test_cases[0].input_file`   | File    | Input file for the test case                         |
| `test_cases[0].output_file`  | File    | Output file for the test case                        |
| `test_cases[0].order`        | Integer | Order of execution (must be unique among test cases) |
| `test_cases[0].points`       | Integer | Number of points awarded for this test case          |
| `test_cases[1].input_file`   | File    | ... (repeatable for more test cases)                 |

#### Object Response

**200 OK**

```json
{
    "detail": "Problem data saved successfully"
}
```
#### Aditional Note
for more info: 
- [checker](../problem_format/custom_checkers)
- [generator](../problem_format/generator)
- [problem format](../problem_format/problem_format.md)


### `POST /api/v2/problem/<Problem Code>/submit`
#### Request Payload
```json
{
    "source": "Your code",
    "language": "<a number>",
    "judge": "Test1" // judge code, you can leave this empty and it will be any judge
}
```
#### Object Response
```json
{
  "submission_id": "<your submission id>"
}
```

#### Additional Notes
- You can use your `submission_id` with this API [get submission with id](site/api?id=apiv2submissionltsubmission-idgt)

# User Management
## User
### `POST /api/v2/moodle-to-dmoj/`

- Require an admin token

#### Request Payload
```json
{
    "provider": "moodle", // this one is permanently moodle for now
    "id": ["<List of ids>"]
}
```
#### Object Response
```json
{
  "<Moodle id>": {
    "profile_id": "<profile id on dmoj side>",
    "user_id": "<user id on dmoj side, matched through UserSocialAuth table>"
  }
}
```
#### Additional detail
- Most of API Requests with things like user id actually require `profile_id` instead

### `POST /api/v2/users/create`

- Need admin token

#### Request Payload
```json
{
    "<moodle_id>": {
        "username": "",
        "email": "",
        "first_name": ""
    },
    "<moodle_id>": {
        "username": "<username>",
        "email": "fullfake@example.com",
        "password": "Secret123!",
        "first_name": "first name",
        "last_name": "last name"
    }
}
```
#### Object Response
```json
{
  "success":{
    "<moodle_id>": {
      "dmoj_uid": "<dmoj_uid>",
      "dmoj_profile_uid": "<dmoj profile_uid>",
      "username": "<dmoj user_name>",
      "email": "<dmoj email>"  
    }
  },
  "errors":{
    "<moodle_id>": {
    }
  }
}
```
## User Submission Data
### `POST /api/v2/user/download-data`

- Need a token, the api will get data according to the user of the token

#### Request Payload
```json
{
  "comment_download": true,
  "submission_download": true,
  "submission_problem_glob": "*",
  "submission_results": ["AC"]
}
```

#### Object Response
```json
{
    "status": "started",
    "progress_url": "<absoulute url that display progress for preparing data>"
}
```

### `GET /api/v2/user/download-data`

- Need token

#### Object Response
```json
{
    "status": "<ready or not>",
    "download_url": "<absolute url for zipfile>"
}
```

#### Additional info
- You need token for the `download_url` for the correct data

# Organization Management
## Organization List
### `POST /api/v2/organizations`

- Need Token with adequate permission

#### Request Payload
```json
{
  "name": "Sample Couse",
  "slug": "sample",
  "short_name": "SAM",
  "about": "A sample org for testing.",
  // "admins": ["<users' profile id>"], ignore unless you have organzation_admin permission, default will be token user
  // "members": ["<users' profile id>"], // ignore unless you have organzation_admin permission, default will be token user
  // "is_open": true, // ignore unless you have organzation_admin permission, default true
  // "slots": 100, // ignore unless you have organzation_admin permission, default null
  "access_code": "ABC1234"
  // ,"logo_override_image": ""
}
```
#### Object Response
**200 OK**
```json
{
    "id": 84,
    "name": "Sample Couse",
    "slug": "sample",
    "short_name": "SAM",
    "about": "A sample org for testing.",
    "admins": [
        1
    ],
    "members": [],
    "creation_date": "2025-07-13T09:02:00.301399+07:00",
    "is_open": true,
    "slots": null,
    "access_code": "ABC1234",
    "logo_override_image": ""
}
```

## Organization Detail
### `GET /api/v2/organization/<OrganizationID>`
- Need Token of author or Curator
- It has the same Response as [Get Organization List](site/api.md#apiv2organizations) but only list the organization with this ID

### `PUT /api/v2/organization/<OrganizationID>`
- It has the same request payload and Response as [Create Organization](#post-apiv2organizations)

### `DELETE /api/v2/organization/<OrganizationID>`
- Need token of author or curator

#### Object Response
**204 No Content**