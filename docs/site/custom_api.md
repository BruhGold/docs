# API Documentation

## Authentication

### `POST /api/token`

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

### `POST /api/token/refresh`

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

## Problem Management

### `POST /api/v2/problems/create`

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
