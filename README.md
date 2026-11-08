# Auto Sleeper

Windows向けのスリープタイマーアプリケーションです。

タスクトレイに常駐し、指定した時間が経過すると自動的にPCをスリープ状態にします。通知機能やタイマーの延長機能を備えており、作業中の時間管理に利用できます。

## Features

- **スリープタイマー**
  - デフォルトの時間を指定してタイマーを開始
  - 設定した候補からタイマー時間を選択
  - タイマーのキャンセル
- **タイマー延長**
  - 設定した時間単位でタイマーを延長
  - スリープ予定時刻が延長時間より近い場合のみ延長可能
  - 延長時間の上限はデフォルト時間の2倍
- **通知**
  - スリープ予定の3分前に通知
  - スリープ後にPCを復帰させた際の通知
- **タスクトレイ**
  - 残り時間をアイコンのツールチップに表示
  - 残り時間に応じてアイコンを変更
- **設定ファイル**
  - TOML形式による各種設定の変更

## Requirements

- Windows
- Python 3.11 以降

### Dependencies

```bash
pip install pystray pillow winotify
```

※ `tomllib` はPython 3.11以降の標準ライブラリです。

## Installation

### Run from source

1. リポジトリをクローンします。

```bash
git clone <repository-url>
cd Auto-Sleeper
```

2. 必要なライブラリをインストールします。

```bash
pip install -r requirements.txt
```

3. アプリケーションを実行します。

```bash
python main.py
```

※ 実際のエントリーポイント名に合わせて `main.py` を変更してください。

### Directory structure

```text
Auto-Sleeper/
├── images/
│   ├── default.png
│   ├── not_started.png
│   ├── 3min.png
│   ├── icon.ico
│   └── 3min.ico
├── main.py
└── README.md
```

## Usage

アプリケーションを起動すると、タスクトレイにAuto Sleeperのアイコンが表示されます。

アイコンを右クリックすると、以下のメニューを利用できます。

| Menu | Description |
|---|---|
| Wanna exit | 終了・即時スリープなどの操作 |
| Open setting file | 設定ファイルを開く |
| Scan | 設定を再読み込み |
| Cancel timer | 実行中のタイマーをキャンセル |
| Start timer | デフォルト時間でタイマーを開始 |
| Start timer (select time) | 指定した候補から時間を選択 |
| Extend time | タイマーを延長 |

### Timer

`Start timer` を選択すると、設定されたデフォルト時間でタイマーが開始されます。

`Start timer (select time)` からは、設定ファイルに登録した時間を選択できます。

タイマー実行中は、タスクトレイアイコンのツールチップに残り時間が表示されます。

### Extend time

`Extend time` から指定した時間を追加できます。

ただし、現在の残り時間が延長時間以下の場合は延長できません。

また、延長後の残り時間には上限があります。

### Notifications

3分前通知が有効になっている場合、スリープ予定時刻が近づくと通知が表示されます。

## Configuration

設定ファイルは以下の場所に作成されます。

```text
%APPDATA%\AutoSleeper\setting.toml
```

初回起動時に設定ファイルが存在しない場合は、デフォルト設定が使用されます。

### Example

```toml
# Default sleeping time (minutes)
sleeping_time = 10

# Selection of extend time (minutes)
extend_time = [5]

# Selection of timer time (minutes)
timer_time = [5]

# Enable/disable three-minute notification
three_min_notification = false

# Extend the previous timer
extend_last_time = false
```

### Configuration options

| Option | Type | Description |
|---|---|---|
| `sleeping_time` | Integer | デフォルトのタイマー時間（分） |
| `extend_time` | Integer array | 延長時間の選択肢（分） |
| `timer_time` | Integer array | タイマー時間の選択肢（分） |
| `three_min_notification` | Boolean | 3分前通知の有効・無効 |
| `extend_last_time` | Boolean | 前回の残り時間を考慮したタイマー時間の延長 |

設定を変更した場合は、タスクトレイの `Scan` を選択して設定を再読み込みしてください。

## Notes

- 本アプリケーションはWindows向けです。
- PCのスリープにはWindows標準の `SetSuspendState` を使用します。
- スリープの実行結果は、Windowsの電源設定やハードウェアの状態などに影響される場合があります。
- スリープ中は通常、タイマーによる処理も停止します。
- 作業中のデータは、スリープ前に保存することを推奨します。
- 
