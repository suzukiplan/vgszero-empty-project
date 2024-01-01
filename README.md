# VGS-Zero Empty Project

VGS-Zero のゲームを開発するための初期プロジェクトです。

## 前準備

### macOS

- XCODE
- `brew install sdl2`
- [SDCC 4.1.0](https://sourceforge.net/projects/sdcc/files/sdcc-macos-amd64/4.1.0/)をインストールしてパスを切る

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

## 解説

- [vgszero](https://github.com/suzukiplan/vgszero) をサブモジュールとして追加
  - `git submodule add https://github.com/suzukiplan/vgszero`
- [./Makefile](./Makefile) でサブモジュールの初期化、ツールチェインのビルド、[./src](./src/)のビルドを実行
- [./src](./src/) ディレクトリ以下がプロジェクトのモジュール
  - [./src/Makefile](./src/Makefile) ... メイクファイル
  - [./src/program.c](./src/program.c) ... プログラム
  - [./src/song1.mml](./src/song1.mml) ... MML (BGM ソース)
  - [./src/se1.wav](./src/se1.wav) ... 効果音1
  - [./src/se2.wav](./src/se2.wav) ... 効果音2

## 使い方

### 1. セットアップ

```bash
# 本リポジトリを mygame として取得
git clone https://github.com/suzukiplan/vgszero-empty-project mygame

# ディレクトリ移動
cd mygame

# .git を削除
rm -rf .git

# ./src/Makefile を編集して PROJECT を自分のプロジェクト名に変更するなど
vi ./src/Makefile

# README.md をクリア
echo "# mygame" >README.md

# .git を初期化
git init
git add -A
git commit -m "initial commit"

# ビルド
make
```

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
