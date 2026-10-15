# AI Tシャツデザイン事業（日本）— コスト調査ノート（初期費用・ランニング・1枚あたり原価）

FX assumption: **US$1 = ¥158** (USD/JPY was in the low ¥158s on 2026-10-01/02 per [OANDA証券 2026-10-02](https://www.oanda.jp/lab-education/?p=129491); JRI expects gradual yen appreciation, [JRI](https://www.jri.co.jp/report/medium/exchange/2026/)). For sensitivity, multiply any JPY figure converted from USD by 0.95 to get the ¥150 case. Research date: 2026-10-15.
How these notes were made: WebFetch and direct HTTP to the vendor pricing pages (ai.google.dev, openai.com, stripe.com, suzuri.jp, etc.) were refused by the sandbox proxy, and the web-search quota ran out partway through. **Every figure below comes from search-engine result summaries of the cited pages, not from reading the pages directly.** Where sources disagreed, both values are shown. Check official pages before committing numbers to the model.

---

## 1. Generative AI API costs (image generation, LLM, upscaling, background removal, moderation) and cost per design session

### Takeaway
Since August 2026 Google's main image-API line is **Nano Banana 2.1 (`gemini-nano-banana-2.1`): about $0.0336 (¥5.3) per 1K image and about $0.113 (¥17.9) per 4K image**. Imagen 4 is past its shutdown date on the Gemini API. With 5–20 generations at 1K plus one 4K final, a design session costs **about ¥45–125 on Nano Banana 2.1**, **¥145–460 on Nano Banana Pro**, and well under ¥10 on cheap FLUX schnell. LLM prompt help (¥0.3–6 per session) and moderation (free to ¥0.24 per image) are negligible. The main risks are (a) the image APIs have no free tier, so every generation costs money, and (b) the session-to-order conversion rate sets the AI cost per order.

### Cited Findings

**Google Gemini API: image models (Standard tier, per output image)**
| Model (nickname) | 0.5K | 1K | 2K | 4K | Batch | Notes |
|---|---|---|---|---|---|---|
| Nano Banana 2.1 `gemini-nano-banana-2.1` (released 2026-10-06 per one source) | – | $0.0336 (¥5.3) | $0.0504 (¥8.0) | $0.113 (¥17.9). One blog says $0.0756 instead; Google's page says $0.113 | about $0.0168 per 1K (¥2.7) | Image output billed at $30 per 1M tokens (about 1,120 tokens per 1K image). Input $1.50 per 1M; text/thinking output $7.50 per 1M. **No free API tier.** — [aifreeapi](https://www.aifreeapi.com/en/posts/nano-banana-2-1); [techaiwire](https://techaiwire.com/articles/nano-banana-2-1-gemini-image-model-half-price/); [OpenRouter](https://openrouter.ai/google/gemini-nano-banana-2.1); [laozhang](https://blog.laozhang.ai/en/posts/nano-banana-2-1). Treat as provisional: only newer secondary posts carry these figures. |
| Nano Banana 2 `gemini-3.1-flash-image(-preview)` | $0.045 (¥7.1) | $0.067 (¥10.6) | $0.101 (¥16.0) | $0.151 (¥23.9) | $0.022 / $0.034 / $0.050 / $0.076 | $60 per 1M image output tokens; batch $30 per 1M — [Gemini API pricing](https://ai.google.dev/gemini-api/docs/pricing); [Gemini 3 dev guide](https://ai.google.dev/gemini-api/docs/gemini-3). One source calls it deprecated — [laozhang/benchlm summary](https://benchlm.ai/media-pricing/nano-banana). A forum thread reports AI Studio and the docs showing different input prices — [Google AI forum](https://discuss.ai.google.dev/t/gemini-3-1-flash-image-preview-inconsistent-pricing-between-ai-studio-and-api-docs/127608) |
| Nano Banana Pro `gemini-3-pro-image(-preview)` | – | $0.134 (¥21.2) | $0.134 (¥21.2) | $0.24 (¥37.9) | $0.067 (1K/2K), $0.12 (4K) | 1,120 tokens at 1K/2K, 2,000 at 4K; Priority tier $216 per 1M image tokens — [Gemini API pricing](https://ai.google.dev/gemini-api/docs/pricing); same standard price on Vertex/Agent Platform — [Google Cloud pricing](https://cloud.google.com/gemini-enterprise-agent-platform/generative-ai/pricing) |
| Nano Banana (original) `gemini-2.5-flash-image` | – | about $0.039 (¥6.2) | – | – | – | One third-party summary says it **retires 2026-10-02** — [benchlm](https://benchlm.ai/media-pricing/nano-banana) / [puter](https://developer.puter.com/tutorials/gemini-api-pricing/). Google docs still name it as a migration target, and I found no official retirement date — [Gemini API Imagen page](https://ai.google.dev/gemini-api/docs/models/imagen). **Conflict, unresolved.** |
| Imagen 4 Fast / Standard / Ultra | – | $0.02 / $0.04 / $0.06 (¥3.2 / ¥6.3 / ¥9.5) | | | no batch discount | **Shutdown 2026-08-17 on the Gemini API**; Google recommends `gemini-nano-banana-2.1` as the replacement — [Gemini deprecations](https://ai.google.dev/gemini-api/docs/deprecations); [Imagen page: "now shut down"](https://ai.google.dev/gemini-api/docs/models/imagen). Prices from [RapidDevelopers (Jul 2026)](https://www.rapidevelopers.com/md/ai-api-limits-performance-matrix/imagen-4) and [invideo (Aug 2026)](https://invideo.io/blog/ai-image-model-pricing/). One aggregator lists Ultra at up to $0.12 — [pricepertoken](https://pricepertoken.com/imagen-pricing). On Vertex AI, older Imagen GA endpoints had to migrate by 2026-06-30 — [Vertex AI release notes](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/release-notes) |
- One source states that as of 2026-10-08 no Gemini image model, Nano Banana 2.1 included, has free API output — [laozhang: Gemini image API free?](https://blog.laozhang.ai/en/posts/gemini-image-api-free).
- Vertex AI is now branded as the "Gemini Enterprise Agent Platform" — [Vertex AI release notes](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/release-notes).

**Google Gemini API: text LLM for prompt assistance (per 1M tokens, input / output)**
- Gemini 2.5 Flash-Lite: $0.10 / $0.40 standard; batch $0.05 / $0.20; priority $0.18 / $0.72 — [Gemini API pricing](https://ai.google.dev/gemini-api/docs/pricing); [Google Developers Blog](https://developers.googleblog.com/en/gemini-25-flash-lite-is-now-stable-and-generally-available/)
- Gemini 3 Flash (preview): $0.50 / $3.00 — [Google blog](https://blog.google/products/gemini/gemini-3-flash/)
- Gemini 3.1 Flash-Lite: $0.25 / $1.50 — [Google blog](https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-3-1-flash-lite/)
- Gemini 3.8 Flash: introductory $0.75 / $3.75 until 2026-12-31, then **$1.50 / $7.50 from 2027-01-01** — [Gemini API pricing](https://ai.google.dev/gemini-api/docs/pricing) (search summary)

**OpenAI image models (1024×1024 per image unless noted)**
| Model | Low | Medium | High | Notes |
|---|---|---|---|---|
| gpt-image-2 | $0.006 (¥0.9) | $0.053 (¥8.4) | $0.211 (¥33.3) | 1024×1536 or 1536×1024: $0.005 / $0.041 / $0.165. Token rates: image input $8 per 1M, cached $2, **image output $30 per 1M**, text input $5 per 1M (batch is half) — [OpenAI pricing](https://developers.openai.com/api/docs/pricing); per-image figures from [costgoat (Oct 2026)](https://costgoat.com/pricing/openai-images) and [wavespeed](https://wavespeed.ai/blog/posts/gpt-image-2-pricing-2026/). Some sites quote lower figures ($0.003 / $0.027) — **conflict** |
| gpt-image-1.5 | $0.009 (¥1.4) | $0.034 (¥5.4) | $0.133 (¥21.0) | [OpenAI model page](https://developers.openai.com/api/docs/models/gpt-image-1.5) |
| gpt-image-1-mini | $0.005 (¥0.8) | $0.011 (¥1.7) | $0.036 (¥5.7) | Third-party figures; a community post says OpenAI's grid for this model is inconsistent — [pricepertoken](https://pricepertoken.com/gpt-image-pricing); [grida](https://canary.grida.co/ai/models) |
- Output sizes are 1024², 1024×1536 and 1536×1024 (the largest edge is 1536 px) — [costgoat](https://costgoat.com/pricing/openai-images).
- Resellers sell gpt-image-2 at $0.025 / $0.03 / $0.05 for 1K / 2K / 4K. These are not OpenAI prices — [apimodels](https://apimodels.app/models/gpt-image-2).

**Other image APIs**
- **Recraft (vector/SVG output, useful for print):** V3 Vector $0.08 (¥12.6), V4 Vector $0.08, V4.1 Vector $0.08, **V4 Pro Vector $0.30 (¥47.4)** — [Recraft API pricing doc](https://www.recraft.ai/docs/api-reference/pricing.md); [Recraft pricing page](https://www.recraft.ai/pricing?tab=api); [OpenRouter](https://openrouter.ai/recraft/recraft-v4-vector). V4.1 raster about $0.035, and a reported cut of V4.1 Pro to $0.21 (third-party, conflicts with the $0.30 figure) — [invideo](https://invideo.io/blog/recraft-ai-image-generator/)
- **Ideogram 3.0:** Turbo $0.03 (¥4.7), Default $0.06 (¥9.5), Quality $0.09 (¥14.2) per image. The transparent-background generation/upscale endpoint is $0.04 at Turbo. A character reference raises the price to $0.10–0.20 — [Ideogram API pricing](https://ideogram.ai/api-pricing/); [puter guide (Jun 2026)](https://developer.puter.com/tutorials/ideogram-api-pricing/)
- **FLUX:** FLUX.2 [pro] on fal is $0.03 for the first megapixel plus $0.015 for each additional megapixel — [aident](https://aident.ai/blog/fal-ai-image-pricing-per-image-vs-megapixel); [costgoat](https://costgoat.com/pricing/flux). Replicate FLUX.2 [pro] was about $0.015 per input/output MP and [flex] about $0.06 per MP (late-2025 data, possibly stale) — [pricepertoken](https://pricepertoken.com/flux-pricing). FLUX.1 schnell is $0.003 per image on fal (cheapest host $0.0005); FLUX.1 dev is $0.025 on fal (cheapest $0.009) — [costbench](https://www.costbench.com/software/ai-media-apis/black-forest-labs/); [puter FLUX (Jun 2026)](https://developer.puter.com/tutorials/flux-api-pricing/)
- **Stability AI** (price list effective 2025-08-01; one credit is about $0.01): Fast Upscale 2 credits ($0.02 / ¥3.2), Conservative Upscale 40 ($0.40 / ¥63), Creative Upscale 60 ($0.60 / ¥95), **Remove Background 5 ($0.05 / ¥7.9)**, edit tools 5 each — [Stability API price update](https://stability.ai/api-pricing-update-25); [puter Stability guide (Jun 2026)](https://developer.puter.com/tutorials/stability-ai-api-pricing/). The official page is dated 2025-08, so treat these as possibly stale.
- **Real-ESRGAN on Replicate (community models):** about $0.0011 per run (cjwbw, T4) to $0.0024 (nightmareai, T4); XXL variant about $0.0091; L40S variant about $0.041 — [Replicate cjwbw](https://internal.replicate.com/cjwbw/real-esrgan); [Replicate nightmareai](https://replicate.com/nightmareai/real-esrgan); [Replicate xxl](https://internal.replicate.com/sfarrowgr/real-esrgan-xxl-images); [Replicate lqhl](https://internal.replicate.com/lqhl/realesrgan)
- **remove.bg API:** $9 for 40 credits, or a $29/month subscription for 200 credits (1 credit = 1 image; failed removals still consume credits) — [toolradar](https://toolradar.com/tools/remove-bg/pricing). Another source lists Lite at $8.10/month for 40 and Pro at $35.10/month for 200 — [saascrmreview](https://saascrmreview.com/remove-bg-review/). That puts the effective cost at about **$0.15–0.23 (¥23–36) per image** (conflicting).
- **Moderation:** OpenAI `omni-moderation-latest` (text and image) is **free** and does not count against usage limits — [Analytics Vidhya (May 2026)](https://www.analyticsvidhya.com/blog/2026/05/openai-omni-moderation/); [eesel](https://www.eesel.ai/blog/openai-moderation-api). Google Cloud Vision SafeSearch: the first 1,000 units per month are free, then **$1.50 per 1,000** (units 1,001–5M) and $0.60 per 1,000 above 5M; free when bundled with Label Detection — [Cloud Vision pricing](https://cloud.google.com/vision/pricing?hl=hu); [vantaige 2026](https://vantaige.io/blog/google-vision-ai-pricing-explained-alternatives-2026)

### Inferences
- **Print-resolution math:** a 30×40 cm print area at 300 dpi needs about 3,543×4,724 px (about 16.7 MP). A 4K output (about 4,096 px long edge) on a 40 cm print gives about 260 dpi, which is usually acceptable for DTG/DTF. A 1K output (1,024 px) at 30 cm wide is about 87 dpi and needs roughly 4× upscaling. gpt-image tops out at 1,536 px, so it always needs an upscale step. Recraft vector output avoids the problem entirely.
- **Cost per design session (USD × 158; image costs only, then the add-ons):**

| Scenario | Light (5 previews + 1 final) | Typical (10 + 1) | Heavy (20 + 1) |
|---|---|---|---|
| Nano Banana 2.1: 1K previews + 4K final | $0.281 → **¥44** | $0.449 → **¥71** | $0.785 → **¥124** |
| Nano Banana 2 (3.1 Flash Image): 1K + 4K | $0.486 → ¥77 | $0.821 → ¥130 | $1.491 → ¥236 |
| Nano Banana Pro: 2K previews + 4K final | $0.91 → ¥144 | $1.58 → **¥250** | $2.92 → **¥461** |
| gpt-image-2: medium previews + 1 high + Real-ESRGAN | $0.48 → ¥76 | $0.74 → ¥117 | $1.27 → ¥201 |
| FLUX.1 schnell previews + Recraft V4 vector final | $0.095 → ¥15 | $0.11 → ¥17 | $0.14 → ¥22 |
  - Add-ons per session: LLM prompt assist (10 turns of about 1,000 input + 300 output tokens) costs ¥0.35 on 2.5 Flash-Lite, ¥2.2 on 3 Flash, and about ¥5.9 on 3.8 Flash at 2027 prices. Background removal is ¥7.9 (Stability) or ¥23–36 (remove.bg). An upscale is ¥0.2–1.4 (Real-ESRGAN) or ¥3.2 (Stability Fast). Moderation is ¥0 (OpenAI) or ¥0.24 per image (SafeSearch). **Realistic all-in session cost: about ¥55–135 on Nano Banana 2.1, about ¥155–475 on Nano Banana Pro.**
  - Batch pricing (−50%) only suits asynchronous jobs such as final 4K re-renders and mockups, not interactive previews.
- **AI cost per order depends on conversion.** If 1 in 5 sessions leads to an order, the AI cost per order is 5× the session cost: about ¥355 per order for a typical Nano Banana 2.1 session, or about ¥1,250 for Nano Banana Pro. At 1 in 10 it doubles again. This is the main AI-cost lever. Use generation caps or credits, cheap models for previews, and the expensive model only for the final render.
- With no free API tier, free-to-try generation without sign-up or rate limits leaks costs directly.
- Model churn is fast. Imagen 4 was shut down in August 2026, 2.5 Flash Image may have retired on 2026-10-02, and NB2 is reportedly deprecated after NB2.1 launched. Build a provider-agnostic abstraction and budget for re-testing prompts each quarter.

### Gaps
- I could not open ai.google.dev, openai.com or recraft.ai directly. Nano Banana 2.1's 4K price ($0.113 vs $0.0756) and its exact release date are unconfirmed.
- No retail price for the Topaz Labs API or Clipdrop upscale/remove-background API was retrieved.
- Vertex AI per-image prices for Imagen 4 upscaling and Nano Banana 2.1 were not confirmed.
- Japanese consumption tax: whether Google/OpenAI invoices add 10% JCT (or require reverse-charge handling) for a Japanese corporation was not verified.
- Licensing and commercial-use terms (indemnity, output ownership) for each API were not checked. They are relevant to the resale marketplace.

---

## 2. Printing and fulfillment costs in Japan (outsourced 1-unit POD, blank shirts, Printful comparison, shipping)

### Takeaway
I found **no published B2B (wholesale) price** from Japanese POD or drop-ship providers such as Printio, UP-T API or SUZURI wholesale; those prices come by quotation. Consumer prices for **one full-color white shirt (body + print) run about ¥1,450–2,800 tax included**. Common points: TMIX from ¥1,458, UP-T from about ¥2,200 (¥2,772 measured with shipping), DTF from about ¥3,581. A blank United Athle 5001 costs about ¥519–935 retail. **Domestic mail-type shipping is ¥240 (Click Post, from 2026-10) to ¥385 (Nekopos ceiling)**. Japan Post raised prices on 2026-10-01.

### Cited Findings

**One-unit full-color print, consumer price (tax included unless noted)**
| Provider | Price | Notes / source |
|---|---|---|
| UP-T | **from ¥2,200 per shirt for one** (comparison site, as of 2026-09-14). Official site: "from ¥1,000 per shirt" with the maximum bulk discount; standard shirts from ¥1,100 | [tshirt-kakaku UP-T](https://tshirt-kakaku.com/companies/up-t/); [UP-T official](https://up-t.jp/); [UP-T T-shirt category](https://up-t.jp/item-detail/category/%E3%82%AA%E3%83%AA%E3%82%B8%E3%83%8A%E3%83%ABT%E3%82%B7%E3%83%A3%E3%83%84) |
| UP-T (measured) | **¥2,772 for one shirt, tax and shipping included**; about ¥1,650 at 50+ | [originaltshirt-guide review](https://originaltshirt-guide.com/up-t-review/) |
| UP-T print fee | On a white shirt, printing is a **flat ¥800** with no shipping or handling fees (article about a year old) | [UP-T news/622](https://up-t.jp/news/622) |
| UP-T DTF (full-color transfer) | **from ¥3,581 per shirt** | [UP-T DTF page](https://up-t.jp/item-detail/category/%E3%82%B7%E3%83%A3%E3%83%84/print/%E3%83%95%E3%83%AB%E3%82%AB%E3%83%A9%E3%83%BC%E8%BB%A2%E5%86%99(DTF)%E3%83%97%E3%83%AA%E3%83%B3%E3%83%88) |
| TMIX | Full-color inkjet **from ¥1,325 (¥1,458 incl. tax)**; budget shirt from ¥1,463 incl. tax; standard shirt from ¥1,595 (¥1,755 incl. tax). An older "¥3,190 (¥3,509 incl. tax) regardless of colors" notation is still on the site | [TMIX category](https://tmix.jp/product_categories/1); [TMIX budget](https://tmix.jp/product_categories/1/inexpensive); [TMIX mens](https://tmix.jp/product_categories/1/mens); [TMIX inkjet](https://tmix.jp/prints/ink-jet) |
| オリジナルプリント.jp (per a UP-T competitor comparison) | Body ¥660 white / ¥715 colored (incl. tax); **inkjet 32×38 cm chest print ¥1,254**; **shipping ¥750 nationwide** (Okinawa excluded). Another review: from ¥1,353 excl. tax. Possibly stale | [UP-T news/2245](https://up-t.jp/news/2245); [custom-fashion-magazine](https://custom-fashion-magazine.jp/13672/) |
| オリジナルプリント特急便 (フジ総合印刷, a different company from オリジナルプリント.jp) | Headline "from ¥539 per shirt" (lowest-tier body; conditions unknown) | [original-print.jp](https://www.original-print.jp/product-category/short-sleeve-t-shirt/) |
| Robin Factory (United Athle 5001, print included) | ¥930 incl. tax per shirt at 10–50 shirts; "from ¥778" | [robinfactory](https://www.robinfactory.co.jp/?product=5001-01) |
| CustoMee (Hamee; manufactured via the Printio API) | Retail **from ¥3,281 incl. tax** per T-shirt (sale price, not wholesale) | [Printio case study](https://printio.me/casestudy/customee/) |
| SUZURI Standard T-shirt | A 2026 list article gives **from ¥3,091**. Another article gives **cost price from ¥2,080 excl. tax**. Cost price was ¥2,816 incl. tax as of 2024-03. **Conflicting; official 2026 price not confirmed** | [melo-blog 2026](https://melo-blog.com/suzuri-gazou-size); [imano-okiniiri](https://imano-okiniiri.com/suzuri-pricing-guide/); [akaeho](https://akaeho.net/blog/suzuri-t-shirt/); [watasiino](https://watasiino.com/contents/4714) |

**B2B / API drop-shipping**
- **Printio API** (OpenFactory) automates product registration, ordering, order retrieval and inventory. The spec was published in 2025-10 and needs a retailer ID and API token. Factories join with no initial or participation fee; billing is per order; Printio carries manufacturing liability; it produces one at a time with individual shipping. **The wholesale price list is not public** — [PR Times (API spec)](https://prtimes.jp/main/html/rd/p/000000019.000066048.html); [Printio API news](https://printio.me/news/printio-api/); [Printio case study](https://printio.me/casestudy/customee/)

**Blank shirt prices**
- United Athle 5001-01 (5.6oz) at the specialist shop **¥519–935 incl. tax** by size and color (list price ¥1,386) — [unitedathle.net](https://www.unitedathle.net/?pid=10478147). MonotaRO lists mixed grey XXXL at ¥1,758 incl. tax — [MonotaRO](https://www.monotaro.com/p/1377/1725/).
- Printstar 085-CVT: no price retrieved (gap).

**Printful (overseas POD) shipping to Japan**
- Printful's shipping page shows **Japan $4.75 first item / +$2.00 each additional** — [Printful shipping](https://www.printful.com/shipping). Another version shows **$4.39 / +$1.50** — [Printify's Printful rate page](https://printify.com/shipping-rates/printful). International standard delivery takes 5–20 business days after fulfillment — [Printful shipping](https://printful.com/shipping). At ¥158 that is **about ¥694–751 per shirt**. **Conflicting.**

**Domestic shipping (after the 2026-10-01 Japan Post rate change)**
- **Click Post: ¥185 → ¥240** (2026-10-01). Weight limit raised from 1 kg to 2 kg; the 3 cm thickness limit is removed; post-office counter drop-off ends (mailbox only) — [haruuu](https://haruuu.online/clickpost-neage-240en-2026/); [stockcrew 2026-07-06](https://stockcrew.co.jp/insights/article_jiji_japanpost_neage_2026-07-06)
- **Yu-Packet: the ¥250/310/360 thickness tiers merge into a flat ¥360** — [stockcrew 2026-07-24](https://stockcrew.co.jp/insights/article_jiji_japanpost_rate_2026-07-24); [TFP](https://tfp-group.co.jp/tool/postage/column/yuupack-yuupacket-kaitei)
- Yu-Pack: average increase of about 10% — [stockcrew](https://stockcrew.co.jp/insights/article_jiji_japanpost_neage_2026-07-06)
- **Nekopos (Yamato) continues.** Yamato withdrew the planned discontinuation in 2025-02, so it now runs alongside Kuroneko Yu-Packet. Corporate-contract ceiling **¥385** nationwide; Yahoo Fleamarket's おてがる配送 shows ¥210 — [stockcrew Yamato 2026](https://stockcrew.co.jp/insights/delivery-yamato); [Yahoo support](https://support.yahoo-net.jp/SccPaypayfleamarket/s/article/H000008477)
- Kuroneko Yu-Packet is priced in three thickness tiers (1/2/3 cm; corporate contract) — [stockcrew Yamato](https://stockcrew.co.jp/insights/delivery-yamato)
- **Takkyubin Compact:** box ¥70; within Kanto ¥650 cash / ¥649 cashless. It was excluded from the 2025-10 Yamato increase (only 120 size and up changed) — [Yamato official](https://www.kuronekoyamato.co.jp/ytc/search/payment/result_02.html); [stockcrew](https://stockcrew.co.jp/insights/logi-240525); [pricey](https://www.pricey.jp/web/articles/4484)

### Inferences
- **Planning figures for 1-unit outsourced fulfillment (white shirt, full-color front print, tax included):** low ¥1,460 (TMIX promo-level), **base ¥2,200 (UP-T 2026-09)**, high ¥2,770 (UP-T measured, shipping included) or ¥3,580 (DTF). The オリジナルプリント.jp breakdown (¥660 body + ¥1,254 print + ¥750 shipping = **¥2,664**) agrees with the base case.
- These are consumer prices. A platform at volume should be able to negotiate 20–40% below them, but this is unverified: get B2B quotes from Printio, UP-T (business/API), TMIX and others. Also confirm the service can **ship directly to the end customer under the platform's name** (blind/white-label shipping).
- A folded T-shirt in a poly mailer fits Click Post or Nekopos, so **¥240–385** is the domestic shipping assumption if the provider's price excludes delivery. Japan-based providers beat Printful-from-overseas (about ¥700–750 shipping plus 5–20 days plus a USD body cost) for the Japanese market.
- Use the SUZURI price (from about ¥2,080 excl. tax / ¥2,816 incl. tax) as the competitive floor. A platform that pays consumer POD prices has no cost advantage over SUZURI.

### Gaps
- No public B2B or wholesale price was found for Printio, UP-T business/API, ClubT, Tシャツトリニティ or SUZURI's maker-side cost. ClubT and Tシャツトリニティ prices were not retrieved.
- No Gelato (local Japanese production) base price or shipping figures were found.
- No Printful base product price for a Bella+Canvas or Gildan tee was retrieved.
- Printstar 085-CVT wholesale or retail price was not retrieved. United Athle wholesale (業販) price was not found, only retail.
- The official SUZURI 2026 base price is unverified (suzuri.jp was blocked).
- Packaging materials (OPP bag, mailer, thank-you card) cost per unit: not sourced.
- Japan Post's own official rate table was not opened. The 2026-10 figures come from logistics-firm articles.

---

## 3. In-house printing alternative (DTG/DTF equipment, consumables, labor, space)

### Takeaway
A professional DTG machine costs about **¥1.85M (Brother GTX pro, Japanese list price incl. tax)**. The US MSRP of the Epson F2270 is $12,995 (about ¥2.05M). Per-shirt ink plus pretreatment is roughly **$3–4 (¥470–630) front and back** for GTX (older article), plus the blank shirt (about ¥520–935) and labor. In-house printing only beats outsourcing (about ¥2,200 per shirt) at steady volume. The Japanese DTF printer price, consumables and labor figures were not obtained.

### Cited Findings
- **Brother GTX pro (Japan): body ¥1,848,000 incl. tax.** Headline says "June 2026 latest", but the date of the price could not be confirmed — [europort 6-machine comparison](https://europort.jp/blog/garment-comparison)
- **Epson SureColor F2270 (hybrid DTG/DTF, US):** MSRP **$12,995** (¥2.05M) — [Epson US](https://epson.com/For-Work/Printers/Large-Format/SureColor-F2270-Standard-Edition-Printer/p/SCF2270SE); dealer price $11,995 — [EquipmentZone](https://www.equipmentzone.com/epson/surecolor-f2270-dtg-dtf-printer/)
- **Brother GTX Pro (US):** MSRP $24,800, promotional about $15,995 — [makerflo 2026](https://makerflo.com/blogs/craft-library/best-dtg-printer-for-small-business) / [designnbuy 2026](https://www.designnbuy.com/blog/best-dtg-printers/) (search summary; exact attribution between these two is uncertain)
- **Ink prices:** F2270 about $0.51 per ml; GTX pro about $0.37 per ml (the comparison is not on the same basis) — same comparison sources as above
- **Average Brother GTX print cost: $3–4 per shirt (front and back).** The article is old and notes that GTX uses more ink than some rivals — [YoPrint](https://www.yoprint.com/blog/which-to-buy-brother-gtx-epson-f2100-or-kornit-breeze)

### Inferences
- Variable cost per shirt in-house: blank ¥520–935 + ink/pretreatment about ¥300–630 (front only to front and back) + film/powder (if DTF) + labor (a few minutes per shirt including pretreatment and press) + packaging. That gives roughly **¥1,000–1,700 variable per shirt** against about ¥2,200 outsourced. Capital: ¥1.85–2.5M for the printer plus a heat press, pretreatment machine and ventilation (not sourced). Depreciated over 3 years that is about ¥51–70k per month in printer depreciation alone, before rent and staff. **Break-even against outsourcing is likely at several hundred shirts per month or more.** This is a rough estimate. In-house also brings misprint risk, color-management labor and a ventilated space requirement.
- Recommendation for an MVP: outsource (asset-light). Revisit in-house or a partner factory once volume is stable.

### Gaps
- Japanese prices for DTF printers (A3 or 60 cm roll), shakers or curing units, film and powder per shirt: not retrieved (search budget exhausted).
- Epson SC-F2270 or SC-F1000 Japanese list price and ink cartridge prices: not retrieved.
- Labor rates (Tokyo minimum wage 2026, part-time production staff hourly wage) and small workshop rent: not retrieved. From memory and unverified: Tokyo's minimum wage was ¥1,226 from 2025-10. Check the 2026 revision.
- Maintenance contracts, head replacement costs and failure or misprint rates: not retrieved.

---

## 4. Payment fees and creator payouts

### Takeaway
On card payments, **Stripe JP charges 3.6%** for domestic cards (no monthly fee), **PAY.JP Standard 3.3%** (no monthly fee; 2.78% at ¥20k/month; 2.59–2.7% at ¥50k/month), and **Shopify Payments 3.55% (Basic) / 3.4% (Grow)**. For marketplace royalties, Stripe Connect adds **¥200 per active account per month plus ¥250 + 0.25% per payout** (secondary sources). Small monthly payouts therefore lose 5–15% to fees, so a minimum payout threshold is essential.

### Cited Findings
**Stripe (Japan)**
- Domestic cards **3.6%**; no initial or monthly fee — [PAY.JP column on Stripe fees (2026)](https://pay.jp/column/stripe-fees-guide); [enhanceit (2026)](https://enhanceit.jp/stripe-tesuryo-hikaku/). Note that the first source is a competitor's column.
- Internationally issued cards **+1.5%**; currency conversion **+1.5%** — [PAY.JP column](https://pay.jp/column/stripe-fees-guide)
- Convenience-store payments: sources conflict (3.6% + ¥100 / 1.5% + ¥100 / 3.6% with ¥120 minimum); refund ¥250 + tax. Bank transfer 1.5% (+¥30–40, or ¥120 minimum; conflicting) — [PAY.JP column](https://pay.jp/column/stripe-fees-guide); [enhanceit](https://enhanceit.jp/stripe-tesuryo-hikaku/)
- PayPay quoted at 3.98% for general online sales (9.48% for digital content). It is unclear whether this rate applies via Stripe — [enhanceit](https://enhanceit.jp/stripe-tesuryo-hikaku/)
- Payouts of domestic-card proceeds to your own bank: **¥0 with automatic payouts** — [PAY.JP Stripe guide](https://pay.jp/column/stripe-japan-guide)
- **Stripe Connect:** **¥200 per active account per month** (¥220 incl. tax; "active" means the account received a payout that month) plus **¥250 + 0.25% per payout** — [PAY.JP Connect guide](https://pay.jp/column/stripe-connect-guide); [Zenn](https://zenn.dev/kotap15/articles/stripe_usecases); [mekuma (2020)](https://mekuma.jp/2020/02/21/stripe-connect-custom-4-commissions/). The official stripe.com/jp/connect/pricing page was not reachable. Connected Account Agreement (JP, 2025-11-18) — [Stripe PDF](https://images.stripeassets.com/fzn2n1nzq965/1g69xUZno6foTC76ukERn8/dc279d6f2d3c7c650c0ffd8f761634e3/Japan__Japanese__-_Stripe_Connected_Account_Agreement__2025-Nov-18_.pdf)

**PAY.JP**
- Standard: **¥0/month, 3.3%** all brands. Business: **¥20,000/month, 2.78%**. Enterprise: **¥50,000/month, 2.59% (Visa/Mastercard), 2.7% (JCB etc.)** — [PAY.JP help](https://help.pay.jp/ja/articles/5044185-%E6%9C%80%E9%81%A9%E3%81%AA%E3%83%97%E3%83%A9%E3%83%B3%E3%81%AE%E9%81%B8%E3%81%B3%E6%96%B9%E3%82%92%E6%95%99%E3%81%88%E3%81%A6%E3%81%8F%E3%81%A0%E3%81%95%E3%81%84); [PAY.JP plan](https://pay.jp/plan)
- **PayPay added 2026-02:** physical goods and services **3.5%**, digital content 9.0% (plus consumption tax; rate set at review) — [PAY.JP notice 2026-02-12](https://pay.jp/info/2026-02-12-123000)

**KOMOJU**
- Convenience-store payment fee changed from **¥190 to ¥220 incl. tax on 2024-11-01** (merchant notice) — [furyu-hm notice](https://furyu-hm.com/blogs/news/20250609). KOMOJU's blog also mentions a **2.75%** transaction fee on all convenience-store purchases — [KOMOJU konbini](https://ja.komoju.com/blog/convenience-store-settlement/konbini-payments/)
- PayPay via KOMOJU: **1.60% (Light plan, with conditions) / 1.98% (Restricted plan)** — [KOMOJU PayPay guide](https://ja.komoju.com/blog/smartphone-settlements/paypay/)

**Shopify Japan**
- Basic: **¥4,850/month (monthly billing), ¥3,650/month (annual)**. Grow: **¥13,500 / ¥10,100**. Shopify Payments domestic cards **3.55% (Basic), 3.4% (Grow)**; Advanced 3.25%. International/AMEX 3.9% (Basic), 3.85% (Grow). Staff accounts: Basic 1, Grow 5. Grow becomes cheaper than Basic above about ¥5.77M in monthly sales — [Finner 2026](https://finner.co.jp/media/shopify-start-guide/); [Rabo](https://rabo.co.jp/blog/shopify/shopify-plan-guide); [spectacle 2026](https://spectacle.world/contents/shopify-fees/)

### Inferences
- **Card fee on a ¥4,400 (tax-included) order:** Stripe ¥158; PAY.JP Standard ¥145; PAY.JP Business ¥122 (+¥20k/month); Shopify Basic ¥156.
- PAY.JP break-even: Business beats Standard above about ¥3.85M in monthly card volume (¥20,000 ÷ 0.52%). Enterprise beats Standard above about ¥7.0M per month (¥50,000 ÷ 0.71%).
- **Creator payout cost on Stripe Connect:** a creator paid ¥3,000 in a month costs ¥200 + ¥250 + ¥7.5 = **¥457.5 (15% of the payout)**. At ¥10,000 it is ¥475 (4.75%); at ¥30,000, ¥525 (1.75%). Mitigations: a minimum payout threshold (for example ¥5,000–10,000) with carry-over, monthly batching, or passing the transfer fee to the creator, as Japanese platforms commonly do (not verified for specific platforms).
- An alternative to Connect is collecting with Stripe or PAY.JP and paying creators by bank transfer (one ¥100–250 transfer per payee), but that makes the platform responsible for KYC, invoice handling (インボイス), any withholding rules and possibly 資金移動業 or 収納代行 legal analysis. This needs legal review; see section 6.

### Gaps
- GMO-PG (GMO Payment Gateway) fees were not retrieved (search budget exhausted).
- Corporate net-bank transfer fees (GMO Aozora, Sumishin SBI, etc.) for bulk creator payouts were not retrieved.
- Stripe's official Japan pricing (konbini, PayPay, Connect) was not opened directly. All Stripe figures come via secondary or competitor articles.
- Chargeback or dispute fees and Stripe Radar pricing in Japan were not retrieved.

---

## 5. Development cost (MVP) and monthly infrastructure

### Takeaway
Japanese rates: freelance engineers average **¥784k per month** (2026-07). Contract-development person-month rates are **PG ¥0.4–1.0M, SE ¥0.6–1.2M, PM ¥0.8–1.6M**, with ¥0.8–1.2M per month the typical range. Generic benchmarks are a small system at ¥1–3M and a custom EC/matching web system at **¥3–10M**. My inference is that a custom MVP covering the AI generator, editor, cart, marketplace, payouts and admin costs **about ¥6–20M**. Early-stage infrastructure (Vercel Pro + Supabase Pro) runs about **¥12–50k per month** before scale.

### Cited Findings
- Freelance engineer average monthly rate: **¥784k (2026-07)**; 435,426 listings; maximum ¥2.5M — [Freelance Start / PR Times](https://prtimes.jp/main/html/rd/p/000001254.000000725.html). ¥780k (2026-03) — [TV Tokyo Plus (PR)](https://www.tv-tokyo.co.jp/plus/external-pr/entry/72623.html)
- TypeScript listings: average annual equivalent **¥9.52M**. React has the most listings by framework; Next.js has the highest unit rate; 91.2% remote — [@IT 2026-05](https://atmarkit.itmedia.co.jp/ait/articles/2605/08/news038.html)
- **Person-month rates:** PM ¥1.0–1.6M (another source ¥0.8–1.5M); senior SE ¥0.8–1.2M; mid-level SE ¥0.6–1.0M; PG ¥0.4–0.8M (another source ¥0.6–1.0M). Contract development commonly ¥0.8–1.2M per month; team engagements ¥1.0–2.5M per month — [xincere](https://blog.xincere.jp/articles/system-development-man-month-unit-price); [overx](https://overx.co.jp/media/man-month-rate/); [enlyt 2026](https://enlyt.co.jp/blog/system-dev-pricing); [siainc 2026](https://www.siainc.jp/topic/system-development-cost)
- Small system: **¥1–3M**. Web system (EC or matching): customization **¥0.5–3M**, from scratch **¥3–10M** — [clyr cost guide 2026](https://casually.clyr.co.jp/guides/cost-guide); [siainc](https://www.siainc.jp/topic/system-development-cost)
- EC site by method: ASP free to ¥1M; cloud EC from ¥3M; package from ¥5M; full scratch ¥10M+ (another source: tens of millions to hundreds of millions of yen, 6+ months). By vendor: freelancer ¥0.3–1M, small web firm ¥1–3M, EC specialist ¥2–5M — [mercart 2026](https://mercart.jp/contents/detail/177/); [EC-CUBE 2026](https://www.ec-cube.net/know-how/ec-build-price/); [atsoho](https://atsoho.com/blog/ecommerce-site-build-cost)
- Overseas marketplace MVP via no-code: up to $2,000 for a simple build, $2,000–10,000 for a moderate one, $100k+ for custom from scratch. The source is a vendor blog, so discount it — [Shipturtle](https://www.shipturtle.com/blog/cost-to-build-an-online-marketplace-website)
- **Vercel Pro:** $20 per developer seat per month including a $20 usage credit; 1 TB Fast Data Transfer and 10M Edge Requests included; overage about $0.15/GB (US) and $2 per 1M edge requests. Example: 3 seats with 250k MAU and 5M API requests costs about **$305/month** — [MakerKit Vercel cost 2026](https://makerkit.dev/blog/saas/vercel-cost); [dev.to](https://dev.to/nayankyada/vercel-pricing-2026-what-you-actually-pay-for-a-real-nextjs-project-33md)
- **Supabase Pro:** **$25/month** (sources differ on whether per organization or per project); $10 compute credit covers one Micro instance; storage overage about $0.021/GB-month (100 GB included); typical small or medium apps **$35–75/month**. Team plan $599/month — [MakerKit Supabase](https://makerkit.dev/blog/saas/supabase-pricing); [Jetadmin 2026](https://www.jetadmin.io/blog/supabase-pricing-2026-guide-to-plans-limits-and-real-world-costs/); [toolradar](https://toolradar.com/blog/supabase-pricing-2026)
- Shopify route: Basic ¥3,650–4,850 per month; Grow ¥10,100–13,500 per month (see section 4) — [Finner](https://finner.co.jp/media/shopify-start-guide/)

### Inferences
- **Custom MVP scope estimate:** Next.js app, AI generation flow with model abstraction, canvas editor and mockup (Fabric.js/Konva), cart and checkout (Stripe), creator storefronts and listings, royalty ledger and Connect payouts, admin panel and moderation queue, print-provider order API integration. Roughly 3–4 people for 3–4 months, or **10–16 person-months**.
  - Contract firm at ¥0.8–1.2M per person-month: **about ¥8–19M** (plus requirements definition, QA and PM overhead).
  - Freelance team (2–3 people at about ¥0.78M per month for 4 months): **about ¥6–9M** (founder handles PM).
  - **Shopify + POD app route:** the Japanese benchmark "freelancer ¥0.3–1M / small web firm ¥1–3M" for EC builds, plus ¥3,650–13,500 per month for the plan and app fees. Shopify does not natively support a multi-creator marketplace with royalties or an AI generator UI, so it fits a single-brand AI-designed store better than a full marketplace.
- **Monthly infrastructure (early stage):** Vercel Pro 2 seats ($40) + Supabase Pro ($25–75) + image storage/CDN ($5–50) + monitoring and email (unsourced, about $20–50). Roughly **$90–215/month, about ¥14–34k**. At about 250k MAU the Vercel example alone is about $305 (¥48k). AI API usage is variable and scales with sessions (section 1).
- Image storage: a 4K PNG of about 10–20 MB is an assumption, not sourced. 10,000 final designs would be about 100–200 GB, or a few thousand yen per month on S3/R2-class storage.

### Gaps
- Japanese articles giving development-cost examples specifically for an AI-generation SaaS or a POD marketplace were not found.
- Cloud Run, Firebase, Cloudflare R2 and AWS S3/CloudFront pricing were not retrieved. From memory and unverified: R2 is about $0.015/GB-month with no egress fee.
- Monthly fees for Shopify POD apps (Printful, Printify, SUZURI-type Japanese apps) were not retrieved.
- Maintenance costs (commonly cited at 15% of build cost per year in Japanese articles) were not sourced.

---

## 6. Operating costs (company setup, trademarks, legal, accounting, moderation, CS, marketing, returns)

### Takeaway
Fixed setup: **KK about ¥200–240k / GK about ¥75–112k** to incorporate; **trademark ¥44,900 per class** in official fees (10 years, self-filed); **terms of service by a lawyer ¥100–300k+**; **tax accountant ¥15–35k per month + ¥100–200k annual filing** (sales under ¥50M). Marketing benchmarks: **Instagram apparel CPC about ¥80–120, ROAS 300–500%**. That implies ad cost of roughly 20–33% of revenue for paid-acquired orders. AI moderation is nearly free (OpenAI moderation is free). No data was found on human-moderation hours, CS outsourcing or misprint/return rates.

### Cited Findings
**Company setup**
- Typical cost: KK about ¥240k, GK about ¥110k — [VS Group](https://vs-group.jp/tax/startup/new/kigyou_price/). With paper articles: **KK from ¥233,000, GK from ¥112,000**. With electronic articles: **KK from ¥196,000, GK from ¥75,000** — [Money Forward (updated 2026-08-20)](https://biz.moneyforward.com/establish/basic/?p=158)
- Registration license tax: **KK from ¥150k, GK from ¥60k**. Articles notarization fee ¥15k–50k (KK only, varies with capital). Revenue stamp ¥40k for paper articles; ¥0 for electronic articles. GK needs no notarization — [Money Forward](https://biz.moneyforward.com/establish/basic/?p=158); [sogyotecho](https://sogyotecho.jp/company_fee/)
- Capital guideline: initial costs plus about 6 months of running costs — [Money Forward](https://biz.moneyforward.com/establish/basic/?p=158)

**Trademark (JPO official fees)**
- Application fee **¥3,400 + ¥8,600 × number of classes**. Registration fee **¥32,900 × classes (10 years)** or ¥17,200 × classes (5-year installment). **1 class = ¥44,900; 2 = ¥86,400; 3 = ¥128,900.** Paper filing adds an electronization fee (¥2,400 + ¥800 per page) — [JPO fee list](https://www.jpo.go.jp/system/process/tesuryo/hyou.html); [ayusawa-partners](https://ayusawa-partners.jp/column/shouhyou-touroku-shutsugan); [crexgroup](https://crexgroup.com/ja/consulting/management/trademark-registration-cost/)
- Patent-attorney fees are set freely by each firm; no reliable range was retrieved — [crexgroup](https://crexgroup.com/ja/consulting/management/trademark-registration-cost/)

**Legal and accounting**
- Terms of service drafted by a lawyer: often **¥100k–300k+**. The source is a side-job article, so weak evidence — [atsoho](https://atsoho.com/blog/terms-of-service-drafting-side-job); Money Forward also explains lawyer cost ranges but the figure was not visible in the snippet — [Money Forward contract](https://biz.moneyforward.com/contract/basic/23030/)
- Tax accountant (corporate): **sales under ¥50M: ¥15–25k per month (quarterly visit) / ¥25–35k (monthly visit); annual filing ¥100–200k. Sales ¥50M+: from ¥40k per month (quarterly) / from ¥45k (monthly); filing from ¥300k** — [CrowdWorks Times (citing 辻・本郷)](https://crowdworks.jp/times/know-how/26769/). Other sources: ¥10–50k per month, with filing at 4–6 months of the monthly fee — [biz.ne.jp](https://www.biz.ne.jp/tax/info/02.html); ¥20–50k per month plus filing (2026) — [dosai](https://dosai.jp/news/sakai-zeirishi-komonryo-souba-2026/)

**Content moderation**
- OpenAI omni-moderation (text and image) is free — [Analytics Vidhya](https://www.analyticsvidhya.com/blog/2026/05/openai-omni-moderation/). Google Vision SafeSearch costs $1.50 per 1,000 images after 1,000 free per month (about ¥0.24 per image) — [Cloud Vision pricing](https://cloud.google.com/vision/pricing?hl=hu)

**Marketing benchmarks (Japan, 2026 agency articles; ranges vary widely)**
- Instagram ads, apparel/fashion: **CPC ¥80–120, ROAS 300–500%**. Another source: low- to mid-priced apparel **CPC ¥65–125 (average ¥95)**. Overall CPC ¥40–200 — [lifunext 2026](https://lifunext.com/blog/sns/instagramad/sns-snsad-14/); [FIRST 2026](https://fir-st.co.jp/blog/instagram-monetization-conditions/); [curumi 2026](https://curumi.co.jp/blog/instagram-ads-2)
- Instagram CPM **¥500–1,000** (a broader range of ¥500–3,000 also cited) — [primenumbers](https://primenumbers.co.jp/blog/snsads/instagram-ad-budget/); [cmstaff](https://cmstaff.co.jp/marketing-blog/snsads/)
- **TikTok CPC ¥30–100, CPM ¥300–600** vs Instagram CPC ¥50–150, CPM ¥400–800 (one comparison table) — [marketingone](https://marketingone.co.jp/sns-ads-trends-instagram-tiktok-comparison-2025/); [HolyTech](https://holytech.jp/column/tiktok-youtube-instagram-sns-operate-sns-operate-agency-guid/). TikTok CPA expectations for mid-priced goods such as apparel exist in one article, but the figure was truncated — [0120.co.jp (2026-05)](https://0120.co.jp/blog/video-84/)
- (Attribution of individual numbers across these agency articles comes from search summaries and may be imprecise.)

### Inferences
- **Paid-acquisition cost per order:** at ROAS 300–500%, ad spend is 20–33% of revenue, so **¥880–1,470 per ¥4,400 order**. Check against CPC: at ¥95 CPC and an assumed 1–2% site conversion rate, CPA would be about ¥4,750–9,500, which is far worse than the ROAS-implied figure. The difference shows that first-order CPA for an unknown brand is likely to exceed the contribution margin. Growth would have to come from organic or UGC sources, creators promoting their own listings, and repeat purchases.
- **Relevant Nice classes for trademarks** (inference): 25 (clothing), 35 (online retail services), 40 (custom printing), 42 (SaaS / design-generation software). Three classes cost ¥128,900 and four cost ¥169,400 in official fees, plus attorney fees.
- **One-time setup budget (excluding development):** KK about ¥200–240k + trademark (2–4 classes) about ¥86–170k + terms and privacy policy about ¥100–300k + misc. That totals **about ¥0.4–0.7M**.
- **Recurring back-office:** tax accountant about ¥280–620k per year at the sub-¥50M sales stage.
- **Moderation:** the API cost is negligible. Human review of flagged items, plus IP and trademark infringement checks on marketplace listings (character, brand and celebrity likeness), is the real cost and should be modeled as staff hours.

### Gaps
- Human-moderation labor hours per 1,000 listings, CS outsourcing prices (per ticket or monthly), and Tokyo's 2026 minimum wage: not retrieved (search budget exhausted).
- Misprint, defect or return rates for POD apparel (Japan or global) were not found. No source for typical reprint allowances.
- CPA or CAC benchmarks specifically for Japanese D2C apparel were not found. Only CPC, CPM and ROAS ranges.
- Lawyer cost for Specified Commercial Transactions Act (特商法) / Payment Services Act (資金決済法) review of the royalty and payout scheme was not found.
- Patent-attorney fee ranges for trademark filing were not found.
- Consumer-protection or return-policy legal requirements (custom-made goods are usually excluded from returns under the store's own policy) were not researched here.

---

## 7. Retail price, gross margin and creator royalty benchmarks (Japan)

### Takeaway
SUZURI, the reference platform, uses a **cost price (from about ¥2,080 excl. tax; ¥2,816 incl. tax in 2024)** plus a creator markup (トリブン) that **defaults to ¥300 and can be set from ¥0 to ¥5,000**. A Standard Tee lists from about ¥3,091. Comparable custom-tee services retail from about ¥3,281 (CustoMee). A typical retail anchor is therefore **¥3,000–5,000 per shirt**, with creator royalties commonly in the ¥300–1,000 range. That last figure is an inference; the survey data was not retrieved.

### Cited Findings
- SUZURI: retail price = cost price + トリブン. トリブン defaults to **¥300** and can be set **from ¥0 to ¥5,000** — [imano-okiniiri](https://imano-okiniiri.com/suzuri-pricing-guide/)
- SUZURI Standard Tee listed **from ¥3,091** (2026 article; it notes prices change) — [melo-blog](https://melo-blog.com/suzuri-gazou-size). Cost price **from ¥2,080 excl. tax** per another article — [imano-okiniiri](https://imano-okiniiri.com/suzuri-pricing-guide/). **¥2,816 incl. tax as of 2024-03** — [akaeho](https://akaeho.net/blog/suzuri-t-shirt/); [watasiino](https://watasiino.com/contents/4714) (attribution of each figure to a specific article comes from a search summary)
- SUZURI ran a T-shirt sale with **¥1,000 off all eligible tees** in 2026-07 (temporary discount, not a base-price change) — [SUZURI media](https://suzuri.jp/media/journal_t-shirts-sale_202607/)
- There is a survey of "how much トリブン per T-shirt" on SUZURI and Tシャツトリニティ, but its results were not retrieved — [okachanblog](https://okachanblog.com/t_shirt_goods/2972/)
- CustoMee (built on the Printio API): T-shirts **from ¥3,281 incl. tax** — [Printio case study](https://printio.me/casestudy/customee/)
- UP-T one-shirt consumer price **from ¥2,200** (2026-09) — [tshirt-kakaku](https://tshirt-kakaku.com/companies/up-t/)

### Inferences
- SUZURI's model implies the platform's gross margin is built into the cost price (POD cost about ¥1,500–2,000 against a ¥2,080–2,560 cost price), while the creator margin (トリブン) is added on top. **A new entrant buying POD at consumer prices (about ¥2,200) cannot match SUZURI's ¥2,816 incl.-tax floor and still pay royalties and ads.** It needs negotiated B2B print costs below about ¥1,600, or must price at ¥4,000–5,000 and justify the premium through the AI-design value.
- Royalty design options: a flat yen amount set by the creator (SUZURI style, default ¥300), or a percentage of retail (for example 10–20%, which is ¥440–880 on ¥4,400). A flat-yen floor plus a creator-set markup keeps the platform margin predictable.

### Gaps
- Official 2026 SUZURI cost-price table (suzuri.jp blocked) and the survey distribution of トリブン values were not retrieved.
- Royalty structures for Tシャツトリニティ, ClubT, UTme!/StyleHint, BOOTH (pixiv) and overseas comparators (Redbubble, Merch by Amazon) were not retrieved.
- Gross-margin benchmarks for Japanese original-tee D2C brands (for example from IR or financial statements of GMO Pepabo's SUZURI segment) were not retrieved.

---

## 8. Unit economics for one T-shirt order (synthesis for the P&L model)

### Takeaway
At a **¥4,400 tax-included price (¥4,000 excl. tax)**, an outsourced 1-unit POD order leaves **about ¥1,200 (30%) contribution before marketing** in the base case: print ¥2,000 excl. tax + shipping ¥218 + packaging ¥40 + payment ¥158 + AI ¥71 + upscale/moderation ¥10 + royalty ¥300. After paid acquisition at ROAS 400% the contribution falls to **about ¥150–250**. Viability depends on B2B print pricing, AI cost control per order and organic or repeat demand.

### Cited Findings
(Input figures cited in sections 1–7; restated with sources.)
- Print + body, 1 unit, white, full color: ¥2,200 incl. tax (UP-T, 2026-09) — [tshirt-kakaku](https://tshirt-kakaku.com/companies/up-t/); range ¥1,458 (TMIX) — [TMIX](https://tmix.jp/product_categories/1) to ¥2,772 (UP-T incl. shipping) — [review](https://originaltshirt-guide.com/up-t-review/)
- Shipping: Click Post ¥240 (2026-10) — [haruuu](https://haruuu.online/clickpost-neage-240en-2026/); Nekopos up to ¥385 — [stockcrew](https://stockcrew.co.jp/insights/delivery-yamato); Yu-Packet ¥360 — [stockcrew](https://stockcrew.co.jp/insights/article_jiji_japanpost_rate_2026-07-24)
- Card fee: Stripe 3.6% — [PAY.JP column](https://pay.jp/column/stripe-fees-guide); PAY.JP 3.3% — [PAY.JP help](https://help.pay.jp/ja/articles/5044185-%E6%9C%80%E9%81%A9%E3%81%AA%E3%83%97%E3%83%A9%E3%83%B3%E3%81%AE%E9%81%B8%E3%81%B3%E6%96%B9%E3%82%92%E6%95%99%E3%81%88%E3%81%A6%E3%81%8F%E3%81%A0%E3%81%95%E3%81%84)
- AI image: Nano Banana 2.1 1K $0.0336 / 4K $0.113 — [aifreeapi](https://www.aifreeapi.com/en/posts/nano-banana-2-1)
- Creator royalty reference: SUZURI default トリブン ¥300 — [imano-okiniiri](https://imano-okiniiri.com/suzuri-pricing-guide/)
- Ad efficiency: apparel Instagram ROAS 300–500% — [lifunext](https://lifunext.com/blog/sns/instagramad/sns-snsad-14/)
- Connect payout fees: ¥200 per month per active account + ¥250 + 0.25% per payout — [PAY.JP Connect guide](https://pay.jp/column/stripe-connect-guide)

### Inferences
**Per-order P&L, base case (¥, excluding consumption tax where applicable; price ¥4,400 incl. tax is an assumption)**

| Line | Low-cost case | **Base case** | High-cost case | Basis |
|---|---|---|---|---|
| Revenue (excl. tax) | 4,000 | **4,000** | 4,000 | assumed price ¥4,400 incl. tax |
| Print + body (outsourced, 1 unit) | −1,327 (TMIX ¥1,458 ÷ 1.1) | **−2,000** (UP-T ¥2,200 ÷ 1.1) | −2,520 (¥2,772 ÷ 1.1, shipping incl.; drop the shipping line) | §2 |
| Domestic shipping | −218 (Click Post ¥240 ÷ 1.1) | **−218** | 0 (included above) / −350 (Nekopos ¥385 ÷ 1.1) | §2 |
| Packaging | −30 | **−40** | −60 | assumption, not sourced |
| Card fee | −122 (PAY.JP Business 2.78% × 4,400) | **−158** (Stripe 3.6% × 4,400) | −158 | §4 |
| AI generation per order | −15 (FLUX + Recraft, one session) | **−71** (NB2.1 typical session, one session per order) | −1,250 (NB Pro, 5 sessions per order) | §1 |
| Upscale / background removal / moderation | −1 | **−10** | −40 | §1 |
| Creator royalty (marketplace orders only) | 0 (own design) | **−300** | −880 (20% of retail) | §7 |
| **Contribution before marketing** | **≈ +2,287** | **≈ +1,203 (30%)** | **≈ −908** | |
| Paid acquisition (if ad-driven, ROAS 300–500%) | −800 (20%) | −1,000 (ROAS 400% on ¥4,000) | −1,333 (ROAS 300%) | §6 |
| **Contribution after ads** | ≈ +1,487 | **≈ +203** | ≈ −2,241 | |

- Not included per order: Connect payout overhead (about ¥450–525 per creator per month, best amortized with payout thresholds); CS, refunds and reprints (misprint rate unknown, a gap); fixed infrastructure (¥14–48k per month); back-office (about ¥25–50k per month).
- **Key sensitivities, in order:** (1) the print cost (a ¥500 B2B reduction adds about 12 points of margin); (2) whether orders are ad-acquired; (3) AI sessions per order, especially when using Pro-class models; (4) royalty level on marketplace sales.
- **Fixed-cost break-even (illustrative):** monthly fixed costs of about ¥100k (infra ¥30k + accounting ¥30k + tools and misc ¥40k, excluding salaries and development amortization) ÷ about ¥1,200 base contribution before ads ≈ **85 organic orders per month**. If a ¥10M development cost is amortized over 36 months (about ¥278k per month), break-even rises to **about 315 organic orders per month**.
- **Japanese consumption tax:** the model above nets 10% JCT out of revenue and out of domestic taxable costs (POD, shipping). Card-processing fees and overseas API charges are left gross. Their JCT treatment (non-taxable financial service vs. reverse charge) needs an accountant's confirmation.

### Gaps
- No real data on sessions-per-order conversion, average designs generated per user or repeat-purchase rates for AI-design apparel services. All are assumptions.
- No misprint, reprint or return-rate data, and no CS cost per order.
- No negotiated B2B POD prices, which are the largest single lever.
- The search quota was exhausted before several planned look-ups. A follow-up session could cover: GMO-PG fees, bank transfer fees, Tokyo minimum wage 2026, CS outsourcing rates, DTF printer and consumable prices in Japan, Gelato and Printful Japan base prices, SUZURI official prices, Topaz/Clipdrop API, Cloudflare R2 and Cloud Run.
