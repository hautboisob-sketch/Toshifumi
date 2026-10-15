# AI Tシャツ事業は原価と権利処理で決まる

Gemini などの画像生成AIでTシャツをデザインし、そのまま印刷注文でき、さらにユーザー同士でデザインを売り買いしてロイヤリティを還元する日本向けサービスは、**2026年時点の既製APIで技術的には十分作れる**。ただし事業として成り立つかは、印刷原価、集客費、権利処理の3点でほぼ決まる。(1) 「サイト内でAI生成→印刷注文→出品・ロイヤリティ」を一つのサービスで完結させる例は、国内外とも見つからなかった。一方で構成要素はすべて既存勢がそろえている。国内最大手のSUZURIは2026年6月、Claude・Gemini・ChatGPTと連携するMCPサーバーを公開し、外部AIで作った画像をそのまま出品してロイヤリティ（トリブン）を得る流れをすでに開いた。米国ではAmazonが同じ月に、AIデザインから注文までを無料で始めている。(2) 実装はできるが、外注の1枚印刷を消費者価格で仕入れると、税込4,400円のTシャツで手元に残る貢献利益は広告前で約1,200円、広告経由の注文では約200円にとどまる。(3) 最大のリスクは、印刷して出荷する運営者自身が、著作権・商標・パブリシティ権の侵害品の「製造・販売者」とみなされうることだ。加えて、Googleの生成物補償は商標の主張を対象外とし、Vertex AIの規約は18歳未満が利用しうるサービスでの生成AI利用を禁じている。この年齢規定は、クラスTシャツなど学生の需要を狙う戦略と正面からぶつかる。(4) 費用の目安は、MVP開発が約600万〜1,900万円、会社設立・商標・利用規約が約40万〜70万円、立ち上げ期の固定費が人件費を除いて月10万円前後。AI生成費は1デザインセッションあたり数十〜百数十円と小さい。最も現実的なのは、自分用・団体用の注文を事業の柱とし、マーケットは運営者が売主となるライセンス型で補助的に載せる形である。

## 一気通貫の競合はまだないが、SUZURIとAmazonが両側から迫っている

