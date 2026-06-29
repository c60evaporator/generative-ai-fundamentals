# LangChain環境構築

## LangChainのインストール

まずLangChainをインストール（よりハイレベルなAPIを提供するdeepagentsも一緒にインストールしておくとgood）

```bash
pip install langchain, deepagents
```

使用するAIモデルのAPI提供元に合わせて、[こちら](https://docs.langchain.com/oss/python/integrations/providers/overview)から統合パッケージを選択してインストールしてください。例えばGemini（Google GenAI）を使用する場合、以下のように`langchain-google-genai`をインストールします。

```bash
pip install langchain-google-genai
```

ローカルLLMを使用したい場合、以下のようにHuggingFaceの統合パッケージ、およびTransformers関係のパッケージをインストールします。

```bash
pip install langchain-huggingface transformers accelerate bitsandbytes
```

## APIキーの設定

### Google GenAIの場合

Google AI Studioに入り、左下の鍵マーク（Get API Key）を押して、右上の「APIキーを作成」をクリック。「インポートしたプロジェクトを選択」は基本的にDefault Gemini ProjectでOK（プロジェクトを分けたい場合は個別設定）

作成が終わったらAPIキーをコピーし、どこかに控えておく（他人に見られないよう注意）。
以下のようにAPIキーを環境変数に設定

```bash
export GOOGLE_API_KEY="コピーしたAPIキー"
```

### 動作確認

### Gemini

このリポジトリの`src/langchain_docs/0_2_quickstart_gemini.ipynb`を実行して"It's always sunny in San Francisco!"と帰ってくれば成功。エージェントに渡した関数をLLMが参照できていることを示す。

### ローカルLLM（HuggingFace）

このリポジトリの`src/langchain_docs/0_2_quickstart_huggingface.ipynb`を実行する。恐らくtool callにうまく対応できず`get_weather`関数を実行できないため、"I don't have real-time data access"的な文章が返ってくると思うが、動作確認としてはこれでOK
なおLangChain公式のQuickStartは古いバージョンのLangChain APIで記述されており動かないので注意。

