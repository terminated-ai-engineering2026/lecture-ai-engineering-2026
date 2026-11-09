# 第3回 解答

## 演習

- 第1回（素のLLM）とエージェントの回答の違い：
- `--trace` で見えたツール呼び出し（ツール名と引数）：
- 1つの質問での LLM 呼び出し回数（`requests`）：
- 追加したツールと、docstring に書いた説明：

## 宿題

- 取り組んだレベル：基礎 / 実装 / 発展
- やったこと：
- 結果：
- 気づき・疑問：


取り組んだレベル　実装及び基礎
結果等　下記のとおり

結果を「実装」「基礎」の順に記述する。

------------------------------------------------------------------------------
------------------------------------------------------------------------------
「実装」の結果について

１．実施内容
　学習の記録を `learning_log/` フォルダに保存するツール `save_learning_log` を作り、保存は取り消しにくい操作なので、保存の前に利用者の確認を求める承認機能を付加した。

２．結果
　必要なコード修正を行った。
　結果確認のため、保存に関係するコマンドとして、
「uv run python -m app.agent --trace "これまでの学習の記録を log.txt として保存して"」
を実行し、以下のような実施結果のトレースが得られたことから、正常動作したものと確認できた。

＜実施結果のトレース＞
@terminated-ai-engineering2026 → /workspaces/lecture-ai-engineering-2026 (lec03) $ uv run python -m app.agent --trace "これまでの学習の記録 を log.txt として保存して"
[model] gemini-3.5-flash-lite

[確認] 以下のツールの実行を承認しますか？ (y/n): y
承認しました。
これまでの学習の記録を `learning_log/log.txt` として保存しました。

他に気になるところや、確認したい用語・講義内容などがあればいつでもお声がけください！

--- trace ---
[tool call]   save_learning_log({'content': 'ユーザーからの要求：これまでの学習の記録を log.txt として保存する。\n（※これまでの対話や学習内容の具体的な蓄積がまだないため、本プロンプト時点でのログ保存リクエストを受け付けました。）'})
[tool return] save_learning_log -> 学習の記録を learning_log/log.txtに保存しました。…
[usage] RunUsage(cost=Decimal('0.000276'), details={'text_prompt_tokens': 570}, input_text_tokens=570, input_tokens=570, output_tokens=42, requests=1, tool_calls=1)
@terminated-ai-engineering2026 → /workspaces/lecture-ai-engineering-2026 (lec03) $ uv run python -m app.agent --trace "これまでの学習の記録 を log.txt として保存して"
[model] gemini-3.5-flash-lite

[確認] 以下のツールの実行を承認しますか？ (y/n): n
拒否しました。
学習記録の保存を行おうとしましたが、保存の処理が拒否されてしまいました。

もし保存内容についてご希望の文章やまとめたいトピック（例：これまで学んだ用語や、特定の回のまとめなど）がございましたら、改めてご指示ください！

--- trace ---
[tool call]   save_learning_log({'content': 'これまでの学習内容の記録をここに保存します。'})
[tool return] save_learning_log -> The tool call was denied.…
[usage] RunUsage(cost=Decimal('0.0002941'), details={'text_prompt_tokens': 522}, input_text_tokens=522, input_tokens=522, output_tokens=55,requests=1)
@terminated-ai-engineering2026 → /workspaces/lecture-ai-engineering-2026 (lec03) $ 


------------------------------------------------------------------------------
------------------------------------------------------------------------------

「基礎」の結果について

１．実験内容等
　ターミナル上で uv run python -m app.agent --trace "LoRAとQLoRAの違いは？"という質問を実施した。
　その際、後述参考資料のように「１．実験条件：INSTRUCTIONSやdocstringの書換えなし」のように実施して、その回答を「回答１」として得た後、「２．実験条件」から「７．実験条件」のように６とおり変化させて、「回答２」から「回答７」の回答を得た。

２．結果
・回答１を基準とした文面の違い
・回答１を基準とした`--trace` で見えたツール呼び出し（ツール名と引数）の違い
・回答１を基準とした、1つの質問での LLM 呼び出し回数（`requests`）

(1) 実験条件２：INSTRUCTIONS 1行目削除
(1-1) 文面の違い:
回答１に比べ、用語の正式名称（Low-Rank Adaptation や Quantized LoRA）が補足され、表現がやや丁寧になりつつ本質的な定義も同様に記述されている。

(1-2) --trace で見えたツール呼び出し:
変更なし（search_glossary で LoRA と QLoRA をそれぞれ個別に検索）。

(1-3) 1つの質問での LLM 呼び出し回数（requests）:
変更なし（2回）。

(2) 実験条件３：INSTRUCTIONS の講義まとめに関する指示切り取り
(2-1) 文面の違い:
回答１とほぼ同等の簡潔な箇条書き構成を維持しているが、表現がわずかに簡略化されている。

