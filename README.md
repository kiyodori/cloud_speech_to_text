## これは何？
動画ファイルを文字起こしする

- 発話内容を文字に起こす
- 話者を区別する

## どう使うの？
1. 事前準備
必要なライブラリをインストールする。

```
$ brew install ffmpeg sox
$ gem install dotenv

# gsutilのインストール
$ curl https://sdk.cloud.google.com | bash
$ exec -l $SHELL
$ gcloud init
```

※ `gcloud init`のところで、プロジェクト作ったりごにょごにょする

Google Cloud Platform の Google Cloud Storage でバケット作成する。

- 個別のオブジェクトが権限を設定できるようにして、バケットを作成する
- 合わせて`Rakefile`を編集して、アップロードするバケット先を変更する

Google Cloud Platform のAPIとサービスからAPIキーを作成する。

- 合わせて`.env`ファイルを`.evn_sample`ファイルを元に作成してAPIキーを記載する

Google Cloud Platform の Cloud Speech-to-Text API を該当プロジェクトで有効にする。

2. `mov`ファイルを`input.MOV`というファイル名で`input/`ディレクトリ配下に設置

3. `mov`ファイルを`flac`ファイルに変換

```
$ rake cloud_speech_to_text:encode
```

4. Google Cloud Storage に`flac`ファイルをアップロード

```
$ rake cloud_speech_to_text:upload
```

5. 文字起こし

```
$ rake cloud_speech_to_text:request
```
