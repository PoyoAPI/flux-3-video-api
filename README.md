# FLUX 3 Video API: runnable cURL and Node.js examples

[![PoYo model page](https://img.shields.io/badge/Try%20on%20PoYo-flux--3--video--api-84cc16)](https://poyo.ai/models/flux-3) [![API docs](https://img.shields.io/badge/API-docs-22d3ee)](https://docs.poyo.ai/api-manual/video-series/flux-3) [![Check](https://github.com/PoyoAPI/flux-3-video-api/actions/workflows/check.yml/badge.svg)](https://github.com/PoyoAPI/flux-3-video-api/actions/workflows/check.yml)

Generate, extend, and control FLUX 3 videos through five explicit PoYo model IDs and one async task workflow.

**Answer:** To use the FLUX 3 Video API, send a server-side request to `POST https://api.poyo.ai/api/generate/submit` with model `flux-3/text-to-video`, `flux-3/image-to-video`, `flux-3/first-last-frame-to-video`, `flux-3/extend-video`, `flux-3/keyframes-to-video` and a PoYo API key. The examples in this repository are runnable with Node.js 22+ or cURL.

Last verified against the live PoYo schema and pricing on **November 16, 2026**.

## Quickstart

1. Create a key in the [PoYo dashboard](https://poyo.ai/dashboard/api-key).
2. Copy `.env.example` to `.env` and replace the placeholder.
3. Run `node node/index.mjs`.

```dotenv
POYO_API_KEY=YOUR_POYO_API_KEY_HERE
POYO_BASE_URL=https://api.poyo.ai
```

The complete cURL request is in [curl/generate.md](curl/generate.md). The Node.js example submits the task and polls until `finished` or `failed`.

## Model IDs

- `flux-3/text-to-video`
- `flux-3/image-to-video`
- `flux-3/first-last-frame-to-video`
- `flux-3/extend-video`
- `flux-3/keyframes-to-video`

Use the exact IDs above. Product names and API model IDs are not always identical.

## Request fields

| Field | Requirement | Meaning |
| --- | --- | --- |
| prompt | required | Video description. |
| duration | optional | Integer from 5 to 20 seconds; default 5. |
| resolution | optional | 720p or 1080p; default 720p. |
| aspect_ratio | optional | auto, 21:9, 2:1, 16:9, 4:3, 1:1, 3:4, or 9:16. |
| sound | optional | Boolean; defaults to true. |
| image_urls | workflow-specific | Exactly 1 for image-to-video or 2 for first/last frame. |
| video_url | extend only | HTTPS MP4 under 50 MB and shorter than 15 seconds. |
| keyframes | keyframes only | 1-10 unique frame-index/image pairs at 24 fps. |

## Pricing snapshot

| Usage | Price |
| --- | --- |
| Standard workflows, 720p | $0.17 per second |
| Standard workflows, 1080p | $0.29 per second |
| Extend video, 720p | $0.41 per second |
| Extend video, 1080p | $0.53 per second |

Pricing can change. Confirm the current amount on the [model page](https://poyo.ai/models/flux-3) before a production rollout.

## Best for

- text-to-video
- image animation
- first/last-frame transitions
- video extension
- keyframe-directed video

## Important behavior

- Choose the model ID that matches the workflow; inputs are validated per workflow.
- Keyframe frame_index values must be unique and fit the duration x 24 fps timeline.
- Extend-video inputs must be HTTPS MP4 files under the documented size and duration limits.

## Async production flow

```text
submit -> persist data.task_id -> poll during development -> receive callback_url webhook in production
```

Treat `finished` and `failed` as terminal states. Accept a webhook only for task IDs your system created, process it idempotently, and reconcile the final state through the status endpoint.

## Repository map

- [`curl/generate.md`](curl/generate.md) — copy-paste request
- [`node/index.mjs`](node/index.mjs) — dependency-free Node.js example
- [`docs/production-notes.md`](docs/production-notes.md) — limits and rollout checklist

## Links

- [Try FLUX 3 Video API on PoYo](https://poyo.ai/models/flux-3)
- [API reference](https://docs.poyo.ai/api-manual/video-series/flux-3)
- [All PoYo examples](https://github.com/PoyoAPI/poyo-examples)
- [PoYo model catalog](https://poyo.ai/models)

## License

MIT

