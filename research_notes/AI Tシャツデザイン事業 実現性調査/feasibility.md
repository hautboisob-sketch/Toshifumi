# 生成AIでTシャツをデザイン→注文→マーケットプレイス（ロイヤリティ）型サービスの技術・事業実現性（日本市場, 2026年10月時点）

> 調査メモ（2026-10-15）：WebFetch が多数のドメイン（docs.cloud.google.com, ai.google.dev, platform.openai.com, gelato.com, suzuri.jp, printful.com, nri.com 等）で DNS 解決エラーとなり本文を直接読めなかったため、以下の引用は主に Web 検索結果のスニペット／要約に基づく。一次資料（Google/OpenAI 公式 docs、各社規約）の本文は未精読であり、数値は実装前に公式ページで再確認が必要。第三者（API 再販業者・ベンダーブログ）由来の数値はその旨を明記した。

## Q1. 技術パイプライン：AI画像 → 印刷可能データ（解像度・透過・色・文字・ベクター化・DTF/DTG特有の問題）

### Takeaway
2026年時点の Google 系モデル（Nano Banana Pro = Gemini 3 Pro Image、Nano Banana 2 = Gemini 3.1 Flash Image）は 4K（1:1 で 4096×4096）出力に対応しており、A4〜ほぼA3 300dpi相当の画素数をネイティブに出せるため「解像度」は2023–24年ほどのボトルネックではなくなった。残る本質的な壁は (1) Gemini/gpt-image-2 系で透過背景がネイティブに出ない（背景除去・差分マット処理が必須）、(2) 半透明・グラデーション・ソフトエッジが DTF/DTG の白インク下地で破綻しやすい（ハーフトーン化等の前処理が必要）、(3) 日本語文字の描画は改善したが誤字ゼロ保証はない、の3点で、いずれも自動前処理パイプライン＋プレビュー確認で吸収可能な「エンジニアリング課題」である。