国内で最も近いのはSUZURIだ。販売価格は原価に「トリブン」（クリエイターの取り分。0〜5,000円を100円単位で設定）を上乗せして決まり、トリブンからは振込手数料しか引かれずにクリエイターへ支払われる ([SUZURIヘルプ](https://help.suzuri.jp/hc/ja/articles/360011006033))([SUZURIヘルプ 振込](https://help.suzuri.jp/hc/ja/articles/115007857247))。運営の利益は原価のほうに織り込まれている。クリエイター数は2026年に100万人を超えた ([koubo.jp](https://koubo.jp/article/62883))。一方で流通額は、確認できた直近の通期実績が**2023年の24億円**で、2024年1〜9月のSUZURI売上高は前年同期比86.3%の6億2,900万円に落ちた ([GMOペパボ 2024年12月期Q3決算説明資料](https://pdf.pepabo.com/presentation/20241106p-agh2ahfahtahchadaemeijohqueiwiev.pdf))。2026年12月期上期の決算でも、GMOペパボは「SUZURI」の流通額の伸び悩みを減収要因に挙げている ([GMOペパボ 2026年12月期Q2決算説明資料](https://www.nikkei.com/markets/ir/irftp/data/tdnr/tdnetg3/20260813/g2rw0w/140120260813519264.pdf))。AIについては、2023年7月にStable Diffusionで生成した画像をTシャツにできる「スリスリAIラボ」を出したが、iOS限定・1日10枚まで・販売不可の実験機能にとどまった ([GMOペパボ プレスリリース 2023-07](https://pepabo.com/news/press/202307101300/))。その後SUZURIは自前の生成機能を育てる道を取らず、**2026年6月9日にAIエージェントと連携するリモートMCPサーバー**を公開して、対話でグッズの作成・管理・販売ができるようにした ([GMOペパボ プレスリリース 2026-06](https://pepabo.com/news/press/202606091300))。つまり「Geminiで作り、そのまま出品し、売れたらロイヤリティを受け取る」流れは、ユーザーがSUZURI上ですでに組める。新サービスが「AI生成＋出品」だけで差別化できる余地は、思ったより狭い。

「AIで生成してそのまま注文する」ところまでなら、国内にもすでに例がある。TMIXは2026年夏ごろ、写真と文章の指示からAIがデザインを作る「AIで画像編集」を公開した ([TMIX](https://tmix.jp/articles/info_ai))。CLAT-JAPANは2024年6月から、テキストで画像を生成する機能を注文者に無料で提供している ([PR TIMES](https://prtimes.jp/main/html/rd/p/000000009.000133064.html))。ただ、どちらも自分用・団体用の注文が目的で、他人に売ってロイヤリティを得る仕組みは確認できなかった。逆の側からは、株式会社薫の「AI lab」が2025年6月に始まっている。ChatGPTやMidjourneyで作った画像を投稿すると受注生産で販売し、売上の約50%をクリエイターに還元するとうたうが、生成は外部ツールに任せており、開始後の規模はわからない ([PR TIMES](https://prtimes.jp/main/html/rd/p/000000002.000163984.html))。BOOTH×pixivFACTORY、Tシャツトリニティ、UTme!マーケットなども「ベース価格＋クリエイターの上乗せ」型で、AI生成は内蔵していない ([BOOTH公式](https://booth.pm/announcements/459))([Tシャツトリニティ](https://www.ttrinity.jp/first/))([ユニクロ](https://www.uniqlo.com/jp/ja/contents/corp/press-release/2015/04/042811_utme.html))。製造インフラの側では、POD用APIを提供するPrintio（OpenFactory）が2026年4月にシリーズAで8,000万円を調達した ([PR TIMES](https://prtimes.jp/main/html/rd/p/000000020.000066048.html))。他方、Canvathは2023年11月にサービスを終えており ([Canvath公式ブログ](https://blog.canvath.jp/information/service_termination/))、PODからの撤退も珍しくない。

海外では、AI生成そのものはもう無料の標準機能だ。2024年に経営統合したPrintfulとPrintifyは、どちらもAI画像生成を**1日15回まで無料**で提供している ([Printful Help](https://support.printful.com/hc/en-us/articles/41396543812625))([Printify Help](https://help.printify.com/hc/en-us/articles/15493218417809-How-can-I-design-a-product-using-Printify-s-AI-Image-Generator))。Printifyが2026年4月ごろに出したChatGPTアプリは、ギフトの説明からデザインを作って注文まで案内するもので、生成エンジンに**Gemini 2.5 Flash Image（Nano Banana）**を使っている ([Printify ChatGPT App Terms](https://printify.com/chatgpt-app-terms-of-use/))([Printify Help](https://help.printify.com/hc/en-us/articles/43090996796689-How-do-I-order-personalized-gifts-through-the-new-Printify-App-in-ChatGPT))。Geminiを選ぶこと自体は実績のある判断だが、差別化の材料にはならない。最大の動きはAmazonで、**2026年6月8日**、Alexa for Shoppingで文章からグッズのデザインを作り、Merch on Demandで印刷・配送する機能を米国限定・デザイン無料で始めた ([TechCrunch](https://techcrunch.com/2026/06/08/amazon-now-lets-you-design-custom-merch-using-ai/))([About Amazon](https://www.aboutamazon.com/news/retail/design-merch-with-ai-alexa-for-shopping))。この流れには、デザインした人へのロイヤリティはない ([PPC Land](https://ppc.land/amazon-lets-shoppers-design-custom-merch-via-alexa-for-shopping/))。FourthwallもChatGPTアプリを通じて、生成からストアへの出品までを一つの会話で済ませられる ([Fourthwall blog](https://fourthwall.com/blog/chatgpt-fourthwall-integration))。GPTShirt.aiのような「プロンプト→注文」型の小さなサイトは数多いが、ユーザー間のマーケットとロイヤリティを持つサイトや、資金調達を公表したサイトは見つからなかった ([GPTShirt.ai](https://gptshirt.ai/))。

| サービス | サイト内AI生成 | 1枚から注文 | 他人への販売・ロイヤリティ | 補足 |
|---|---|---|---|---|
| SUZURI | 実験機能のみ（2023年、販売不可）。2026年6月にMCPで外部AIと連携 | ○ | ○ トリブン0〜5,000円 | クリエイター100万人、流通額は伸び悩み |
| TMIX / CLAT-JAPAN | ○ | ○ | 確認できず | 自分用・団体用の注文向け |
| AI lab（株式会社薫） | なし（外部AIで作って投稿） | ○ | ○ 売上の約50% | 2025年6月開始、規模は不明 |
| Printful / Printify | ○ 1日15回無料 | ○ | 自社ストア経由 | ChatGPTアプリはGemini 2.5 Flash Image |
| Amazon（Alexa for Shopping） | ○ 無料、米国のみ | ○ | なし | 2026年6月開始 |
| Redbubble / Threadless / Zazzle | なし | ○ | ○ | 外部で作られたAIアートの販路 |

「生成＋注文＋ロイヤリティ」の組み合わせが空いているのは、誰も気づいていないからではない。マーケット単体の採算が悪くなっているからだと読むべきだ。海外の主要マーケットは、そろってクリエイターの取り分を削っている。Redbubbleは月間売上に対して、Standardで50%、Premiumで20%のプラットフォーム手数料を取る ([Redbubble Help](https://help.redbubble.com/hc/en-us/articles/50959494599060-How-does-the-platform-fee-get-calculated))。2025年9月からは、20%を超える上乗せ部分にも別の手数料をかけ始めた ([Redbubble blog](https://blog.redbubble.com/2025/08/excess-markup-fee-explained/))。Threadlessはマーケットでの販売を2025年2月から**一律10%のロイヤリティ**に切り替えた ([Threadless Support](https://support.threadless.com/article/171-how-do-artist-earnings-work))。Zazzleの標準ロイヤリティは5%（上限50%）で、衣料品ではそこから35%のマーケティング手数料が引かれる ([Zazzle Help](https://www.zazzle.co.nz/hc/article/ambassador_referral_commissions_creator_royalties-30810357489175))。Merch by Amazonは2026年6月に流入元別の段階制へ移った。販売者向けブログによると、19.99ドルのTシャツで、Amazon内の自然流入が中心のクリエイターの取り分は約2.44ドルと、従来の約半分になった ([Merch Titans](https://merchtitans.com/blog/amazon-merch-royalty-changes-june-2026))。RedbubbleとTeePublicを運営するArticoreは、FY26（2026年6月期）に純利益1,090万豪ドルで黒字に戻した。ただし売上は6.9%減っており、回復の一因はアーティスト向け手数料の変更とされる ([Smallcaps](https://smallcaps.com.au/article/articore-group-returns-to-profitability-in-fy26-as-gross-margin-hits-record))。Springは旧プラットフォームを閉じて月額9.99ドルのサブスクリプションへ移り ([Quiver Quant](https://www.quiverquant.com/news/Amaze%2BHoldings%2BProvides%2BUpdate%2Bon%2BProduct%2BRoadmap,%2BSubscription%2BRollout%2Band%2BPath%2Bto%2BProfitability))、Society6は2025年、AI生成や低品質な作品を減らすとしてキュレーション制に移った ([Wildlife Art Store](https://www.wildlifeartstore.com/sell-art-on-society6/))。

## 技術の壁は4K生成で下がり、残る難所は印刷前の処理と需要の薄さ

### 画素数は足りており、品質事故の主因は透過と半透明

2026年時点のGoogleの画像モデルは、印刷に必要な画素数をほぼ満たしている。Nano Banana Pro（Gemini 3 Pro Image）は1K・2K・4Kの出力に対応し ([Google Cloud](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/models/gemini/3-pro-image?authuser=1))、4Kは縦横比1:1で4096×4096ピクセルになる。A4を300dpiで刷るのに必要なのは約2480×3508ピクセル、A3でも約3508×4961ピクセルなので、4K出力はA4には十分で、A3にもほぼ届く。2Kで生成し、ImagenのアップスケールAPI（出力は17メガピクセル以下）で拡大する手もある ([Google Cloud](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/model-reference/imagen-upscale-api))。大手のPrintfulも1024×1024で生成し、注文が入った時点で自動拡大して生産に回している ([Printful Help](https://support.printful.com/hc/en-us/articles/41396543812625))。

残る難所は3つある。1つ目は透過背景だ。Nano Banana系とgpt-image-2系はどちらも透過背景をそのままは出力できないと報告されている ([APIYI](https://help.apiyi.com/en?p=30331))。白背景と黒背景で2回生成し、その差分から透明度を計算するといった後処理が必要になる ([Replicate](https://replicate.com/jide/nano-banana-2-transparent/readme))。2つ目は半透明の部分だ。DTFやDTGのプリンタは不透明なドットしか打てない。そのため、AI画像に多い半透明部分やグラデーション、ぼかした縁では白インクの下地が「もや」や四角い縁となって出てしまい、ハーフトーン化などの前処理が欠かせない ([Transfer Superstars](https://www.transfersuperstars.com/blogs/dtf-graphic-design/fixing-common-dtf-printing-problems-photoshop-vs-ai))。3つ目は日本語の文字だ。週刊アスキーは、初代Nano Bananaに比べてProでは「実際に読める文章」が増えたものの、細かな誤りは残ると評価している ([週刊アスキー](https://weekly.ascii.jp/elem/000/004/354/4354220/3/))。チーム名や推しの名前を入れたい場合は、生成モデルに文字を描かせず、エディタ側でフォントを重ねるほうが誤字の心配が少ない。どの難所も、背景除去→半透明の除去→ハーフトーン化→刷り上がりのプレビューを顧客に承認してもらう、という自動パイプラインで吸収できる。つまり事業の可否を左右する問題ではなく、開発で解ける課題だ。

製造との接続も既製品で足りる。Printio APIは2025年10月に仕様書を一般公開しており、商品登録、発注、製造状況の確認、追跡番号の受け取りまで扱える ([PR TIMES](https://prtimes.jp/main/html/rd/p/000000019.000066048.html))。SUZURI APIは無料で使えるが、商品も決済もSUZURI側にできるため、運営者が自前の利幅を取りにくい ([SUZURI API](https://suzuri.jp/developer/documentation/v1))。海外のGelatoは日本国内での生産をうたっているが、国内の提携工場や対応品目は公開していない ([Gelato](https://gelato.com/print-on-demand/japan))。

### 伸びているのは自分用・団体用で、他人の作品を買う市場は頭打ち

伸びているのは「自分用・団体用」の受注生産で、「他人のデザインを買う」個人間の販売ではない。オリジナルプリント.jpを運営するイメージ・マジックは、2024年12月期に売上高77億6,700万円（前年比27.9%増）を計上した ([決算短信](https://finance-frontend-pc-dist.west.edge.storage-yahoo.jp/disclosure/20250214/20250213574502.pdf))。2025年12月期は約94億円とされる ([edinetdb](https://edinetdb.jp/company/E37419/earnings/FY2025))。これに対してSUZURIの流通額は横ばいから減少が続いている。Printioは国内のPOD市場全体を約100億円と見積もっている ([Printioブログ](https://printio.me/blog/pod-world-japan-2023ss/))。推し活市場は約3.8兆円と推計されるが ([NRI](https://www.nri.com/jp/media/column/mcs_blog/20250829.html))、推し活の月間支出は3,000円未満が39.3%で最も多く ([マイナビニュース](https://news.mynavi.jp/article/20260323-4233123/))、グッズの入手先も71%が公式の物販だ ([ネットショップ担当者フォーラム](https://netshop.impress.co.jp/node/15015))。単価3,000〜4,000円のTシャツを頻繁に買う層は限られる。266人を対象にした実験もある。作り手を伏せるとAI製品は人間のデザインと同等以上に評価されたが、AIが関わったことを開示すると、品質の印象も支払意欲も下がった ([ボローニャ大学 修士論文](https://amslaurea.unibo.it/id/eprint/36888/))。この結果からすると、「AIで作れる」を売り文句にするより、「自分たちだけのデザインが短時間で決まる」体験を売るほうが筋がよい。

マーケットの側では、誰でもAIで似た画像を作れる以上、デザインそのものの希少価値が低い。Googleの規約も、同じか似た出力が複数の顧客に生成されうると明記している ([Google Cloud Service Specific Terms](https://cloud.google.com/terms/service-terms))。国内ではBOOTHが2023年、似た作品の大量出品を検索結果から外したが ([BOOTH公式 #646](https://booth.pm/announcements/646))、それでも改善しなかった。2025年7月には、強制非公開やアカウント停止まで踏み込んでいる ([BOOTH公式 #828](https://booth.pm/announcements/828))。海外でも、Redbubbleを含むArticoreのマーケット売上はFY25に前年比10%減、FY22比では21%減だった ([Capital Brief](https://www.capitalbrief.com/briefing/articore-group-posts-28-steeper-loss-in-fy25-13deb9ab-1319-44a0-abdf-26acff75b56d/))。まとめると、サービス自体は作れるが、マーケットを収益の柱にするのは難しい。注文を主軸に据え、マーケットは「作った人が公開してよいと選んだデザインのギャラリー」として小さく始めるのが妥当だ。

## 運営者は「場の提供者」ではなく侵害品の製造・販売者とみなされうる

### AIの依拠性推認と物理的な印刷が、運営者の責任を重くする

侵害リスクで最も重いのは、運営者が単なる「場の提供者」ではなく、侵害品の製造・販売者と評価されうる点だ。自炊代行事件では、利用者の依頼で書籍をスキャンした業者が複製の主体とされ、私的使用のための複製という例外は認められなかった ([弁護士ドットコムニュース](https://www.bengo4.com/c_1009/n_4446/))。米国のGreg Young Publishing v. Zazzle事件では、DMCAのセーフハーバー（免責規定）は物理的な商品の印刷・製造には及ばないとされた。2019年には第9巡回区控訴裁判所が**46万ドル**の陪審評決を復活させている ([Proskauer](https://www.proskauer.com/blog/online-willful-infringement-standard-clarified-zazzle-jury-award-reinstated))([Eric Goldman blog](https://blog.ericgoldman.org/archives/2017/06/dmca-safe-harbor-doesnt-protect-zazzles-printing-of-physical-items-greg-young-v-zazzle.htm))。チュッパチャプス事件は、モール運営者は侵害を知ってから合理的な期間内（楽天の場合は8日以内）に削除すれば責任を負わないとした ([知財高裁 判決要旨](https://www.ip.courts.go.jp/app/files/hanrei_jp/999/081999_point.pdf))。しかしこの枠組みは、印刷して出荷するという運営者自身の行為には及ばないと考えるべきだ。

AI特有の問題もある。文化庁の「AIと著作権に関する考え方について」（2024年3月）は、利用者が既存の作品を知らなくても、それがAIの学習データに含まれていれば、似たものが生成されたときには通常、依拠性が推認されると整理している ([文化庁](https://www.bunka.go.jp/seisaku/bunkashingikai/chosakuken/pdf/94037901_01.pdf))。ユーザーがプロンプトに「ピカチュウ」と書かなくても、似たキャラクターが出てくれば侵害になりうる。プロンプトの禁止語だけでは足りず、出力画像の類似チェックと人による審査が要る。東京地裁は2023年9月29日の判決で、Tシャツのイラストに著作物性を認め、翻案権と同一性保持権の侵害に加えて商標権の侵害も認めている ([日本ユニ著作権センター](https://jucc.sakura.ne.jp/precedent/precedent-2023-09-29d.html))([岡田弁理士 判例紹介](https://knpt.com/contents/trademark/2023.11.30.pdf))。有名人の顔を主役にしたTシャツは、ピンク・レディー事件の最高裁判決が示したパブリシティ権侵害の第1類型（肖像そのものを鑑賞の対象とする商品）にあたる ([法務省資料](https://www.moj.go.jp/content/001468506.pdf))。Vertex AIには人物を生成させない`ALLOW_NONE`という設定があり ([Vertex AI APIリファレンス](https://docs.cloud.google.com/ruby/docs/reference/google-cloud-ai_platform-v1/latest/Google-Cloud-AIPlatform-V1-ImageConfig-PersonGeneration))、有名人の名前のブロックと組み合わせて使うべきだ。

### Googleの規約は補償から商標を外し、18歳未満の利用を禁じる

Googleとの契約は、リスクを移す手段としては弱く、事業設計を縛る制約としては強い。Google Cloudの生成物補償は、次の場合を対象外にしている。侵害を「知るべきだった」出力、安全フィルターを回避した場合、権利者から通知を受けた後の利用、そして**出力を商取引に使ったことに基づく商標関連の主張**だ。補償の対象も、手を加えていない出力に限られる ([Google Cloud Service Specific Terms](https://cloud.google.com/terms/service-terms))。ブランド名やロゴが主なリスクになるTシャツでは、補償はほとんど効かない。補償対象のリストに載るのは、Vertex AI（現在の名称はGemini Enterprise Agent Platform）でGA版のモデルを使う場合だけで、**Gemini API（Google AI Studio）は載っていない** ([Google Cloud Generative AI Indemnified Services](https://cloud.google.com/terms/generative-ai-indemnified-services))。Gemini APIの無料枠では、入力も出力も製品改善に使われ、人が読むこともある ([Gemini API Additional Terms](https://ai.google.dev/terms))。

そのうえVertex側の規約は、**18歳未満を対象とする、または18歳未満が利用する可能性が高いサービスで生成AIを使ってはならない**と定めている。違反が疑われれば、Googleはただちに利用を止められる ([Google Cloud Service Specific Terms](https://cloud.google.com/terms/service-terms))。クラスTシャツ、部活、文化祭といった学生の団体注文は、需要が最も確実な層と見られる。しかし生成機能を学生に開放すれば、この規定に抵触しうる。Gemini APIや他社モデルの年齢規定は今回確認できていない。モデル選びは技術の判断ではなく、法務の判断として扱う必要がある。OpenAIの補償（Copyright Shield）も商標関連の請求を対象外にしているため ([National Law Review](https://natlawreview.com/article/openais-copyright-shield-broadens-user-ip-indemnities-ai-created-content))、どの提供者を選んでも商標リスクは自社で負うことになる。

### ロイヤリティは著作権ではなく契約で作り、お金の流れと源泉徴収を設計する

ロイヤリティ還元の仕組みには、法的な論点が3つある。1つ目は、デザインの「権利」が弱いことだ。文化庁の整理では、AI生成物が著作物になるのは、プロンプトの具体性、試行の回数、候補からの選択、加筆修正といった人の創作的な関与が積み重なった場合に限られる ([文化庁](https://www.bunka.go.jp/seisaku/bunkashingikai/chosakuken/pdf/94037901_01.pdf))。短いプロンプトで1回生成しただけの画像は、著作物と認められにくい。2025年11月には、千葉県警がAI生成画像を著作物と判断し、無断で複製した人物を全国で初めて書類送検した ([日本経済新聞](https://www.nikkei.com/article/DGKKZO92731350Q5A121C2CM0000/))。ただし、これは人の関与が大きかった事案とみられ、Tシャツのデザイン全般に当てはまるわけではない。そのためロイヤリティは、著作権ではなく利用規約上の契約上の権利として設計し、「このデザインは法的に独占できる」と受け取られる表示は避けるべきだ。

2つ目はお金の流れだ。プラットフォームが購入者から代金を集めて出品者に分配すると、資金移動業（為替取引）にあたるかが問題になりうる ([EC法務](https://ec-houmu.com/start/shunodaikou))。2025年の資金決済法改正では、受取人が個人の場合の収納代行の扱いが議論されたが、解説によって説明が食い違っている ([有斐閣](https://yuhikaku.com/articles/-/56937))([BUSINESS LAWYERS](https://www.businesslawyers.jp/articles/1476))。SUZURIと同じように運営者がTシャツの売主になり、代金を自社の売上として受け取ったうえで、デザイナーには販売実績に応じたライセンス料を自社の支払いとして払う形にすれば、この論点を避けやすい。特定商取引法などの消費者法への対応も、運営者一社にまとまる。ただし、これらの法令の具体的な義務は今回一次資料で確認できていないため、弁護士の確認が必要だ。

3つ目は税金だ。個人のデザイナーに払う「デザインの報酬」や「著作権の使用料」は、所得税法204条1項1号の源泉徴収の対象になる。税率は10.21%で、1回の支払いが100万円を超える部分は20.42%だ ([国税庁 所得税基本通達](https://www.nta.go.jp/law/tsutatsu/kihon/shotoku/36/02.htm))([所得税法204条](https://www.zeiken.co.jp/hourei/HHTOK000000/204.html))。少額のロイヤリティを多数の個人に払う事業では、源泉徴収、支払調書、マイナンバー管理を仕組み化する費用が見えにくい負担になる。免税事業者のデザイナーへの支払いにかかる仕入税額控除も、令和8年度の税制改正により、2026年10月から2028年9月までは70%に下がる ([freee](https://www.freee.co.jp/kb/kb-invoice/invoice_transitional_measures/))。フリーランス法は、ユーザーが自発的に作ったデザインのライセンスには適用されにくい。ただし運営者がお題を出して制作を頼む場合は、対象になりうる ([中小企業庁リーフレット](https://www.chusho.meti.go.jp/keiei/torihiki/download/freelance/law_03.pdf))。

### 運用面で見落としやすいのは、モデルの入れ替わりの速さと審査の負担

運用面で見落としやすいのは、モデルの入れ替わりの速さだ。Imagen 4はGemini APIでの提供を2026年8月17日に終え、Googleは後継として`gemini-nano-banana-2.1`を勧めている ([Gemini API deprecations](https://ai.google.dev/gemini-api/docs/deprecations))。プロンプトや前処理を特定のモデルに合わせ込むと、数か月で作り直しになる。モデルを差し替えられる抽象化層が要る。審査の負担も重い。BOOTHが大量出品への対策を2度強化しなければならなかったように、審査は運営にとって大きな固定費になりうる。権利者からの通報への対応期限は、情報流通プラットフォーム対処法（2025年4月施行）が大規模事業者に課す「原則7日以内の通知」が目安になる。新興企業は指定対象外だが、チュッパチャプス事件の8日とも整合する ([総務省](https://www.soumu.go.jp/main_sosiki/joho_tsusin/d_syohi/ihoyugai.html))([契約ウォッチ](https://keiyaku-watch.jp/media/hourei/joho-platform-2024/))。

| リスク | 重さ | 主な根拠 | 対策の要点 |
|---|---|---|---|
| 既存キャラクター・作品に似た出力を印刷・販売する | 高 | 文化庁の依拠性推認、自炊代行事件、Zazzle判決 | 作品名・作家名の禁止語、出力の類似検出、出品前・印刷前の人による審査、通報への7日以内の対応、同じデザインの再印刷を止める |
| ブランド名・ロゴ（商標） | 高 | 東京地判2023年9月29日、Google補償の商標除外 | ロゴ検出、文字入れ時の登録商標チェック。リスクは自社で負う前提 |
| 有名人の肖像 | 高 | ピンク・レディー事件の最高裁判決 | 人物生成をオフにする（`ALLOW_NONE`）、有名人の名前をブロック、写真変換機能を制限 |
| Google規約違反によるAPI停止 | 高 | 18歳未満条項、フィルター回避の禁止 | 利用者を18歳以上に限る、またはモデル提供者を見直す。予備の提供者を確保 |
| ロイヤリティのお金の流れ | 中〜高 | 資金決済法（2025年改正） | 運営者が売主のライセンス型。残高の換金・譲渡を禁止。弁護士の意見書 |
| 源泉徴収・インボイス | 中 | 所得税法204条、令和8年度税制改正 | 10.21%の源泉徴収を仕組み化。ロイヤリティが税込か税抜かを規約に明記 |
| AIデザインの権利の弱さ | 中 | 文化庁の整理、千葉の書類送検事例 | ロイヤリティは契約上の権利と明記。重複出品を禁止。生成ログを保存 |
| モデルの廃止・価格改定 | 中 | Imagen 4の提供終了 | モデルの抽象化、四半期ごとの再検証 |

## 1枚4,400円で残るのは広告前で約1,200円、広告経由なら約200円

### 初期費用は開発がほとんどを占め、法人設立・商標・規約は100万円未満

初期費用の大半は開発費だ。MVPに必要なのは、AI生成の流れ、キャンバスエディタとモックアップ、カートと決済、クリエイターの出品ページ、ロイヤリティの台帳と支払い、管理画面と審査キュー、印刷業者への発注API連携である。これを3〜4人・3〜4か月、計10〜16人月と見積もる。受託開発の人月単価80万〜120万円で計算すると**約800万〜1,900万円**になる ([siainc](https://www.siainc.jp/topic/system-development-cost))([xincere](https://blog.xincere.jp/articles/system-development-man-month-unit-price))。フリーランスの平均月単価78.4万円（2026年7月）([PR TIMES](https://prtimes.jp/main/html/rd/p/000001254.000000725.html)) で2〜3人を4か月集めるなら、**約600万〜900万円**で済む（創業者がプロジェクト管理を担う前提）。ECやマッチング系のWebシステムをゼロから作る相場は300万〜1,000万円とされ ([siainc](https://www.siainc.jp/topic/system-development-cost))、AI生成、審査、支払いの機能の分だけ上に振れる。Shopifyとアプリで組む方法もあるが、複数クリエイターへのロイヤリティ分配やAI生成のUIは標準では扱えず、単一ブランドのストアにしか向かない。

法人設立の費用は、株式会社なら電子定款で約19.6万円から ([マネーフォワード](https://biz.moneyforward.com/establish/basic/?p=158))。商標は特許庁の手数料だけで1区分44,900円（10年）かかる ([特許庁](https://www.jpo.go.jp/system/process/tesuryo/hyou.html))。衣類、オンライン小売、印刷加工、ソフトウェアといった3〜4区分を押さえると、13万〜17万円になる。弁護士に利用規約を作ってもらう費用は10万〜30万円程度とされる ([atsoho](https://atsoho.com/blog/terms-of-service-drafting-side-job))。合計で40万〜70万円が目安だ。ただし、お金の流れ、源泉徴収、Googleの規約まで含めた弁護士・税理士のレビューは別料金で、その相場は今回確認できていない。

### AI生成費は小さく、利益を決めるのは印刷原価

AI生成費は思ったより小さい。Imagen 4の後継としてGoogleが推奨するNano Banana 2.1は、1K画像1枚が約0.034ドル（約5.3円）、4K画像1枚が約0.113ドル（約17.9円）と報じられている ([aifreeapi](https://www.aifreeapi.com/en/posts/nano-banana-2-1))。1ドル158円で換算すると、プレビュー10枚と最終4K画像1枚の典型的なセッションで約71円、軽い使い方で約44円、重い使い方で約124円になる。上位のNano Banana Proは1K・2Kが0.134ドル、4Kが0.24ドルで ([Gemini API pricing](https://ai.google.dev/gemini-api/docs/pricing))、典型的なセッションで約250円と3倍以上かかる。プロンプト補助のLLMや、無料で使えるOpenAIのモデレーション ([Analytics Vidhya](https://www.analyticsvidhya.com/blog/2026/05/openai-omni-moderation/)) の費用は誤差の範囲だ。ただし、Geminiの画像APIには無料枠がないとされ ([laozhang](https://blog.laozhang.ai/en/posts/gemini-image-api-free))、注文に至らなかったセッションの費用も積み上がる。5回のセッションで1件注文が入るとすると、1注文あたりのAI費用はNano Banana 2.1で約355円、Proでは約1,250円に膨らむ。なお、Nano Banana 2.1の価格は二次情報でしか確認できておらず、補償の対象になるVertex AI上での単価もわかっていない。

印刷を外注する場合、法人向けの卸価格表は公開されておらず、すべて見積もり制だ。消費者価格では、白Tシャツ1枚のフルカラー印刷が約1,460円（TMIX）から2,200円（UP-T）である ([TMIX](https://tmix.jp/product_categories/1))([tshirt-kakaku](https://tshirt-kakaku.com/companies/up-t/))。オリジナルプリント.jpの内訳（ボディ660円＋インクジェット印刷1,254円＋送料750円＝2,664円）も同じくらいの水準だ ([UP-T比較記事](https://up-t.jp/news/2245))。送料は、2026年10月の日本郵便の値上げ後でクリックポストが240円 ([haruuu](https://haruuu.online/clickpost-neage-240en-2026/))、ネコポスが法人契約で最大385円 ([stockcrew](https://stockcrew.co.jp/insights/delivery-yamato))。決済手数料は、国内カードでStripeが3.6%、PAY.JPのStandardプランが3.3%だ ([PAY.JPコラム](https://pay.jp/column/stripe-fees-guide))([PAY.JP](https://pay.jp/plan))。

| 1注文あたり（税抜、1枚） | 金額 | 前提 |
|---|---|---|
| 売上 | 4,000円 | 販売価格は税込4,400円と仮定 |
| 印刷＋ボディ（外注で1枚） | −2,000円 | UP-Tの税込2,200円 |
| 送料 | −218円 | クリックポスト240円 |
| 梱包 | −40円 | 仮定 |
| 決済手数料 | −158円 | Stripe 3.6% |
| AI生成 | −71円 | Nano Banana 2.1で典型的な1セッション |
| 背景除去・拡大・モデレーション | −10円 | 各種API |
| ロイヤリティ（出品デザインが売れた場合） | −300円 | SUZURIのトリブン初期値と同額 ([imano-okiniiri](https://imano-okiniiri.com/suzuri-pricing-guide/)) |
| **広告前の貢献利益** | **約1,200円（30%）** | |
| 広告費 | −1,000円 | Instagramのアパレル広告のROAS 300〜500%のうち400%と仮定 ([lifunext](https://lifunext.com/blog/sns/instagramad/sns-snsad-14/)) |
| **広告後の貢献利益** | **約200円** | |

この表で最も効いてくるのは、SUZURIとの価格差だ。SUZURIのスタンダードTシャツの原価は、税抜2,080円からとする記事と、税込2,816円（2024年3月時点）とする記事があり、2026年の一覧記事では3,091円からと書かれている。情報源によって数字が食い違うが ([imano-okiniiri](https://imano-okiniiri.com/suzuri-pricing-guide/))([watasiino](https://watasiino.com/contents/4714))([でらこブログ](https://melo-blog.com/suzuri-gazou-size))、いずれもSUZURIの利益をすでに含んだ価格だ。印刷を消費者価格の約2,200円で仕入れる新規参入者は、ロイヤリティと広告費を払いながらこの価格には並べない。法人向けの見積もりで印刷原価を1,600円程度まで下げるか、4,000〜5,000円の価格に見合う体験価値を示すしかない。印刷原価を500円下げれば、利益率は約12ポイント上がる。広告に頼るのも難しい。InstagramのアパレルのCPC（クリック単価）は80〜120円とされ ([lifunext](https://lifunext.com/blog/sns/instagramad/sns-snsad-14/))、サイトでの購入率を1〜2%と仮定すると、1件の獲得単価は約4,750〜9,500円になる。初回注文の貢献利益を大きく上回るため、成長の源泉は自然流入、クリエイター自身の宣伝、団体での一括注文、リピートに求めるしかない。

ロイヤリティの支払いにも見えにくい費用がある。Stripe Connectを使うと、支払いを受けたアカウントごとに月200円、さらに1回の振込ごとに250円＋0.25%かかる ([PAY.JPコラム](https://pay.jp/column/stripe-connect-guide))。3,000円を払うと手数料は約458円で、支払額の15%に達する。最低支払額（たとえば5,000〜10,000円）を設けて、未満の分は翌月に繰り越す仕組みが欠かせない。

### 損益分岐は自然流入で月85〜315枚。自社印刷は需要が見えてから

立ち上げ期の固定費は小さい。インフラはVercel Proが1席月20ドル ([MakerKit](https://makerkit.dev/blog/saas/vercel-cost))、Supabase Proが月25ドルから ([MakerKit](https://makerkit.dev/blog/saas/supabase-pricing)) で、ストレージや監視を含めても月1.4万〜3.4万円程度だ。税理士の顧問料は売上5,000万円未満で月1.5万〜3.5万円、決算申告が10万〜20万円かかる ([CrowdWorks Times](https://crowdworks.jp/times/know-how/26769/))。人件費を除いた固定費を月約10万円とすると、広告前の貢献利益約1,200円で割って、損益分岐は**自然流入の注文で月約85枚**になる。開発費1,000万円を36か月で償却すると**月約315枚**だ。ただし、この試算には人件費、人による審査、カスタマーサポート、刷り損じによる再印刷が入っていない。これらの単価は今回のどの資料からも得られておらず、実際には最大の費用項目になる可能性が高い。自社で印刷する選択肢もある。業務用のDTGプリンタはBrother GTX proが税込184.8万円で ([europort](https://europort.jp/blog/garment-comparison))、1枚の変動費は約1,000〜1,700円と外注の約2,200円より安い。しかし減価償却だけで月5万〜7万円かかり、外注より安くなるのは月数百枚を安定して刷れるようになってからだ。MVPは外注で始めるのが合理的である。

## 結論

今回の調査で、問いの軸が変わった。「AIでTシャツを作れるか」はもう論点ではない。論点は「印刷にかかる原価と法的責任を誰が負うか」だ。本当の競合は、どこかのAI Tシャツ新興企業ではなく、ユーザーが無料で組める「Gemini＋SUZURIのMCP」という組み合わせである。勝ち筋は、そこにないものに限られる。たとえば、団体での意思決定を最後まで引き受けること（案出し→投票→サイズ集計→一括決済→期日どおりの納品）、刷り上がりの品質を保証すること（半透明や誤字の自動検出、印刷事故時の再印刷）、そして日本語の文字合成である。ただし、需要が最も確実な学生の団体注文は、Vertex AIの18歳未満条項とぶつかる。最初に決めるべきは、社会人のサークル、企業のイベント、推し活のオフ会など成人の団体に絞るのか、未成年が使えるモデル提供者と契約条件を確保するのか、だ。このどちらを選ぶかで、ターゲット、マーケティング、モデル選定がまとめて決まる。

マーケットは、運営者が売主となるライセンス型にし、運営の利益はSUZURIと同じく原価側に載せるのが最も安全だ。AI画像は権利として弱く、簡単にまねされる。そのため報酬の対象は、1枚の画像よりも、テンプレート、生成レシピ、シリーズ、クリエイターのフォロワーに置くほうが長持ちする（今回の資料から導いた推論）。費用の面で効いてくるのは、1セッション数十円のAI費用ではなく、法人向けの印刷単価交渉と人による審査の体制である。審査の質は、そのまま法的責任の大きさになる。今回の数値の多くは検索結果の要約から集めたもので、Nano Banana 2.1の価格、SUZURIの原価、資金決済法の解釈には資料間の食い違いが残る。投資判断の前に、Printio・UP-T・TMIXから法人向けの見積もりを取ること、そして年齢条項とお金の流れについて弁護士の意見を得ることが、次に取るべき二つの行動になる。
