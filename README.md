# VGS-Zero Empty Project

VGS-Zero のゲームを開発するための初期プロジェクトです。

## 対応機種

VGS-Zero のゲーム開発は Linux (推奨) または macOS に対応しています。

## 前準備

### Linux

```bash
# install GNU Make, GCC and other
sudo apt install build-essential

# install SDL2
sudo apt-get install libsdl2-dev

# install ALSA
sudo apt-get install libasound2
sudo apt-get install libasound2-dev
```

[SDCC 4.1.0](https://sourceforge.net/projects/sdcc/files/sdcc-linux-amd64/4.1.0/) をインストールしてパスを切る。

> apt や apt-get で sdcc をインストールすると対応していないバージョン（4.1.0以外）がインストールされる可能性があるため、上記リンクからダウンロードしたものを使用してください。

### macOS

- XCODE
- `brew install sdl2`
- [SDCC 4.1.0](https://sourceforge.net/projects/sdcc/files/sdcc-macos-amd64/4.1.0/)をインストールしてパスを切る

> HomeBrew で sdcc をインストールすると対応していないバージョン（4.1.0以外）がインストールされる可能性があるため、上記リンクからダウンロードしたものを使用してください。（ただし、未署名バイナリなので Finder で control ボタンを押しながら開き1ファイルづつ実行の許可をする必要があり面倒くさいので頑張ってください）

## 使い方

### 1. セットアップ

```bash
# 本リポジトリを mygame として取得
git clone https://github.com/suzukiplan/vgszero-empty-project mygame

# ディレクトリ移動
cd mygame

# .git を削除
rm -rf .git

# submodule を一旦削除
rm .gitmodules
rm -rf vgszero

# ./src/Makefile を編集して PROJECT を自分のプロジェクト名に変更するなど
vi ./src/Makefile

# README.md をクリア
echo "# mygame" >README.md

# .git を初期化
git init

# サブモジュールを追加
git submodule add https://github.com/suzukiplan/vgszero

# commit
git add -A
git commit -m "initial commit"

# ビルド
make
```

- make が完了すると SDL2 版 VGS-Zero がビルドされた game.pkg を起動します
- 初回 make には少し時間が掛かりますが 2 回目以降は高速になります

### 2. [./src/Makefile](./src/Makefile) の編集方法

編集が必要な箇所は先頭の 5 行（以下）のみです。

```Makefile
PROJECT = mygame
SRC_FILES = main.rel table.rel
CHR_FILES = image0.chr image1.chr image2.chr
WAV_FILES = se0.wav se1.wav se2.wav
BGM_FILES = song0.bgm song1.bgm song2.bgm
```

- `PROJECT` ... プロジェクト名
- `SRC_FILES` ... C言語のプログラム（.c）からコンパイルされるオブジェクトファイル（.rel）を記述（複数可）
- `CHR_FILES` ... バンク4（第5バンク）以降に追加される映像データ（.bmp から変換される .chr）ファイルを記述（複数可）
- `WAV_FILES` ... 効果音データ（.wav）ファイルを記述（複数可）
- `BGM_FILES` ... BGMデータ（.mml からコンパイルされる .bgm）ファイルを記述（複数可）

ファイル数が多くて 1 行で記述するのが大変な場合、次のように定義することもできます。

```Makefile
SRC_FILES = main.rel
SRC_FILES += table.rel
SRC_FILES += hoge.rel
SRC_FILES += hige.rel
SRC_FILES += hage.rel
```

### 3. 開発のヒント

#### (初期状態のファイル構成)

- [./src/main.c](./src/main.c) ... プログラムのメインコード
- [./src/table.c](./src/table.c) ... 定数テーブル（constグローバル変数）の実体定義など
- [./src/header.h](./src/header.h) ... 共通ヘッダーファイル
- 初期状態は Hello, World! (BGM & SE 付き) になっています

#### (ソースファイルの追加手順)

1. [./src/Makefile](./src/Makefile) の `SRC_FILES` に追加ファイルの拡張子（.c）を .rel にしたものを追加
2. [./src] ディレクトリにソースファイル（.c）を追加
3. 追加ソースで定義する関数プロトタイプなどを [./src/header.h] に定義

#### (効果音の追加手順)

1. [./src/Makefile](./src/Makefile) の `WAV_FILES` に効果音ファイル（.wav）を追加
2. [./src] ディレクトリに効果音ファイル（.wav）を追加
3. `vgs0_se_play` に指定する効果音番号は `WAV_FILES` の指定位置（0スタート）です

#### (BGMの追加手順)

1. [./src/Makefile](./src/Makefile) の `BGM_FILES` に追加する MML ファイルの拡張子（.mml）を .bgm にしたものを追加
2. [./src] ディレクトリに MML ファイル（.mml）を追加
3. `vgs0_bgm_play` に指定する BGM 番号は `BGM_FILES` の指定位置（0スタート）です
