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

以下の 4 行を編集してください

```Makefile
PROJECT = mygame
CHR_FILES = image1.chr
WAV_FILES = se1.wav se2.wav
BGM_FILES = song1.bgm
```

- `PROJECT` ... プロジェクト名
- `CHR_FILES` ... バンク4（第5バンク）以降に追加される映像データ（.bmp から変換される .chr）ファイルを記述（複数可）
- `WAV_FILES` ... 効果音データ（.wav）ファイルを記述（複数可）
- `BGM_FILES` ... BGMデータ（.mml からコンパイルされる .bgm）ファイルを記述（複数可）

### 3. プログラムの記述

- [./src/program.c](./src/program.c) にゲームのメインコードを記述してください
- 初期状態は Hello, World! (BGM & SE 付き) です
