# API Testing — reqres.in Sample Suite

Collection file: [reqres-api-tests.postman_collection.json](./reqres-api-tests.postman_collection.json)
Target API: [reqres.in](https://reqres.in) — a public REST API sandbox intended for testing/practice.

> Note: reqres.in's exact auth requirements and response shapes can change over time — verify current behavior against their docs before treating these as a live regression suite. Included here to demonstrate test design and assertion approach.

## Coverage

| Method | Endpoint | Scenario | Key Assertions |
|---|---|---|---|
| GET | `/api/users?page=2` | List users (positive) | Status 200, `data` is a non-empty array, each user has `id`/`email`/`first_name` |
| POST | `/api/users` | Create user (positive) | Status 201, response echoes submitted `name`/`job`, includes generated `id` and `createdAt` |
| PUT | `/api/users/2` | Update user (positive) | Status 200, updated field reflected, includes `updatedAt` |
| DELETE | `/api/users/2` | Delete user (positive) | Status 204, no content in body |
| GET | `/api/users/23` | Non-existent user (negative) | Status 404, empty response body |

## Why this set

A small but deliberate mix of **positive and negative cases** across the main CRUD operations — this is the same shape I'd use to sanity-check a REST backend before diving into deeper edge cases (invalid payloads, missing auth, pagination boundaries, etc.).

## How I'd extend this for a real project

- Add schema validation (e.g. with `ajv` or Postman's built-in schema test) instead of just checking individual fields
- Add negative tests for malformed request bodies (missing required fields, wrong types)
- Chain requests using Postman variables (e.g. capture the `id` from POST and reuse it in the PUT/DELETE calls)
- Run the collection via Newman in a CI pipeline for automated regression on every build
