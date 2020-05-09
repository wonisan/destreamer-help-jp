# destreamerの使い方(日本語要約)

destreamerとは、Microsoft Streamの動画を保存し、オフライン環境でも視聴できるようにするためのツールです。
  
## 免責事項
destreamerを利用したことによる責任について、製作者、もしくはこの翻訳者は、一切負いません。  
最新の情報は、destreamerのrepoを確認してください。  
この内容は、公式文章の翻訳(意訳含む)の他に、公式[README.md][README.md]に記載のない情報も含まれています。  

## 前提条件
- [**Node.js**][node]: バージョンは8.0以降である必要があります。利用するNodeが、Node 8の場合は`Parse Error`の`code: HPE_HEADER_OVERFLOW`を受け取るかもしれません。その際は、Node 10以上にアップデートし、再度ビルドをしてください。
- **npm**: 通常Node.jsインストール時に入っています。コマンドラインで `npm` と入力してインストールしているか確認してください。
- [**ffmpeg**][ffmpeg]: 2019年以降の最新バージョンである必要があります。パスを通すか、 プロジェクトのルートディレクトリに入れてください。
- [**git**][git]: npmの依存関係。

## 動作環境
Win/Mac/Linux で動作します。

Windows
-  `destreamer.cmd` (コマンドプロンプト)か、  
`destreamer.ps1`(PowerShell) で実行してください。  
改行している場合、エスケープ文字も必要です。  
PowerShellはバックティック[ ` ]を使用し、cmd.exeはキャレット[ ^ ]を使用します。
- MinGWや、MSYS、Cygwinでは動作しないことがあります。  
(一部ユーザーからは、出来ないとの報告が上がっています。)
- WSLでは動作しません。 詳しくは、公式[README.md][README.md]に記載してあります。
- 管理者権限で実行すると動作しないことがあります。

Mac/Linux
- `destreamer.sh`(ターミナル)で実行してください。  
改行している場合には、エスケープ文字も必要です。  
- rootユーザーの場合、動作しないことがあります。

## ビルド方法
destreamerをビルドするには、repoのcloneを作成。  
前提条件のソフトをインストールした後npmでビルドするだけです。  
以下に入力するコマンドを記します。  

```sh
$ echo +++++前提条件をここまでに整えておくと手間が省けます。+++++
$ git clone https://github.com/snobu/destreamer
$ cd destreamer
$ npm install
$ npm run build
```

## オプションを翻訳すると…

```
$ ./destreamer.sh

オプション:
  --help                   ヘルプを表示します
  --version                バージョンを表示します
  --videoUrls, -i          このオプションの後にURLを入力します
  --videoUrlsFile, -f      このオプションの後にURLを記載したtxtファイルへのパスを
                           入力します
  --username, -u           "username@example.com"の形式でユーザー名を入力します
  --outputDirectory, -o    出力ファイルを配置するディレクトリを変更します
                          （無指定の場合、プロジェクトファイル配下の`videos`に
                            そのまま保存します）
  --outputDirectories, -O  出力ファイルを配置するディレクトリを変更します
                          （1リンクごとに、一つ新たにディレクトリを作成し保存します）
  --noExperiments, -x      コンソール上にサムネイル画像の表示しません　
                                                              [default: 無効(F)]
  --simulate, -s           ビデオのダウンロードを行わず、メタデータのみを表示します。
                                                              [default: 無効(F)]
  --verbose, -v            追加の情報(debug情報)を表示し、実行します
                                                              [default: 無効(F)]
  --noCleanup, --nc        FFmpegで出力エラーが発生した際は、
                           自動的にファイルを消しません         [default: 無効(F)]
                         
  --vcodec                動画を再エンコードする際のFFmpegコーディックを指定します
                          (ex. libx265) もしくは"none" と指定して動画を無効にします
                                              [default: "copy"(再エンコードなし)]
  --acodec               音声を再エンコードする際のFFmpegコーディックを指定します
                           (ex.libopus) もしくは"none" と指定して音声を無効化します
                                              [default: "copy"(再エンコードなし)]
  --format                出力時のビデオコンテナのフォーマットを指定します。  
                         (対応フォーマット:mkv, mp4, movなどFFmpegがサポートする形式
                                                                [default: "mkv"]
  --skip                  ファイルを既にダウンロードしている場合はスキップします。
                                                                [default: 無効(F)]
```

##  For example(Mac/Linux)
Windowsでは、
`./destreamer.sh` → `destreamer.ps1` or `destreamer.cmd`に置き換えてください。

**最小実行コマンド**
```sh
$ ./destreamer.sh -i "https://web.microsoftstream.com/video/VIDEO-1"
```

**ユーザー名を自動入力するには…**
```sh
$ ./destreamer.sh -u user@example.com -i "https://web.microsoftstream.com/video/VIDEO-1"
```

**ビデオを任意の場所に保存するには…**
```sh
$ ./destreamer.sh -i "https://web.microsoftstream.com/video/VIDEO-1" -o /Users/hacker/Downloads
```

**2つ以上の動画をダウンロードするには…**
```sh
$ ./destreamer.sh -i "https://web.microsoftstream.com/video/VIDEO-1" \
                     "https://web.microsoftstream.com/video/VIDEO-2"
```
- エスケープ文字も必ず入力してください。

**txtファイルからURLを入力するなら…**
```sh
$ ./destreamer.sh -f list.txt
```

- `.txt`のURLリンクは一行につき1リンクのみです。
- エスケープ文字を忘れないで下さい。

**---Trips---**
- デフォルトの出力コンテナは`.mkv`です。ほかのコンテナを利用したい場合(ex.`mp4`)は、  
ダウンロードする際`--format mp4`と指定してください。

- 念のため、`--username` は任意です。  
ただユーザー名が自動入力されるだけです。  

- `-o`では、絶対パスで入力してください。  
ex. `/mnt/videos`(Mac/Linux).  
    `%USERPROFILE%/destreamer/videos` (Win)  

- ダウンロードしたファイルは、デフォルトで`videos/`に保存されます。
`-o`とパスを指定することで任意の場所に保存することができます。

## バグを報告 / アイデアを発表
まずは、[README.md][README.md]を確認して次のことを確認してください。
- 前提条件は、一つも不適合がありませんか？
- npmで正しくビルドされていますか？
- コマンドを実行する際は、  
　WindowsではcmdかPowerShell、  
　MacやLinuxでは、ターミナルで実行してください。
- コマンド実行時の構文は間違っていませんか？
- そのURLは、対応サービス(MS Stream)ですか？
- 利用しているアカウントは適切なアクセス権が付与されていますか？
- Streamをダウンロードする際は、  
安定したネットワークでないとダウンロードが失敗する場合があります。
- MS Streamなどのネットワークサービスは予告なく仕様が変更となり、  
 動作しない場合があります。  
destreamerを最新版に更新し、再度実行してみてください。

それでもバグが発生した場合は[**ここ**][feedback]をクリックして、
同じ内容の報告がないか確認してから報告してください。  
`--verbose, -v`オプションで実行し、  
このログとともに報告するとスムーズな対応ができるかもしれません。  
  
なおこの文章の誤りは、[**ここ**][help]に報告してください。  
私は先の**ビッグイベント**で日々忙しいので、対応が遅れるかもしれません。

[ffmpeg]: https://www.ffmpeg.org/download.html
[xming]: https://sourceforge.net/projects/xming/
[node]: https://nodejs.org/en/download/
[git]: https://git-scm.com/downloads
[wsl]: https://github.com/snobu/destreamer/issues/90#issuecomment-619377950
[README.md]: https://github.com/snobu/destreamer/blob/master/README.md
[feedback]: https://github.com/snobu/destreamer/issues
[help]: https://github.com/wonisan/destreamer-help-jp/issues
