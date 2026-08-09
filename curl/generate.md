# FLUX 3 Video API: cURL quickstart

Submit an async video task with model `flux-3/text-to-video`:

```bash
export POYO_API_KEY="your-api-key"
export POYO_BASE_URL="https://api.poyo.ai"

curl -X POST "$POYO_BASE_URL/api/generate/submit" \
  -H "Authorization: Bearer $POYO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
  "model": "flux-3/text-to-video",
  "input": {
    "prompt": "A clean macro product shot of a glass perfume bottle rotating slowly on black stone, crisp reflections, premium studio lighting",
    "duration": 5,
    "resolution": "720p",
    "aspect_ratio": "16:9",
    "sound": true
  }
}'
```

Save `data.task_id`, then check status:

```bash
curl "$POYO_BASE_URL/api/generate/status/<task_id>" \
  -H "Authorization: Bearer $POYO_API_KEY"
```

For production, add a public HTTPS `callback_url` to the top-level request and make the receiver idempotent.

See the [full README](../README.md) and [production notes](../docs/production-notes.md).

