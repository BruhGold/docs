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

#### Response

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

#### Response

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

#### Response

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

### GET /api/v2/problems

Example: [/api/v2/problems?partial=True&type=Uncategorized](https://dmoj.ca/api/v2/problems?partial=True&type=Uncategorized)

#### Basic filters

- `partial` - boolean

#### List filters

- `code` - problem code
- `group` - problem group full name
- `type` - problem type full name
- `organization` - organization id

#### Additional filters

- `search` - similar to a list filter, except searches for the list of parameters in the problem's name, code, and description.

#### Object response

```json
{
  "code": "<problem code>",
  "name": "<problem name>",
  "types": ["<list of type full name>"],
  "group": "<problem group full name>",
  "points": "<problem points>",
  "partial": "<whether partials are enabled for this problem>",
  "is_organization_private": "<whether the problem is private to organizations>",
  "is_public": "<whether the problem is publicly visible>"
}
```

## Problem Detail View

### GET /api/v2/problem/<problem code>

Example: [/api/v2/problem/helloworld](https://dmoj.ca/api/v2/problem/helloworld)

#### Object response

```json
{
  "code": "<problem code>",
  "name": "<problem name>",
  "authors": ["<list of author username>"],
  "types": ["<list of type full name>"],
  "group": "<problem group full name>",
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

#### Additional info

`is_public`: Whether the problem is publicly visible to the organizations listed. If `is_organization_private` is `false`, the problem is visible to all users.

### PUT /api/v2/problem/<problem code>

#### Info

- Follow Question POST Format
- Need Token

#### Object response

**200 OK**

- Change data similiar to a GET

### DELETE /api/v2/problem/<problem code>

- Need Token
- No Payload

#### Object response

**204 No Content**

```json
{
  "detail": "Problem deleted successfully."
}
```
