---
generated: '2026-07-21'
method: generated
name: Register persons and search by face
description: Create a collection, register one or more persons with face images, then search the collection by a face image.
api: openapi/seventh-sense-opencv-fr-openapi.json
operations: [create_collection_collection_post, create_person_person_post, search_from_one_or_more_images_of_same_person_search_post]
source: >-
  operationIds verified in openapi/seventh-sense-opencv-fr-openapi.json.
---

# Register persons and search by face

Build a searchable face registry with OpenCV Face Recognition, then match a probe image against it.

## Auth
- Header `x-api-key: <DEVELOPER_KEY>` on every request. Get a key at https://developer.opencv.fr. See `authentication/seventh-sense-authentication.yml`.
- Pick a regional base URL: `https://us.opencv.fr`, `https://sg.opencv.fr`, or `https://eu.opencv.fr`. See `conventions/seventh-sense-conventions.yml`.

## Steps
1. **Create a collection** — `create_collection_collection_post` (`POST /collection`) to make a named group that scopes your registry and searches. Capture the returned `collection_id`.
2. **Register a person** — `create_person_person_post` (`POST /person`) with the person's name and one or more base64 face images, associated to the `collection_id`. Repeat for each person. Capture each `person_id`.
3. **Search by face** — `search_from_one_or_more_images_of_same_person_search_post` (`POST /search`) with a probe image (and optionally the `collection_id`, a `min_score` default 0.81, and `max_results` up to 100). Results come back ranked by descending similarity score (0..1).

## Notes
- `/search` stores nothing; only registered persons (from `/person`) persist. A score >= 0.81 is a strong match.
- List/paginate persons with `get_persons_persons_get` (`GET /persons`) using `skip`/`take`/`order_by`/`search`.
- Validation failures return HTTP 422 `{ "detail": [...] }`. See `errors/seventh-sense-problem-types.yml`.