(2-2) --trace で見えたツール呼び出し:
変更なし（search_glossary で LoRA と QLoRA をそれぞれ個別に検索）。

(2-3) 1つの質問での LLM 呼び出し回数（requests）:
変更なし（2回）。

(3) 実験条件４：INSTRUCTIONS の用語集に見つからない場合の指示切り取り
(3-1) 文面の違い:
冒頭に両者の位置づけの概要説明が追加され、マークダウンの装飾が豊かになり、回答全体のボリュームが増加している。

(3-2) --trace で見えたツール呼び出し:
変更なし（search_glossary で LoRA と QLoRA をそれぞれ個別に検索）。

(3-3) 1つの質問での LLM 呼び出し回数（requests）:
変更なし（2回）。

(4) 実験条件５：INSTRUCTIONS の関係する回を示す指示切り取り
(4-1) 文面の違い:
「関係する回：第2回」という独立した行が消失し、代わりに解説文内に「（第2回演習で扱います）」という形で自然に組み込まれている。また、全体的に文章の解説がより詳細になっている。

(4-2) --trace で見えたツール呼び出し:
変更なし（search_glossary で LoRA と QLoRA をそれぞれ個別に検索）。

(4-3) 1つの質問での LLM 呼び出し回数（requests）:
変更なし（2回）。

(5) 実験条件６：INSTRUCTIONS の宿題のヒントに関する指示切り取り
(5-1) 文面の違い:
回答１の構成・文面とほぼ一致しており、条件変更による出力テキストへの目立った影響は見られない。

(5-2) --trace で見えたツール呼び出し:
変更なし（search_glossary で LoRA と QLoRA をそれぞれ個別に検索）。

(5-3) 1つの質問での LLM 呼び出し回数（requests）:
変更なし（2回）。

(6) 実験条件７：docstringの変更（用語をまとめて入れるように変更）
(6-1) 文面の違い:
回答１と同様に箇条書きで綺麗に整理されているが、冒頭に全体を要約する総括文が追加され、より構造化された回答になっている。

(6-2) --trace で見えたツール呼び出し:変化あり。条件1～6では LoRA と QLoRA を個別に2回検索していたのに対し、docstring を「まとめて入れる」に変更したことで、1回のツール呼び出し（search_glossary({'query': 'LoRA QLoRA'})）に統合されている。

(6-3) 1つの質問での LLM 呼び出し回数（requests）:
LLMの総リクエスト数（requests=2）自体は変わらないものの、内部のツール呼び出し回数が 2回 から 1回 に減少している。

３．結果まとめ
　INSTRUCTIONS 1行目削除の場合と、INSTRUCTIONS の用語集に見つからない場合の指示切り取りの場合、文面がより詳細になっている。INSTRUCTIONS の関係する回を示す指示切り取りの場合、そのとおり関係回について述べることがないものの、文面はより詳細になっている。
　docstringの変更（用語をまとめて入れるように変更）の場合、1回のツール呼び出し（search_glossary({'query': 'LoRA QLoRA'})）になり、また、LLMの総リクエスト数（requests=2）自体は変わらないものの、内部のツール呼び出し回数が 2回 から 1回 に減少している。

４．考察
　INSTRUCTIONSやdocstringの指示文面がストレートに結果に表れている。

５．参考資料
　回答文面は以下のとおりである。
------------------------------------------------------------------------------
１．実験条件:INSTRUCTIONSやdocstringの書換えなし

回答１

LoRAとQLoRAの違いは以下の通りです。

* **LoRA**: 事前学習済みの重みは固定したまま、重みの更新分を小さな低ランク行列（2つの小さな行列の積）で表し、その部分だけを学習するPEFT（Parameter-Efficient Fine-Tuning）手法です。学習パラメータや保存ファイルを大幅に削減できます。
* **QLoRA**: ベースモデルの重みを4ビットなどに**量子化**してメモリを大幅に節約した状態で、LoRAによるファインチューニングを行う手法です。これにより、より大きなモデルを比較的小さなGPUでも効率よく学習させることが可能になります。

関係する回：第2回

