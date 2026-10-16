# Seedance 1.0 Pro Quality API (`doubao-seedance-1-0-pro-quality`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-ad-ceramic-mug-thumb.jpg" width="820" alt="Sample video frame generated through APIMart">
</p>

> **from $0.0184 per second** on the APIMart route (billed by resolution tier) — one OpenAI-compatible endpoint at `https://api.apimart.ai/v1`, no monthly plan required. *(observed 2026-10-12)*

**[Get an API key](https://go.apimart.ai/k-ef60c4)** · **[Live pricing](https://go.apimart.ai/k-64f948)** · **[Model page](https://go.apimart.ai/k-580978)** · [⚡ 60-second quickstart](#quickstart)

**Why teams call Seedance 1.0 Pro Quality (`doubao-seedance-1-0-pro-quality`) through APIMart**

- **One key, entire catalog.** The same `https://api.apimart.ai/v1` base URL and `Authorization` header reach Seedance 1.0 Pro Quality (`doubao-seedance-1-0-pro-quality`) and 300+ other image, video and language models — switch the `model` field, not your client.
- **$1 minimum, pay as you go.** No subscription and no prepaid plan to size up front: top up from $1 and spend it on calls. There is no free quota to burn through first, so the price in this table is the price you pay.
- **The charge comes back in the response.** Every call reports the amount billed (`cost` / `credits_cost`), so a spend number is read per call instead of guessed at month end.
- **Async by design.** Submit, take the `task_id`, poll `GET /v1/tasks/{id}` — batching and retries are ordinary queue work, not a bespoke integration.

<!-- /conv-kit:v1 -->



Seedance 1.0 Pro Quality is ByteDance's higher-fidelity video route on APIMart: text-to-video and image-to-video in 480p / 720p / 1080p, billed per generated second.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Text-to-video / image-to-video | `doubao-seedance-1-0-pro-quality` | per generated second, by resolution | see the [model page](https://go.apimart.ai/k-580978) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/videos/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Resolution | List price | Effective price |
| --- | --- | --- |
| per generated second | — | see the live pricing page |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-64f948) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

## Request parameters

| Field | Type | Default | Notes |
| --- | --- | --- | --- |
| `model` | string | required | the model id shown in the route table above |
| `prompt` | string | required | the shot description; camera and subject motion belong here |
| `duration` | integer | route default | clip length in seconds — billing is per generated second |
| `resolution` | string | route default | 480P / 720P / 1080P or the route’s own tiers |
| `size` / `aspect_ratio` | string | `16:9` | output framing |
| `image_urls` | string[] | — | first frame, first + last frame, or style references, where supported |
| `seed` | integer | — | fix the seed to reproduce a take |

The table lists the fields accepted by the APIMart `videos/generations` endpoint. Check the [model page](https://go.apimart.ai/k-580978) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/videos/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"doubao-seedance-1-0-pro-quality","prompt":"A bamboo forest at sunrise, slow push-in","duration":5,"resolution":"720P","size":"16:9"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/videos/generations", headers=HEADERS, timeout=60, json={
    "model": "doubao-seedance-1-0-pro-quality", "prompt": "A bamboo forest at sunrise, slow push-in", "duration": 5,
}).json()
task_id = created["data"]["id"]

while True:
    task = requests.get(f"{BASE}/tasks/{task_id}", headers=HEADERS, timeout=60).json()["data"]
    if task["status"] in ("completed", "failed"):
        break
    time.sleep(5)
print(task.get("cost"), task["result"]["videos"][0]["url"])
```

Runnable versions: [`examples/`](examples). The task lifecycle is `pending → processing → completed | failed`.

## FAQ

**What is the Seedance 1.0 Pro Quality model id?**

`doubao-seedance-1-0-pro-quality`. See the route table above.

**How is it billed?**

**from $0.0184 per second** on the APIMart route (billed by resolution tier). The completed task reports the exact amount in its `cost` field; a failed task is refunded.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-ef60c4) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, other languages** — 9 more language builds: 中文 [seedance-1-0-pro-quality-api-cn](https://github.com/lo0-trmv3dup2/seedance-1-0-pro-quality-api-cn) · Deutsch [seedance-1-0-pro-quality-api-de](https://github.com/qt89oz47ugft9i/seedance-1-0-pro-quality-api-de) · Español [seedance-1-0-pro-quality-api-es](https://github.com/g708boud1/seedance-1-0-pro-quality-api-es) · Français [seedance-1-0-pro-quality-api-fr](https://github.com/fdu-6/seedance-1-0-pro-quality-api-fr) · Bahasa [seedance-1-0-pro-quality-api-id](https://github.com/i74lkq1472/seedance-1-0-pro-quality-api-id) · 日本語 [seedance-1-0-pro-quality-api-ja](https://github.com/fkqozxa1p/seedance-1-0-pro-quality-api-ja) · 한국어 [seedance-1-0-pro-quality-api-ko](https://github.com/t26b6qupjc93k7/seedance-1-0-pro-quality-api-ko) · Português [seedance-1-0-pro-quality-api-pt](https://github.com/onpfo05pvnb/seedance-1-0-pro-quality-api-pt) · Русский [seedance-1-0-pro-quality-api-ru](https://github.com/rb5mqsn/seedance-1-0-pro-quality-api-ru)
- **More models in this language** — [midjourney-api-gateway](https://github.com/apimartxc3iskw/midjourney-api-gateway) · [gemini-2-5-flash-image-preview-api-gateway](https://github.com/mcqdlebg/gemini-2-5-flash-image-preview-api-gateway) · [nano-banana-api-gateway](https://github.com/gix2kust/nano-banana-api-gateway) · [gemini-3-1-flash-image-preview-api-gateway](https://github.com/nn6jtthao8/gemini-3-1-flash-image-preview-api-gateway) · [nano-banana-2-api-gateway](https://github.com/u9mdw3pt594q2r/nano-banana-2-api-gateway) · [gemini-3-pro-image-preview-api-gateway](https://github.com/uqu9q/gemini-3-pro-image-preview-api-gateway)
- **Topic hubs** — every build of this model, all languages: [seedance-1-0-pro-quality](https://github.com/topics/seedance-1-0-pro-quality) · [seedance-1-0-pro-quality-api](https://github.com/topics/seedance-1-0-pro-quality-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-580978> | `apimart.ai/model/doubao-seedance-1-0-pro` |
| Live pricing | <https://go.apimart.ai/k-64f948> | `apimart.ai/pricing` |
| Get an API key | <https://go.apimart.ai/k-ef60c4> | `apimart.ai/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Seedance 1.0 Pro Quality is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
