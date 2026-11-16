# FLUX 3 Video API production notes

## Confirmed contract

- Endpoint: `POST /api/generate/submit`
- Model IDs: `flux-3/text-to-video`, `flux-3/image-to-video`, `flux-3/first-last-frame-to-video`, `flux-3/extend-video`, `flux-3/keyframes-to-video`
- Authentication: `Authorization: Bearer <POYO_API_KEY>`

## Input limits

| Field | Requirement | Constraint |
| --- | --- | --- |
| prompt | required | Video description. |
| duration | optional | Integer from 5 to 20 seconds; default 5. |
| resolution | optional | 720p or 1080p; default 720p. |
| aspect_ratio | optional | auto, 21:9, 2:1, 16:9, 4:3, 1:1, 3:4, or 9:16. |
| sound | optional | Boolean; defaults to true. |
| image_urls | workflow-specific | Exactly 1 for image-to-video or 2 for first/last frame. |
| video_url | extend only | HTTPS MP4 under 50 MB and shorter than 15 seconds. |
| keyframes | keyframes only | 1-10 unique frame-index/image pairs at 24 fps. |

## Operational checklist

- Keep API keys in a server-side secret manager.
- Validate user input before calling the API.
- Persist `data.task_id` immediately after submission.
- Use five-second polling only for development; prefer `callback_url` in production.
- Make webhook processing idempotent and reconcile status before fulfilling an order.
- Copy output files to durable storage before provider URLs expire.
- Recheck the live model page before hard-coding prices.

## Failure handling

Treat non-2xx HTTP responses and non-success API `code` values as errors. Preserve the HTTP status, API code, request ID when present, and a redacted response body for debugging.

Schema: [PoYo API documentation](https://docs.poyo.ai/api-manual/video-series/flux-3)