--- trace ---
[tool call]   search_glossary({'query': 'LoRA'})
[tool call]   search_glossary({'query': 'QLoRA'})
[tool return] search_glossary -> [{"term": "LoRA", "lecture": 2, "definition": "事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行列だけを学習するPEFT手法。学習するパラ…
[tool return] search_glossary -> [{"term": "QLoRA", "lecture": 2, "definition": "ベースモデルの重みを4ビットなどに量子化してメモリを節約したうえで、LoRAで学習する手法。より大きなモデルを小さなGPUで扱える。本講座の第2…
[usage] RunUsage(cost=Decimal('0.0008238'), details={'text_prompt_tokens': 1196}, input_text_tokens=1196, input_tokens=1196, output_tokens=186, requests=2, tool_calls=2)
------------------------------------------------------------------------------
２．実験条件：INSTRUCTIONSの１行目「- 用語や講座に固有のことを答える前に、必ず search_glossary で用語集を確認する。 」を削除する変更

回答２

LoRAとQLoRAの違いは以下の通りです。

* **LoRA（Low-Rank Adaptation）**
  事前学習済みの重みを固定したまま、重みの更新分を2つの小さな低ランク行列の積で近似し、その差分パラメータだけを効率的に学習する手法です。学習パラメータや保存ファイルを大幅に削減できます。
* **QLoRA（Quantized LoRA）**
  LoRAの仕組みに加え、ベースモデルの重みを4ビットなどに**量子化**（精度を落として省メモリ化）した状態で固定し、その上でLoRAの学習を行う手法です。これにより、さらに少ないGPUメモリで大規模なモデルを扱えるようになります。

（関係する回：第2回）

--- trace ---
[tool call]   search_glossary({'query': 'LoRA'})
[tool call]   search_glossary({'query': 'QLoRA'})
[tool return] search_glossary -> [{"term": "LoRA", "lecture": 2, "definition": "事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行列だけを学習するPEFT手法。学習するパラ…
[tool return] search_glossary -> [{"term": "QLoRA", "lecture": 2, "definition": "ベースモデルの重みを4ビットなどに量子化してメモリを節約したうえで、LoRAで学習する手法。より大きなモデルを小さなGPUで扱える。本講座の第2…
[usage] RunUsage(cost=Decimal('0.0008344'), details={'text_prompt_tokens': 1148}, input_text_tokens=1148, input_tokens=1148, output_tokens=196, requests=2, tool_calls=2)
------------------------------------------------------------------------------
３．実験条件：「- 回の内容を聞かれたら get_lecture_summary を使う。」を切り取る

回答３

LoRAとQLoRAの違いは以下のとおりです。

* **LoRA**: 事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行列だけを効率よく学習する手法です。
* **QLoRA**: LoRAの仕組みをベースにしつつ、ベースモデルの重みを4ビットなどに量子化（ビット数を小さくしてメモリを節約）することで、さらに少ないGPUメモリで大きなモデルを扱えるようにした手法です。

関係する回：第2回

--- trace ---
[tool call]   search_glossary({'query': 'LoRA'})
[tool call]   search_glossary({'query': 'QLoRA'})
[tool return] search_glossary -> [{"term": "LoRA", "lecture": 2, "definition": "事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行列だけを学習するPEFT手法。学習するパラ…
[tool return] search_glossary -> [{"term": "QLoRA", "lecture": 2, "definition": "ベースモデルの重みを4ビットなどに量子化してメモリを節約したうえで、LoRAで学習する手法。より大きなモデルを小さなGPUで扱える。本講座の第2…
[usage] RunUsage(cost=Decimal('0.0007523'), details={'text_prompt_tokens': 1166}, input_text_tokens=1166, input_tokens=1166, output_tokens=161, requests=2, tool_calls=2)
------------------------------------------------------------------------------
４．実験条件： 「- 用語集に見つからないことは、推測で断定せず「用語集にはない」と伝える。」を切り取る。

回答４

**LoRA**と**QLoRA**は、どちらも大規模言語モデル（LLM）の効率的なファインチューニング（PEFT）を行うための手法です。主な違いは、**ベースモデルの重みの保持方法（精度）**にあります。

* **LoRA（Low-Rank Adaptation）**
  事前学習済みの重み（通常は16ビットなどの精度）をそのまま固定し、追加の低ランク行列（アダプタ）だけを学習することで、学習パラメータ数を大幅に削減する手法です。

* **QLoRA（Quantized LoRA）**
  LoRAの仕組みをベースにしつつ、**ベースモデルの重みを4ビットなどの低ビットに量子化**（圧縮）してから学習する手法です。これにより、モデルのメモリ使用量を劇的に減らし、より小規模なGPUでも巨大なモデルを学習できるようになります。

---

第2回（関連回：第2回）

--- trace ---
[tool call]   search_glossary({'query': 'LoRA'})
[tool call]   search_glossary({'query': 'QLoRA'})
[tool return] search_glossary -> [{"term": "LoRA", "lecture": 2, "definition": "事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行列だけを学習するPEFT手法。学習するパラ…
[tool return] search_glossary -> [{"term": "QLoRA", "lecture": 2, "definition": "ベースモデルの重みを4ビットなどに量子化してメモリを節約したうえで、LoRAで学習する手法。より大きなモデルを小さなGPUで扱える。本講座の第2…
[usage] RunUsage(cost=Decimal('0.0009438'), details={'text_prompt_tokens': 1146}, input_text_tokens=1146, input_tokens=1146, output_tokens=240, requests=2, tool_calls=2)

------------------------------------------------------------------------------
５．実験条件：「- 回答の最後に、関係する回（第N回）を示す。」を切り取る。

回答５

LoRAとQLoRAの主な違いは、**ベースモデルの重みをそのまま使うか、量子化してメモリをさらに削減するか**という点にあります。

講座の用語集によると、それぞれの定義は以下のとおりです：

* **LoRA（Low-Rank Adaptation）**
  * 事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行列だけを学習するPEFT（Parameter-Efficient Fine-Tuning）手法。学習するパラメータが少なく、学習結果（アダプタ）も小さいファイルで保存できます（第2回演習で扱います）。
* **QLoRA（Quantized LoRA）**
  * ベースモデルの重みを4ビットなどに**量子化してメモリを節約したうえで、LoRAで学習する手法**。より大きなモデルを小さなGPUで扱えるようにします。

まとめると、LoRAが「効率的なパラメータの学習」に焦点を当てているのに対し、QLoRAはそれに加えて「ベースモデル自体の軽量化（量子化）」を組み合わせることで、さらに少ないメモリ（VRAM）で大規模なモデルのファインチューニングを可能にする手法です。

--- trace ---
[tool call]   search_glossary({'query': 'LoRA'})
[tool call]   search_glossary({'query': 'QLoRA'})
[tool return] search_glossary -> [{"term": "LoRA", "lecture": 2, "definition": "事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行列だけを学習するPEFT手法。学習するパラ…
[tool return] search_glossary -> [{"term": "QLoRA", "lecture": 2, "definition": "ベースモデルの重みを4ビットなどに量子化してメモリを節約したうえで、LoRAで学習する手法。より大きなモデルを小さなGPUで扱える。本講座の第2…
[usage] RunUsage(cost=Decimal('0.0011086'), details={'text_prompt_tokens': 1162}, input_text_tokens=1162, input_tokens=1162, output_tokens=304, requests=2, tool_calls=2)

------------------------------------------------------------------------------
６．実験条件：「- 宿題の答えそのものは書かず、考え方のヒントで導く。」を切り取る
回答６

LoRAとQLoRAの違いは以下の通りです。

* **LoRA**: 
  事前学習済みの重みを固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行列だけを学習するPEFT（Parameter-Efficient Fine-Tuning）手法です。学習するパラメータが少なく、小さなファイルサイズでアダプタを保存できます。
* **QLoRA**: 
  ベースモデルの重みを4ビットなどに量子化してメモリを大幅に節約した上で、LoRAによる学習を行う手法です。これにより、より大きなモデルを小さなGPU環境でも扱えるようになります。

関係する回：第2回

--- trace ---
[tool call]   search_glossary({'query': 'LoRA'})
[tool call]   search_glossary({'query': 'QLoRA'})
[tool return] search_glossary -> [{"term": "LoRA", "lecture": 2, "definition": "事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行列だけを学習するPEFT手法。学習するパラ…
[tool return] search_glossary -> [{"term": "QLoRA", "lecture": 2, "definition": "ベースモデルの重みを4ビットなどに量子化してメモリを節約したうえで、LoRAで学習する手法。より大きなモデルを小さなGPUで扱える。本講座の第2…
[usage] RunUsage(cost=Decimal('0.0008143'), details={'text_prompt_tokens': 1156}, input_text_tokens=1156, input_tokens=1156, output_tokens=187, requests=2, tool_calls=2)
------------------------------------------------------------------------------
７．実験条件：docstringの「用語を１つだけ入れる」を「まとめて入れる」に変える。

回答７

**LoRA**と**QLoRA**の違いは、ベースモデルの重みをそのまま使うか、量子化してメモリを削減するかにあります。

* **LoRA（Low-Rank Adaptation）**
  事前学習済みの重みは固定したまま、重みの更新分を低ランク行列（2つの小さな行列の積）で近似し、その部分だけを学習するPEFT（Parameter-Efficient Fine-Tuning）手法です。学習パラメータや保存するファイルサイズを大幅に削減できます。

* **QLoRA（Quantized LoRA）**
  LoRAの仕組みに加え、ベースモデルの重みを4ビットなどに**量子化**してメモリ消費量をさらに劇的に削減したうえで学習を行う手法です。これにより、より大きなモデルを小さなGPU環境でも効率的に学習できるようになります。

関係する回：第2回

--- trace ---
[tool call]   search_glossary({'query': 'LoRA QLoRA'})
[tool return] search_glossary -> [{"term": "LoRA", "lecture": 2, "definition": "事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行列だけを学習するPEFT手法。学習するパラ…
[usage] RunUsage(cost=Decimal('0.0007755'), details={'text_prompt_tokens': 885}, input_text_tokens=885, input_tokens=885, output_tokens=204, requests=2, tool_calls=1)
------------------------------------------------------------------------------
