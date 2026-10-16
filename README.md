# Seedance 1.0 Pro Quality API (`doubao-seedance-1-0-pro-quality`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-ad-ceramic-mug-thumb.jpg" width="820" alt="Sample video frame generated through APIMart">
</p>

> **from $0.0184 per second** on the APIMart route (billed by resolution tier) — `https://api.apimart.ai/v1` の OpenAI 互換エンドポイント1つだけ、月額プランは不要です。*(2026-10-12 時点)*

**[APIキー](https://go.apimart.ai/k-669595)** · **[料金](https://go.apimart.ai/k-756d46)** · **[モデルページ](https://go.apimart.ai/k-4a8e7a)** · [⚡ 60秒クイックスタート](#quickstart)

**チームが APIMart 経由で Seedance 1.0 Pro Quality（`doubao-seedance-1-0-pro-quality`）を使う理由**

- **キー1つでカタログ全体。**同じ `https://api.apimart.ai/v1` の base URL と `Authorization` ヘッダーで Seedance 1.0 Pro Quality（`doubao-seedance-1-0-pro-quality`）と画像・動画・言語モデルなど 300+ のモデルにアクセスできます。切り替えるのは `model` フィールドだけで、クライアント側の変更は不要です。
- **最低 $1 から、従量課金。**サブスクリプションも、事前に規模を見積もるプリペイドプランもありません。$1 からチャージして、呼び出した分だけ使えます。
- **課金内容はレスポンスで返ります。**完了したタスクはすべて請求額（`cost` / `credits_cost`）を報告し、`failed` になったタスクは返金されます。
- **非同期前提の設計。**送信して `task_id` を受け取り、`GET /v1/tasks/{id}` をポーリングするだけ。バッチ処理もリトライも特別な統合ではなく、普通のキュー処理です。

<!-- /conv-kit:v1 -->

**別名:** シードダンス · シーダンス

Seedance 1.0 Pro Quality is ByteDance's higher-fidelity video route on APIMart: text-to-video and image-to-video in 480p / 720p / 1080p, billed per generated second.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Text-to-video / image-to-video | `doubao-seedance-1-0-pro-quality` | per generated second, by resolution | see the [model page](https://go.apimart.ai/k-4a8e7a) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/videos/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Resolution | List price | Effective price |
| --- | --- | --- |
| per generated second | — | see the live pricing page |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-756d46) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

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

The table lists the fields accepted by the APIMart `videos/generations` endpoint. Check the [model page](https://go.apimart.ai/k-4a8e7a) for anything this model adds on top.

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

**Seedance 1.0 Pro Quality のモデル id は何ですか？**

`doubao-seedance-1-0-pro-quality` です。上のルート表を参照してください。

**課金方法は？**

**from $0.0184 per second** on the APIMart route (billed by resolution tier)。完了したタスクは `cost` フィールドに正確な金額を報告し、失敗したタスクは返金されます。

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-669595) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [seedance-1-0-pro-quality-prompts-ja](https://github.com/fkqozxa1p/seedance-1-0-pro-quality-prompts-ja)
- **Same model, other languages** — 9 more language builds: 中文 [seedance-1-0-pro-quality-api-cn](https://github.com/lo0-trmv3dup2/seedance-1-0-pro-quality-api-cn) · Deutsch [seedance-1-0-pro-quality-api-de](https://github.com/qt89oz47ugft9i/seedance-1-0-pro-quality-api-de) · Español [seedance-1-0-pro-quality-api-es](https://github.com/g708boud1/seedance-1-0-pro-quality-api-es) · Français [seedance-1-0-pro-quality-api-fr](https://github.com/fdu-6/seedance-1-0-pro-quality-api-fr) · Relay/gateway [seedance-1-0-pro-quality-api-gateway](https://github.com/bzo1u6sfkle/seedance-1-0-pro-quality-api-gateway) · Bahasa [seedance-1-0-pro-quality-api-id](https://github.com/i74lkq1472/seedance-1-0-pro-quality-api-id) · 한국어 [seedance-1-0-pro-quality-api-ko](https://github.com/t26b6qupjc93k7/seedance-1-0-pro-quality-api-ko) · Português [seedance-1-0-pro-quality-api-pt](https://github.com/onpfo05pvnb/seedance-1-0-pro-quality-api-pt) · Русский [seedance-1-0-pro-quality-api-ru](https://github.com/rb5mqsn/seedance-1-0-pro-quality-api-ru)
- **More models in this language** — [midjourney-api-ja](https://github.com/apimartt1q/midjourney-api-ja) · [gemini-2-5-flash-image-preview-api-ja](https://github.com/ic7skkh/gemini-2-5-flash-image-preview-api-ja) · [nano-banana-api-ja](https://github.com/e4ylndl3g-v/nano-banana-api-ja) · [gemini-3-1-flash-image-preview-api-ja](https://github.com/gv4ju/gemini-3-1-flash-image-preview-api-ja) · [nano-banana-2-api-ja](https://github.com/lpbxkrzbfel/nano-banana-2-api-ja) · [gemini-3-pro-image-preview-api-ja](https://github.com/zzsxs8r/gemini-3-pro-image-preview-api-ja)
- **Topic hubs** — every build of this model, all languages: [seedance-1-0-pro-quality](https://github.com/topics/seedance-1-0-pro-quality) · [seedance-1-0-pro-quality-api](https://github.com/topics/seedance-1-0-pro-quality-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-4a8e7a> | `apimart.ai/ja/model/doubao-seedance-1-0-pro` |
| Live pricing | <https://go.apimart.ai/k-756d46> | `apimart.ai/ja/pricing` |
| Get an API key | <https://go.apimart.ai/k-669595> | `apimart.ai/ja/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Seedance 1.0 Pro Quality is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
