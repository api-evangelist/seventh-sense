---
generated: '2026-07-21'
method: generated
name: Verify a person with a liveness check
description: Run an anti-spoofing liveness check on a captured face image, then verify or search it against registered persons.
api: openapi/seventh-sense-opencv-fr-openapi.json
operations: [check_liveness_for_a_single_image_liveness_post, search_and_check_liveness_from_one_image_of_same_person_search_live_face_post, verify_match_from_one_or_more_images_of_same_person_verify_post]
source: >-
  operationIds verified in openapi/seventh-sense-opencv-fr-openapi.json.
---

# Verify a person with a liveness check

Confirm a live human (not a spoof) and then confirm their identity.

## Auth
- Header `x-api-key: <DEVELOPER_KEY>`. See `authentication/seventh-sense-authentication.yml`.
- Regional base URL (us/sg/eu.opencv.fr). See `conventions/seventh-sense-conventions.yml`.

## Steps
1. **Liveness check** — `check_liveness_for_a_single_image_liveness_post` (`POST /liveness`) with the captured face image. A score >= 0.5 is considered live (iBeta Level 2, ISO 30107-3). Meet the image requirements: one fully-visible face, min 224x224 face box, >=25px border padding, >=80px inter-pupil distance, pitch/yaw within +/-30 degrees.
2. **Verify or search** — either:
   - `verify_match_from_one_or_more_images_of_same_person_verify_post` (`POST /verify`) to confirm the image matches a specific known `person_id`, or
   - `search_and_check_liveness_from_one_image_of_same_person_search_live_face_post` (`POST /search-live-face`) to run liveness AND search a collection in one call.

## Notes
- Reject the flow if the liveness score is below 0.5 before trusting any match.
- These endpoints are stateless — they store nothing unless you register the person via `/person`.
- Validation errors return HTTP 422 `{ "detail": [...] }`. See `errors/seventh-sense-problem-types.yml`.
