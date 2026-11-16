# ocr

ロボット設計製作論 2026 チーム Jubilant の OCR 班のリポジトリです。
四隅に ArUco マーカーを印刷した紙をカメラで見つけ、真上から見た画像に補正して Claude で日本語の要点にし、VOICEVOX で読み上げます。

## 中身

| フォルダ | 内容 |
|---|---|
| [`ocr_recognizer`](ocr_recognizer) | ROS 2 パッケージ（`ocr_node`）。カメラ画像 → ArUco 補正 → Claude → VOICEVOX → `/ocr/done`。OCR エンジンの比較結果（`benchmark/`）とテスト用の紙（`test_docs/`）もここ |
| [`ocr_ai`](ocr_ai) | Claude で翻訳・要約する実験（Python と Rust） |
| [`ocr_voicevox`](ocr_voicevox) | VOICEVOX で音声を作る実験 |
| [`compose.yaml`](compose.yaml) | `ocr_node` と VOICEVOX ENGINE を Docker でまとめて動かす設定 |

## 置き場所

ROS 2 Jazzy のワークスペースの `src/` の下に clone します。

```bash
mkdir -p ~/ros2/jubilant_ws/src
git clone https://github.com/2026-robodesign-jubilant/ocr.git ~/ros2/jubilant_ws/src/ocr
```

準備・ビルド・実行の手順は [`ocr_recognizer/README.md`](ocr_recognizer/README.md) にあります。

## Docker でまとめて動かす

```bash
cd ~/ros2/jubilant_ws/src/ocr
read -rs ANTHROPIC_API_KEY && export ANTHROPIC_API_KEY
docker compose up --build
```

ほかの班のノードと通信できるように、`network_mode: host` と `ipc: host` を使い、`ROS_DOMAIN_ID` をそろえます。詳しくは [`ocr_recognizer/README.md`](ocr_recognizer/README.md) の「Docker で動かす」を見てください。

## ライセンス

[BSD 3-Clause](LICENSE)。音声を人に聞かせるときは「VOICEVOX:ずんだもん」のようにクレジットを表示してください。
