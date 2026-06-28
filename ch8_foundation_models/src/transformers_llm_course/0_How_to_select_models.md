### Hugging Faceのタグ名で見るべきポイント

モデルを探すときは、[公式](https://huggingface.co/models)でタグ名検索するのが基本。

例えば`Llama-3.2-3B-Instruct`

- `Llama`: モデル名
- `3.2`: モデルのバージョン
- `3B`: モデルのサイズ
- `Instruct`: 対応タスク

モデルのサイズと対応タスクについてもう少し詳細に解説します

#### モデルのサイズ

モデルのサイズと必要VRAMは、大まかに以下のような関係があります

- FP16：約 2GB / 1Bパラメータ
- INT8：約 1GB / 1Bパラメータ
- 4bit（量子化）：約 0.5GB / 1Bパラメータ

実際はKVキャッシュやオーバーヘッドで上記よりもVRAMを使うので、30%ほど余裕を見て以下くらいが目安になると思います

| モデルサイズ | FP16での必要VRAM    | 4bit量子化での必要VRAM |
| ------ | ------------------- | -------- |
| 3B     | 6GB → **8GB程度必要**   | 2GB      |
| 7B     | 14GB → **16GB必要**   | 4GB      |
| 13B    | 26GB → **30GB必要**   | 8GB      |
| 70B    | 140GB → **複数GPU必須** | 35GB     |

#### 対応タスク

||タグ名の例|`Tasks`の選択肢|例|用途|
|---|---|---|---|---|
|Baseモデル|(なし)|`Text Generation`等|"google/gemma-2-2b"|研究・事前学習（自分でファインチューニングが必要）|
|Instructモデル|`Instruct`、`it`、`Chat`等|`Text Generation`または`Image-Text-to-Text`|"google/gemma-2-2b-it"|対話（Instruction Tuningされている）|
|Visionモデル|`Vision`、`VL`等（最近はタグのないVision対応モデルが多い）|`Image-Text-to-Text`|"google/gemma-2-2b-it"|画像解釈|

チャットのようなテキスト生成をしたいのであれば、とりあえず`Instruct`にしておけばOK（Gemmaだと`it`）なので、検索時に`Tasks`の`Text Generation`（LLM）または`Image-Text-to-Text`（VLM）を選択すればOKです（最近はデフォルトでVLMになっているモデルが多いので、テキスト生成でも`Image-Text-to-Text`が適切なケースもあります）。

### おすすめモデル

Hugging Face内の使いやすいInstructionモデル（Instruction Tuningされたモデル）のうち、いいねが多いものを、VRAMサイズごとに以下に示します（同モデルでも量子化の有無で重複があることにご注意ください）。

#### 8GB VRAM (RTX5060等)

量子化なしで3Bくらい、量子化ありで10Bくらいが上限

|`model`引数に指定する文字列|いいね数|備考|
|---|---|---|
|"microsoft/Phi-3-mini-4k-instruct"|1.41k||
|"Qwen/Qwen2.5-1.5B-Instruct"|659||
|"google/gemma-2-2b-it"|1.33k||
|"meta-llama/Llama-3.2-3B-Instruct"|2.09k||
|"mistralai/Mistral-7B-Instruct-v0.2"|3.11k|4bit量子化必要|
|"Qwen/Qwen2.5-7B-Instruct"|1.19k|4bit量子化必要|
|"meta-llama/Llama-3.1-8B-Instruct"|5.67k|4bit量子化必要|

#### 24GB VRAM (RTX4090等)

量子化なしで10Bくらい、量子化ありで30Bくらいが上限

|`model`引数に指定する文字列|いいね数|備考|
|---|---|---|
|"mistralai/Mistral-7B-Instruct-v0.2"|3.11k||
|"Qwen/Qwen2.5-7B-Instruct"|1.19k||
|"meta-llama/Llama-3.1-8B-Instruct"|5.67k||
|"google/gemma-3-12b-it"|703||
|"google/gemma-3-27b-it"|1.95k|4bit量子化必要|
|"google/gemma-4-31b-it"|1.53k|4bit量子化必要|

#### 96GB VRAM (RTX PRO 6000 Blackwell等)

量子化なしで40Bくらい、量子化ありで120Bくらいが上限

|`model`引数に指定する文字列|いいね数|備考|
|---|---|---|
|"google/gemma-3-27b-it"|1.95k||
|"google/gemma-4-31b-it"|1.53k||
|"meta-llama/Llama-3.3-70B-Instruct"|2.69k|4bit量子化or複数GPU必要|
|"Qwen/Qwen2.5-72B-Instruct"|925|4bit量子化or複数GPU必要|