### Cited Findings
**必要解像度（目安）**
- 一般的なカラー印刷は 350dpi 以上が目安とされる。あるTシャツ専門店はAI画像入稿に「約 3500 × 4900 ピクセル以上、300dpi」を指定（店舗ごとの要件）— [Web検索要約（入稿データ解像度）: Printful JP blog「完璧なプリントファイルの作り方」](https://www.printful.com/jp/blog/how-to-create-perfect-printfile) / [Epson FAQ: 推奨解像度 300–360dpi、視距離で変わる](https://www.epson.eu/en_EU/faq/KA-01467)
- 第三者ガイドでは、標準的な前面プリント範囲 約12×14インチ × 300dpi = 3600×4200px が最低ライン、あるいは「多くのアパレルで 4000×4000px 以上」が目標とされる（第三者情報）— [deep-image.ai: AI Image Generation for POD / DTG Resolution Gap](https://deep-image.ai/blog/ai-image-generation-print-on-demand-dtg-resolution/); [Cliprise: AI T-shirt design workflow](https://www.cliprise.app/learn/workflows/ecommerce/ai-tshirt-design-workflow-upload-ready-file)
- SUZURI の管理画面からの作成では長辺 2000px 以上推奨、アップロード上限 4MB（API でも同じかは未確認）— [clawindex: SUZURI API skill（第三者）](https://clawindex.app/skills/suzuri-api-pepabo-pepabo-marketplace)

**モデルのネイティブ出力解像度（2026）**
- Nano Banana Pro（Gemini 3 Pro Image）は 1K/2K/4K 出力に対応。Google Cloud docs では 4K 出力は GA エンドポイント上でも「Preview」扱い。アスペクト比は 1:1, 3:2, 2:3, 3:4, 4:3, 4:5, 5:4, 9:16, 16:9, 21:9。プレビューID `gemini-3-pro-image-preview` は 2026年6月25日に停止し `gemini-3-pro-image` に移行 — [Google Cloud: Gemini 3 Pro Image (Nano Banana Pro)](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/models/gemini/3-pro-image?authuser=1)
- 4K 時の画素数は 1:1 で 4096×4096、16:9 で 5504×3072、21:9 で 6336×2688。4K は明示指定が必要で、未指定は 1K（第三者ガイド）。4K 単価 $0.24 — [laozhang.ai: Nano Banana Pro 4K guide（第三者）](https://blog.laozhang.ai/en/posts/nano-banana-pro-4k-guide.md)
- Nano Banana 2（Gemini 3.1 Flash Image）の Gemini API 価格：0.5K $0.045 / 1K $0.067 / 2K $0.101 / 4K $0.151（4096×4096）。Vertex 上では 4K 出力は Preview — [benchlm: Nano Banana API pricing（Google価格ページの転記, 2026-09-11 読取）](https://benchlm.ai/media-pricing/nano-banana); [DeepLearning.AI The Batch: Nano Banana 2](https://www.deeplearning.ai/the-batch/nano-banana-2-aka-gemini-3-1-flash-image-makes-edits-easier-and-faster); [Google Cloud: Gemini 3.1 Flash Image](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/models/gemini/3-1-flash-image?authuser=2)
- Imagen 4 Ultra（`imagen-4.0-ultra-generate-001`）は `sampleImageSize` で "1K"/"2K" を指定可能（デフォルト1K）。2048×2048 という画素数は第三者情報のみ — [Google Cloud: Set output resolution](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/image/set-output-resolution)
- Imagen のアップスケール API（`imagen-4.0-upscale-preview`）は x2/x3/x4、出力は 17MP 以下という上限（4096×4096 ≒16.8MP は可、2048²×4 は不可）— [Google Cloud: Imagen upscale API](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/model-reference/imagen-upscale-api); [Upscale images using Imagen](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/image/upscale-image)
- OpenAI gpt-image-1 / 1.5 は 1024×1024, 1536×1024, 1024×1536。gpt-image-1.5 の 1536×1024 は medium 約 $0.05 / high 約 $0.20（第三者価格表）— [Runware: gpt-image-1 docs](https://runware.ai/docs/models/openai-gpt-image-1.md); [Segmind: GPT Image 1.5](https://www.segmind.com/models/gpt-image-1.5)
- gpt-image-2 は 2026年4月21日リリース。最大解像度は「4K（例 3840×2160、各辺16の倍数）」とする情報と「ネイティブ2K＋任意で4Kアップスケール」とする情報が対立（いずれも第三者）— [fal.ai: What is GPT Image 2](https://fal.ai/learn/tools/what-is-gpt-image-2); [NYU Shanghai RITS: ChatGPT Images 2.0 with 2K output](https://rits.shanghai.nyu.edu/ai/openai-launches-chatgpt-images-2-0-with-2k-output-and-reasoning-mode/); [apiyi: gpt-image-2 launch](https://docs.apiyi.com/en/news/gpt-image-2-launch)
- Printful の AI Image Generator は 1回 1024×1024 生成で、注文時に「自動でアップスケールして生産要件を満たす」と公式ヘルプに記載（=大手PODはアップスケール前提で運用）— [Printful Help: AI Image Generator](https://support.printful.com/hc/en-us/articles/41396543812625)

**透過背景**
- Nano Banana Pro / Nano Banana 2 はネイティブ透過背景生成に非対応と報告（第三者）。gpt-image-2 系も同様に非対応と報告 — [APIYI help（第三者）](https://help.apiyi.com/en?p=30331); [APIYI: gpt-image-2 transparent background not supported](https://help.apiyi.com/en/gpt-image-2-transparent-background-not-supported-en.html)
- 回避策：白背景で生成→同一画像を黒背景に編集→差分からアルファを計算する「2パス差分マット」がコミュニティで実装されている — [Replicate: jide/nano-banana-2-transparent](https://replicate.com/jide/nano-banana-2-transparent/readme); [Google AI Dev Forum: How can I make the background transparent?](https://discuss.ai.google.dev/t/how-can-i-make-the-background-transparent/111698)
- gpt-image-1 系は `background: transparent` パラメータがあり、PNG/WebP 出力のみ・quality medium/high 推奨。パラメータ指定だけでは透過にならずプロンプトでも指示が必要だった事例あり — [OpenAI Community: transparent background](https://community.openai.com/t/how-to-output-a-background-transparent-image-using-the-edit-or-generate-interface/1349936); [imagine.art: GPT Image 1.5 features](https://www.imagine.art/blogs/gpt-image-1-5-features)
- AI出力は「偽の市松模様（グレー/白チェッカー）背景」になりがち、低解像度も典型的問題 — [KolorMatrix: ChatGPT & AI Artwork for DTF Printing](https://kolormatrix.com/pages/chatgpt-ai-artwork-for-dtf-printing)

**DTF/DTG 特有の印刷品質問題**
- プリンタは不透明なドットしか打てず、半透明ピクセルの扱いは RIP 任せで予測不能。DTF では白インク下地が半透明部・フェード部にも入り、白いもや・四角い縁（スペックル状の矩形）が出る。極小ドットは接着パウダーが付かず転写不良になる — [Transfer Superstars: Fixing Common DTF Printing Problems](https://www.transfersuperstars.com/blogs/dtf-graphic-design/fixing-common-dtf-printing-problems-photoshop-vs-ai); [Inkthreadable: Gradients & transparency](https://help.inkthreadable.co.uk/en/articles/435627-gradients-transparency)
- 対策はハーフトーン化（グラデ・影・フェード部のみ網点化、重要ディテールはソリッド維持）。ただし細い文字・ディテールの潰れ、低解像度AI画像での網点の目立ちが副作用。例として「グレースケール化→カーブ調整→35線・22°スクリーン」等のワークフロー（ベンダー例、標準ではない）— [Transfer Superstars: AI art → breathable DTF prints with halftones & knockout](https://www.transfersuperstars.com/blogs/dtf-graphic-design/transform-ai-art-to-breathable-dtf-prints-with-halftones-knockout); [Printed Mint: halftones simulating transparency](https://printedmint.com/blog/drop-shipping-business-tips-5/mastering-halftones-simulating-transparency-in-dtf-t-shirt-printing-985); [DTF Dallas: What is a DTF halftone](https://dtfdallas.com/blogs/news/what-is-a-dtf-halftone)
- DTF は PET フィルムに CMYK＋白で印刷し、白は濃色生地上で色を発色させる下地 — [Atlanta Vinyl: White ink in DTF](https://atlantavinylstore.com/blogs/news/what-is-white-ink-in-dtf-printing-and-why-does-it-matter)
- 大手PODの DTG は sRGB 前提とする第三者ガイドあり（CMYK 変換は不要という含意）— [Cliprise: AI T-shirt design workflow（第三者）](https://www.cliprise.app/learn/workflows/ecommerce/ai-tshirt-design-workflow-upload-ready-file)
- 汎用画像生成AIはスクリーン印刷に向かない写実画を出しがちで、Tシャツ特化ツールは輪郭・色数制約に対応しやすいとの解説 — [Framia: AI Tシャツデザインジェネレーター](https://framia.converge.ai/page/ja-JP/blog/ai-tshatu-dezain-jenereta)

**文字描画（日本語）**
- 週刊アスキー：初代 Nano Banana では日本語や多言語の看板・UI文字が支離滅裂になりがちだったが、Pro では細かい誤りは残るものの「実際に読める文章」として生成されるケースが一気に増えた — [週刊アスキー](https://weekly.ascii.jp/elem/000/004/354/4354220/3/)
- AIPICKS は Nano Banana Pro について「日本語業務に部分的にしか使えない」と推定評価（公開情報＋コミュニティからの推測と明記）— [AIPICKS: Nano Banana Pro 日本語対応](https://aipicks.jp/tool/nano-banana-pro/japanese)
- gpt-image-2 は日本語含む複数文字体系で >95% の文字描画精度を OpenAI が主張と報じられる（第三者経由、独立検証なし）— [fal.ai: GPT Image 2 review](https://fal.ai/learn/tools/gpt-image-2-review); [tosea.ai: GPT Image 2 guide](https://tosea.ai/blog/gpt-image-2-complete-guide)

**ベクター化**
- Recraft V3 には SVG 出力専用バリアント（ロゴ・アイコン・イラスト向け）があり、Tシャツデザインが主用途の一つとして紹介される（「唯一の真のベクター生成AI」という主張は未検証）— [Segmind: Recraft V3 SVG](https://segmind.com/models/recraft-v3-svg); [cutout.pro: Recraft guide](https://www.cutout.pro/learn/recraft-ai)

### Inferences
- 画素数計算（自前）：A4 300dpi ≒ 2480×3508px、A3 300dpi ≒ 3508×4961px、A3 350dpi ≒ 4093×5787px。Nano Banana Pro/2 の 4K（1:1 で 4096²、縦長比率なら長辺 4800px 前後と推定）は A4 は余裕、A3 300dpi にほぼ届く水準。2K 生成＋Imagen x2 アップスケール（17MP 上限内）でも同等が得られる。→ 解像度は「コスト（4K $0.15–0.24/枚）と生成待ち時間」の問題に変わった。
- 生成コスト試算：試行錯誤で1注文あたり 1K を10回＋最終4K 1回生成すると Nano Banana 2 で約 $0.67+$0.15 ≒ $0.8（≒120円）程度。Tシャツ1枚の粗利（後述）に対して許容範囲だが、無料ユーザーの生成乱用対策（回数制限・課金）が必要。
- 推奨パイプライン：①プロンプト補助LLM→②1K で複数案生成（低コスト）→③選択案を対話編集→④4K 再生成 or 2K＋アップスケール→⑤背景除去（差分マット or セグメンテーションモデル）→⑥アルファのしきい値処理（半透明除去）＋グラデ部ハーフトーン化→⑦濃色ボディ用白版プレビュー→⑧ sRGB PNG で POD へ入稿。⑤⑥が品質事故の主因になるため、自動処理＋「プリント後イメージ」プレビュー（半透明を潰した状態）で顧客に事前承認させる設計が重要。
- 日本語の「文字入りデザイン」（チーム名・推しの名前・文化祭スローガン）は需要の中心になり得るが、生成モデルに文字を描かせるより、生成画像＋フォントでのテキスト合成（エディタ側でレイヤー合成）にする方が誤字リスク・著作権（フォントライセンス）管理の面で安全。

### Gaps
- Google 公式ドキュメント本文（各アスペクト比の 4K 画素数表、文字描画の公式説明、SynthID 透かしの印刷物への影響）を直接精読できなかった（DNS エラー）。
- 日本語文字描画の誤字率に関する独立した定量ベンチマークは見つからなかった。
- Real-ESRGAN / Topaz 等の汎用アップスケーラーと Imagen upscale の印刷品質比較、Vectorizer.AI の品質・料金に関する信頼できる情報は得られなかった。
- 日本の印刷所（UP-T, オリジナルプリント.jp 等）の DTF/DTG 入稿ガイド（RGB/CMYK、白版、半透明の扱い）の一次情報は取得できず。

## Q2. プロダクト/UX：モックアップ、プロンプト補助、反復編集、非デザイナーの到達度、支払意欲のエビデンス

### Takeaway
モックアップ生成（Printful v2 Mockup API、Printify AI mockups）や「プロンプト→生成→商品化」の UX は既に大手PODが標準機能化しており（Printify は 2024年8月から OpenAI 搭載、1日15回無料）、AI生成そのものは差別化要因にならない。一方、消費者調査は「AI生成と明示すると知覚品質・支払意欲が下がる」傾向を示しており、"AI製" を前面に出すより「自分（たち）だけのデザインが簡単に作れる」体験価値で売るべきというシグナルがある。

### Cited Findings
- Printful Mockup Generator v2：`POST /v2/mockup-tasks` で非同期タスク作成→GET でポーリング。生成は通常10–30秒（第三者ラッパー記載）。旧 API（task_key 方式）は空レスポンス報告あり — [apis.io: Printful Mockup Generator API](https://apis.io/apis/printful/printful-mockup-generator-api/); [glama.ai: printful_create_mockup_task](https://glama.ai/mcp/servers/Purple-Horizons/printful-mcp/tools/printful_create_mockup_task); [Zeroqode forum](https://forum.zeroqode.com/t/printfuls-mockup-api-not-working-on-bubble/2764)
- 汎用モックアップAPI も存在（Printess, Mediamodifier）— [Printess Mockup API](https://www.printess.com/kb/api-reference/mockup-api.html); [Mediamodifier Mockup API](https://prelive.mediamodifier.com/mockup-api)
- Printify の AI Image Generator は 2024年8月から OpenAI 搭載、Product Creator 内でテキスト→画像、1日15回無料。別途 AI モックアップ機能を段階展開 — [Printify blog: AI image generator（2024-11-01）](https://printify.com/blog/ai-image-generator/); [Printify Help: AI mockups](https://help.printify.com/hc/en-us/articles/43679503506321-How-can-I-create-AI-mockups-on-Printify)
- Printful の AI Image Generator も1日15回無料、1024×1024、背景除去はオプション、注文時に自動アップスケール — [Printful Help](https://support.printful.com/hc/en-us/articles/41396543812625)
- 米 Rush Order Tees は無料 AI Design Wizard（企業・学校・チーム向け、プロンプトガイド、スタイル提案：ヴィンテージ/ミニマル/カートゥーン等）を公開 — [Impressions Magazine](https://impressionsmagazine.com/?p=167327)
- テキスト→AI画像→そのままTシャツ注文のサービス例：Inktee AI（creati.ai 掲載 2024年6月）、Holover（複数案生成・注文前プレビュー/修正）— [creati.ai: Inktee AI](https://creati.ai/ja/ai-tools/inktee-ai/); [HuntScreens: Holover](https://huntscreens.com/products/holover)
- SUZURI は 2023年9月に Stability AI Japan と連携し、AI画像をそのままグッズ化する「スリスリAIラボ」を開始 — [FASHIONSNAP: SUZURI × Stable Diffusion](https://www.fashionsnap.com/article/2023-09-09/suzuri-stablediffusion)
- 支払意欲：266人の実験調査（修士論文）で、作者を伏せると AI 製品は人間デザインと同等以上に評価されたが、AI関与の開示は知覚品質・WTPを総じて下げた。ファストファッションは AI 適合的、ラグジュアリーは人間の創造性と結びつく — [Univ. Bologna AMS Laurea: Nava](https://amslaurea.unibo.it/id/eprint/36888/)
- 米国505人調査（2024 Auburn 博士論文）：AIデザインのアパレルへの知覚は中立〜やや否定的、機能面が最も好意的 — [Auburn ETD](https://etd.auburn.edu/handle/10415/9439)
- 製品説明に「AI」を使うと感情的信頼の低下を介して購入意向が下がるとする米国1,000人超調査（二次情報、未検証）— [scribe.disroot.org repost](https://scribe.disroot.org/post/684496)

### Inferences
- 非デザイナーの到達度：Gemini 系の対話編集（多ターン編集）と LLM プロンプト補助を組み合わせれば、「コンセプト→ラフ→修正」までは非デザイナーでも到達可能。ただし印刷事故（半透明・細線・文字誤り）を見抜けないので、システム側の自動検査と「印刷後見え方プレビュー」が必須。
- 日本語 UX（日本語プロンプト→英語内部プロンプト変換、推し活/部活/文化祭テンプレート、色指定のプリセット、ボディ色に応じた自動白版）が国内での差別化余地。
- "AI" を売り文句にするより、「テンプレ＋AIで自分たちだけのデザイン」「1枚から・数日で届く」「チームで投票して決める」などの体験・利便性訴求の方がエビデンスと整合的。

### Gaps
- 日本の消費者を対象にした「AI生成デザインのTシャツ」への支払意欲調査は見つからなかった。
- Printify/Printful の AI 生成機能の利用率・CVR に関する公開データは見つからなかった。
- 3D プレビュー（Three.js 等）の導入効果に関する定量データは未調査。

## Q3. オペレーションモデル：国内POD/OEM連携 vs グローバルPOD vs 自社印刷

### Takeaway
最も現実的な立ち上げ方は「API で画像入稿→受注生産→発送まで委託できる国内/国内拠点PODとの連携」で、候補は SUZURI API（クリエイター商品化・トリブン方式）、イメージ・マジック（オリジナルプリント.jp）の画像入稿API、Printio(OpenFactory)、Gelato（日本ローカル生産を公称、平均2–3日配送）、Printful（2020年に日本メーカーと提携、DTG）。UP-T（丸井織物）は公開APIが確認できず個別交渉案件。自社印刷は在庫・設備・QCの負担が大きく、需要検証後の選択肢。

### Cited Findings
**(a) 国内POD/OEM**
- SUZURI API：v1、OAuth（クライアントID/シークレット）、material（画像素材）の一覧/作成/更新/削除、products 取得等に対応（第三者クライアント/スキル紹介より）。「トリブン」はクリエイターが商品作成時に設定する取り分で、売れると振り込まれる — [GitHub: suzuri-actionscript-client](https://github.com/hadashiA/suzuri-actionscript-client); [clawindex: SUZURI API skill](https://clawindex.app/skills/suzuri-api-pepabo-pepabo-marketplace)
- SUZURI は GMOペパボ運営、画像アップロードでグッズ作成・販売、6カテゴリ47種類、Tシャツが最人気。2024年7月に刺しゅうTシャツ（税込3,795円〜）開始 — [commercepick](https://www.commercepick.com/archives/7364); [GMO ニュース PDF](https://group.gmo/pdf/news/gmo_news_9068.pdf)
- SUZURI は規約違反チェックで、テキスト・デザインを違反基準入りプロンプトで LLM 判定する仕組みを紹介 — [Speaker Deck（ペパボ発表資料）](https://speakerdeck.com/player/2e149725097e4d03899e91eed8ddc1d5)
- SUZURI 事業の売上：2025年12月期2Q累計 402百万円（前年同期 382百万円、105.0%）。生産体制改善で出荷日数短縮 — [logmi Finance: GMOペパボ 2025年2Q 決算説明](https://finance.logmi.jp/articles/382468); [JPX 開示資料](https://www2.jpx.co.jp/disc/36330/140120250807534214.pdf)
- イメージ・マジック（オリジナルプリント.jp）は「画像入稿API接続」によるオンデマンドEC型グッズ販売の仕組みを提供。完全受注生産で在庫リスクなし、CS と決済（カート）はイメージ・マジック側が担当。初期費用 通常10万円（税別）を実質0円にするキャンペーン（**2016年頃のプレスリリースで古い情報**）。Shopify アプリもあり — [DreamNews プレスリリース](https://www.dreamnews.jp/index.php/press/0000130274)
- Printio（OpenFactory）も1点からの受注生産を API で自動化できるサービスとして紹介 — [kepple: 記事](https://kepple.co.jp/articles/inwc_s6-f1h)
- UP-T は丸井織物運営、石川県の自社工場で生産、1枚から注文可、法人・イベント対応。公開 API/Webhook のドキュメントは検索で見つからず — [WWDJAPAN](https://www.wwdjapan.com/articles/2104167); [App Store: UP-T](https://apps.apple.com/us/app/up-t-%E3%82%AA%E3%83%AA%E3%82%B8%E3%83%8A%E3%83%ABt%E3%82%B7%E3%83%A3%E3%83%84%E3%81%AE%E3%82%A2%E3%83%83%E3%83%97%E3%83%86%E3%82%A3%E3%83%BC/id1092418257?uo=4)
- 小ロット：最小5枚〜の業者や、1点から注文OKの業者が混在 — [Printful JP: クラスTシャツ](https://www.printful.com/jp/class-t-shirts); [PR TIMES](https://prtimes.jp/main/html/rd/p/000000059.000165428.html)

**(b) グローバルPOD**
- Gelato：32か国・130+（別ページでは140+）の生産パートナー網、日本をローカル生産国として掲載。「日本の顧客への平均配送 2–3日」と自社主張。Tシャツは DTG/DTF/刺しゅう。日本の具体的パートナー名は非公開 — [Gelato: Print on demand in Japan](https://gelato.com/print-on-demand/japan); [Gelato: Locally printed in Japan](https://www.gelato.com/print-on-demand/japan); [Gelato: T-shirts](https://www.gelato.com/ja/custom-t-shirts)
- 第三者レビュー：Gelato の標準注文は4–6日着、87%が同一国内で生産・配送。基本アパレルの品揃えは Printful より限定的、梱包品質のばらつき — [eCommerceCEO: Gelato review 2026](https://www.ecommerceceo.com/reviews/gelato/)
- Printful：2020年8月に日本の製造会社と提携し国内フルフィルメント開始。対象は DTG アパレル6品目、国内配送1–3営業日（従来は海外拠点から 8–30営業日）。**2020年の古い情報で現行カタログは未確認** — [Printful News](https://www.printful.com/news/printful-partners-with-leading-japanese-manufacturing-company)
- Printful 日本語サイトはクラスTシャツを「1点から注文OK」「注文後最短3日でお手元に」と訴求 — [Printful JP: クラスTシャツ](https://www.printful.com/jp/class-t-shirts)
- Printful のフルフィルメント平均は全商品 2–5営業日（一般値）— [Printful Shipping](https://printful.com/shipping)

### Inferences
- モデル比較（推論）：
  - SUZURI API：マーケットプレイス＆決済・CS・生産をSUZURI側に寄せられるため最速で立ち上がるが、顧客接点・価格決定・データがSUZURIに帰属しやすく、「運営者が注文ごとに収益化」する設計は SUZURI のトリブン（＝上乗せ額）を運営者が受け取る形に限定されやすい。デザイナーへのロイヤリティ分配は自前精算が必要。
  - イメージ・マジック/Printio 型（ホワイトラベルAPI）：自社EC・自社決済で価格とマージンを自分で決められる。運営者の収益＝販売価格 −（ボディ＋加工＋送料）。団体・クラT 向けの多枚数割引も設計しやすい。
  - Gelato/Printful：API・モックアップ・ドキュメントは成熟。日本ローカル生産を公称するが、日本で生産可能なボディ/色/加工の範囲、日本語CS、国内の返品対応は要確認。
  - 自社印刷：DTF 機材は比較的低価格化しているが、プレス・検品・梱包・CS人員が必要で、繁忙期（文化祭シーズン）の需要の波を吸収しにくい。需要検証後の垂直統合オプション。
- 返品・QC：受注生産のため「イメージ違い」は原則返品不可が業界慣行と推測されるが、AI特有の印刷事故（半透明・誤字）は運営側の前処理責任になりやすく、無償再印刷の原資（売上の数%）を見込むべき。

### Gaps
- SUZURI API の現行公式仕様（materials の画像指定方法、注文/チェックアウトAPI有無、第三者サービスによる商用利用条件）は公式ページにアクセスできず未確認。
- SUZURI スタンダードTシャツのベース価格、UP-T・オリジナルプリント.jp の1枚単価（2026年）は確認できなかった（ユニットエコノミクス算定の必須データ）。
- Gelato の日本パートナー名、日本で生産可能なアパレル品目、DTF/DTG の別、日本語サポート範囲は不明。
- Printful 日本拠点の現行対象品目・稼働状況（2020年以降の更新）は不明。
- TMIX 法人向けの API 有無は調べきれなかった。

## Q4. 需要シグナルと市場：オリジナルTシャツ需要、推し活、季節性、ターゲット

### Takeaway
推し活市場は推計3.5–3.8兆円規模（推計主体で差大）、推し活人口は約1,384万人とされ「オリジナルグッズ」需要の母集団は大きい。ただし推し活グッズの入手は公式物販が71%と主で、1か月の推し活支出は3,000円未満が最多層（39.3%）という調査もあり、単価2,500–4,000円のTシャツは「頻繁に買うもの」ではない。国内最大級のSUZURI事業の売上は年8億円規模・前年比+5%程度で、オリジナルグッズPOD市場は成熟〜緩成長と読める。クラスT/部活/イベントなど「団体・多枚数・期限付き」需要が最も確度の高いセグメント。

### Cited Findings
- NRI：推し活市場は 2025年3月時点で約3.8兆円と推計 — [NRI コラム（2025-08-29）](https://www.nri.com/jp/media/column/mcs_blog/20250829.html)
- Oshicoco・CDG の推し活実態アンケート（2025年1月）：推し活人口 1,384万人、市場規模 3兆5千億円 — [koukoku.jp まとめ](https://www.koukoku.jp/service/suketto/marketer/marketing/%E3%80%902025%E5%B9%B4%E6%9C%80%E6%96%B0%E3%80%91%E6%8E%A8%E3%81%97%E6%B4%BB%E5%B8%82%E5%A0%B4%E3%81%AE%E4%BB%8A%E3%81%8C%E3%82%8F%E3%81%8B%E3%82%8B%EF%BC%81%E5%B8%82%E5%A0%B4%E8%A6%8F%E6%A8%A1/)
- 矢野経済研究所「オタク」市場：2021年 約7,000億円→2022年 約8,000億円。財務省資料の表では「オタク」主要16分野合計が2024年度 10,090億円 — [財務省「ファイナンス」2025年11月号 PDF](https://www.mof.go.jp/public_relations/finance/2025011/202511f.pdf); [クロス・マーケティング コラム](https://www.cross-m.co.jp/column/marketing/mkc20241025)
- タイトー調査（2025年10月、10–30代500人）：推しグッズ入手は公式ショップ・物販 71%、小売・EC・フリマ 45% — [ネットショップ担当者フォーラム](https://netshop.impress.co.jp/node/15015)
- トイズキング調査（2025年1月、10–50代）：推しのジャンルは男性アイドル34.1%、アニメ・漫画28.5%、女性アイドル22.5%。推し活支出は月3,000円未満が39.3%で最多 — [マイナビニュース（2026-03-23）](https://news.mynavi.jp/article/20260323-4233123/)
- ヒロモリ調査（2024年7月）：推し活経験者の約8割が商品・グッズを購入 — [commercepick](https://www.commercepick.com/archives/55021)
- SUZURI 事業売上 2025年2Q累計 402百万円（前年同期比105.0%）— [logmi Finance](https://finance.logmi.jp/articles/382468)
- クラスTシャツは文化祭・学園祭のほか体育祭・合唱コンクール・卒アル撮影でも使用され「需要は近年益々高まっている」（業者PR、統計なし）— [PR TIMES](https://prtimes.jp/main/html/rd/p/000000059.000165428.html); [ココナラマガジン](https://coconala.com/magazine/2227)
- 推し活グッズを手作りする需要の存在（手作り解説コンテンツ）— [CAD研究所: 推し活グッズ手作り](https://cad-kenkyujo.com/oshikatsu-goods-tedukuri/)

### Inferences
- ターゲット優先度（推論）：
  1. 団体・イベント（クラT、部活、サークル、社内イベント、草野球/フットサル、推し活オフ会）：期限・人数が明確で、1注文あたり10–40枚になり得る。AIは「デザイナーがいない団体が短時間で案を出し、LINE等で投票して決める」用途に刺さる。季節性は文化祭（6–7月・9–11月）、体育祭（5–6月・9–10月）、卒業（2–3月）に偏ると推定（統計未確認）。
  2. 推し活個人：「推しカラー」「推しの名前」「参戦日」入りなど、文字中心・非公式要素の自作需要。ただし公式キャラ・実在人物の肖像を使うと著作権・肖像権/パブリシティ権侵害リスク（Q5参照）。
  3. 小規模ブランド・インフルエンサー：既存の SUZURI/BASE/Printful 連携が強く、差別化困難。
- 推し活市場の「3.5–3.8兆円」はチケット・遠征費等を含む広義推計で、オリジナルTシャツに回る金額はその一部にすぎない点に注意（推計）。

### Gaps
- Google トレンド（「オリジナルTシャツ」「AI Tシャツ」「クラT」）の時系列は取得できなかった（トレンド画面へのアクセス不可）。季節性は推定にとどまる。
- 国内オリジナルTシャツ（プリント加工）市場規模の公的/調査会社データは見つからなかった。
- 推し活グッズのうち「自作/オーダー」比率の調査データは見つからなかった。

## Q5. マーケットプレイスの力学：鶏と卵、発見性、AIデザイン過剰供給、キュレーション、ロイヤリティ価値の持続性

### Takeaway
「誰でも似た画像を生成できる」環境では、AI生成デザインそのものの希少価値は低く、ロイヤリティ型マーケットは過剰供給・模倣・権利侵害の管理コストに押される。実例として、Redbubble（Articore）のマーケットプレイス売上はFY25に前年比-10%（FY22比-21%）、国内では BOOTH が2023年にAI依拠性の高い出品を検索除外し、2025年7月には大量出品・依拠性の高い出品に対しアカウント停止を含む措置へ強化、pixiv は2023年にリクエスト/FANBOX でAI作品を禁止した。マーケットプレイスは主事業ではなく「作成者の注文を補完する発見機能」として小さく始め、キュレーション（人手＋LLM審査）と重複検知を前提にすべき。

### Cited Findings
- Articore（Redbubble/TeePublic）FY25（2025年6月期）：マーケットプレイス売上 約A$379.1M、前年 A$423.1M から -10%、Redbubble の減少が TeePublic の成長を相殺。FY22 の A$482.6M 比 -21%。純損失 A$11.3M（FY24 A$8.8M）、粗利率 45.6%（+270bp）— [Capital Brief](https://www.capitalbrief.com/briefing/articore-group-posts-28-steeper-loss-in-fy25-13deb9ab-1319-44a0-abdf-26acff75b56d/); [Power Retail](https://powerretail.com.au/articore-ends-fy25-with-momentum-despite-annual-loss/); [Eightx teardown](https://eightx.co/blog/asx-teardown-redbubble-2026); [ASX 開示 PDF](https://announcements.asx.com.au/asxpdf/20250815/pdf/06mxp2802r3t9b.pdf)
- Etsy は AI アートを禁止せず開示（ラベル）を要求する方針と報道。Tom's Guide は「低品質アートの大量生産から作家・購入者を守る効果は乏しい」と評価 — [Tom's Guide](https://www.tomsguide.com/ai/ai-image-video/etsy-will-let-you-sell-ai-art-on-its-platform-but-theres-a-catch); [ShirtStorm](https://theshirtstorm.beehiiv.com/p/will-etsy-ban-ai-designs-how-to-thrive-no-matter-the-outcome)
- 「Etsy/Redbubble の新規出品の12–15%がAI生成（2023年は3%未満）」という数字は販売者向けサイトの主張で一次ソース未確認 — [ecommerce-platforms.com](https://ecommerce-platforms.com/articles/print-on-demand-design-trends)
- Redbubble で600超のAIデザインをアップロードし、トラフィック減少下でも収益は出たという販売者の体験談（逸話的）— [The Design Nexus](https://thedesignnexus.beehiiv.com/p/something-strange-happened-redbubble-shop)
- BOOTH：2023年5月、特定作品・作家への依拠性が高いAI生成作品を出品するショップの全商品を検索結果から除外（基準は非公開）— [Impress Watch](https://www.watch.impress.co.jp/docs/news/1501164.html)
- BOOTH：2025年7月18日、「依拠性が高い商品の出品」「大量・連続的な出品」の監視を強化し、商品非公開・ショップ強制非公開・アカウント停止を段階的/即時に実施。従来策では改善に至らなかったため。類似・パターン化商品の大量出品による検索妨害も問題視 — [GameBusiness.jp](https://www.gamebusiness.jp/article/2025/07/18/24681.html); [電ファミニコゲーマー](https://news.denfaminicogamer.jp/news/2507182c); [Impress Watch](https://www.watch.impress.co.jp/docs/news/2032504.html)
- pixiv：2023年にリクエスト・FANBOX で AI 作品の取り扱いを禁止 — [Impress Watch](https://www.watch.impress.co.jp/docs/news/1504986.html)
- 著作権：文化庁の整理では作風・画風・アイデアは保護対象外。侵害は「類似性」と「依拠性」の両方で判断。学習段階は著作権法30条の4、生成・利用段階は通常の侵害判断 — [東京大学 生成AI教材 PDF](https://genai.t.u-tokyo.ac.jp/assets/materials/01_03_生成AIの注意点・リスク_03_生成AIと著作権.pdf); [LegalOn: 生成AIと著作権](https://www.legalontech.com/jp/media/copyright-of-generative-ai)
- 米国著作権局の現行ガイダンスでは、人間による意味ある編集を伴わない純粋なAI生成画像は一般に著作権登録対象外 — [Framia: AIアートは売れるか（二次）](https://framia.converge.ai/page/ja-JP/blog/can-you-sell-ai-generated-art)

### Inferences
- ロイヤリティの持続性：純AI生成画像は著作権保護が弱い（少なくとも米国、日本でも創作的寄与が乏しいと著作物性が否定され得る）ため、マーケットで売れたデザインはプロンプト再現・スクショで容易に模倣される。デザイナーへのロイヤリティが「守れる資産」になりにくい。→ 価値の源泉を「画像」ではなく「テンプレート/プロンプトレシピ/シリーズ（ブランド）/コミュニティ（フォロワー）」に置く設計（例：テンプレ利用ごとに作成者へ報酬、リミックス時の自動クレジット）が現実的。
- 鶏と卵：初期は「自分用に作って注文する」シングルプレイヤー価値で集客し、生成物を公開するかを選ばせて供給を溜める（Canva/Kittl型）。需要側は団体注文の「デザイン案ギャラリー」（例：文化祭テンプレ人気順）から始めると発見性が担保しやすい。
- キュレーション：SUZURI の LLM 規約チェック事例のように、アップロード時のLLM＋画像類似度（既存キャラ/ロゴ/既出デザイン）判定、出品数レート制限、公開前の人手レビュー（初期）を組み込む必要。BOOTH の事例から、放置すると検索汚染→プラットフォームの価値毀損が起きる。
- 推し活×公式IPは最大の法的リスク（キャラ・ロゴ・実在アイドルの肖像）。「非公式グッズ作成ツール」と見なされるとプラットフォーム責任・炎上リスク。テキスト・カラー・抽象モチーフ中心のテンプレに限定するか、IPホルダーとの公式コラボ（SUZURI等の先例）で正規化するのが安全。

### Gaps
- Etsy・Redbubble の AI 出品比率・削除率に関する一次データは未確認（二次情報のみ）。
- AI生成デザイン限定のロイヤリティ型マーケットで成功している事例の売上・継続率データは見つからなかった。
- 日本における AI 生成物の著作物性に関する裁判例・文化庁「AIと著作権に関する考え方について」（2024）原文は未精読。

## Q6. AIネイティブ・マーチャンダイズ系スタートアップの成功/失敗事例と差別化アイデア

### Takeaway
AI Tシャツ生成ツールは2023–2026年に多数出現した（Inktee AI、Holover、Pollo AI、Rush Order Tees の AI Design Wizard、Printify/Printful の内蔵生成機能など）が、AIデザインTシャツ専業で明確にスケールした事例や詳細なポストモーテムは見つからなかった。大手POD/印刷会社がAI生成を「無料機能」として内製化した結果、独立系の差別化は「日本語UX・団体注文ワークフロー・推し活/学校行事テンプレ・LINE連携・品質保証」に移っている。

### Cited Findings
- AI専業ツール例：Inktee AI（テキスト→AI画像→Tシャツ注文、2024年6月掲載）、Holover（複数案生成・プレビュー・修正）、Pollo AI（印刷向け高解像度グラフィック）、WeShop AI（企業イベントTのモックアップ）— [creati.ai](https://creati.ai/ja/ai-tools/inktee-ai/); [HuntScreens](https://huntscreens.com/products/holover); [Pollo AI](https://pollo.ai/ja/ai-image-generator/shirt-design); [WeShop AI](https://www.weshop.ai/ja/tools/ai-t-shirt-design-generator)
- 既存印刷会社の取り込み：Rush Order Tees が無料 AI Design Wizard を公開（学校・チーム・企業向け）— [Impressions Magazine](https://impressionsmagazine.com/?p=167327)
- 大手PODの内製化：Printify（2024年8月〜OpenAI搭載、15回/日無料）、Printful（15回/日無料、1024²、自動アップスケール）— [Printify blog](https://printify.com/blog/ai-image-generator/); [Printful Help](https://support.printful.com/hc/en-us/articles/41396543812625)
- 国内大手の取り込み：SUZURI × Stability AI「スリスリAIラボ」（2023年9月）— [FASHIONSNAP](https://www.fashionsnap.com/article/2023-09-09/suzuri-stablediffusion)
- 失敗例（隣接領域）：Elevation Capital 出資のAIファッションスタイリスト Alle が事業終了。共同創業者は6回ピボットしたと述懐 — [Entrepreneur India](https://india.entrepreneur.com/?p=92526)
- 個人開発 Ai Shirtz：MVP にこだわれずローンチが約4週間遅れたと創業者が振り返り — [Indie Hackers: Ai Shirtz](https://www.indiehackers.com/product/ai-shirtz)
- 「デザイン生成自体は容易になり、優位性は売上に変える部分にある」との指摘（ガイド記事）— [mydesigns.io blog](https://mydesigns.io/blog/wp-json/wp/v2/posts/3366)

### Inferences
- 成功要因（推論）：
  1. 「AIで作れる」ではなく「団体の意思決定（案出し→投票→サイズ集計→一括決済→期日納品）」を丸ごと解決する。LINE グループでの共有・投票・個別サイズ回収・割り勘決済は日本の学校/部活/サークル文脈で強い差別化になり得る（LINEミニアプリ等の実装詳細は未調査）。
  2. 印刷品質保証（AI特有の半透明・文字誤りの自動検出、印刷後プレビュー、事故時の無償再印刷）でPOD大手の「無料生成機能」と差をつける。
  3. 日本語プロンプト補助＋テンプレート（クラT定番構図、背番号/名前入れ、推しカラー配色、和柄、文字主体デザイン）で非デザイナーの成功率を上げる。
  4. 生成コスト（4K 1枚 $0.15–0.24）は注文時のみ発生させ、試作は1K低コストで回す。
  5. マーケットプレイスはテンプレ/レシピ単位の報酬にして「模倣容易性」問題を回避。
- 失敗要因（推論）：差別化のないAI生成ツール（大手が無料提供）、マーケット先行での鶏と卵、IP侵害出品の放置による炎上・決済代行/プラットフォーム停止、印刷事故による返金コスト、文化祭シーズン集中による繁閑差。
- 事業性の判断（推論）：技術的には2026年時点で十分構築可能（既製API＋前処理パイプライン）。事業としては「AIデザインのマーケットプレイス」単体はRedbubble等の減速とAI過剰供給から見て弱く、「団体・イベント向けの AI 支援オリジナルTシャツ注文サービス」を主軸に、マーケットは補助機能とするのが最も実現性が高い。

### Gaps
- AIネイティブなTシャツ/マーチャンダイズ専業スタートアップ（2023–2026）の資金調達・売上・閉鎖に関する具体的ポストモーテムは見つからなかった。
- 国内で生成AI×オリジナルTシャツ注文を掲げたサービスのプレスリリース（PR TIMES 等）は検索で見つからなかった（存在しないのか検索漏れかは不明）。
- LINE 連携（LINEミニアプリ、LINE Pay 後継の決済手段等）の現行仕様・事例は未調査。
- ユニットエコノミクス（日本のPOD卸価格、送料、決済手数料、ロイヤリティ率の相場）は一次データ不足で算定できていない。
