# Dishonored JPN

Steam版 *Dishonored: Definitive Edition* の字幕・UI文言・フォントを日本語化し、未翻訳テキストを編集できる支援ツールです。DLC 05〜07も処理対象です。

> **注意**: UPKのパッチはゲームファイルを変更します。実行前にゲームフォルダをバックアップしてください。Steamの「ファイルの整合性を確認」で元へ戻すこともできます。

## 機能

- UPK内の字幕を日本語DBで差し替え
- 未翻訳の英語字幕をTSVへ抽出、GUIで編集、DBへ再取り込み
- `.int` の未翻訳キーをTSVへ抽出、GUIで編集、INTへ反映
- 日本語フォントUPKと日本語INTをゲームへ配置
- INT・フォント適用前の日時付き自動バックアップ
- GUIとCLIに対応

## 動作環境

- Windows
- Steam版 Dishonored
- 配布ZIPを展開できる空き容量

配布版の利用に Python・uv のインストールは不要です。`decompress.exe` と必要な実行時ファイルは配布フォルダに同梱されています。

## 起動と配置

ZIPを任意の場所へ展開し、**フォルダ内のファイルを分離・削除せず** `DishonoredJPTool.exe` を実行してください。EXE単体では動作しません。

```text
Dishonored_JP_PatchTool\
├─ DishonoredJPTool.exe
├─ decompress.exe
├─ db\
├─ INT\
├─ Font\
├─ tcl\, tk\ などの実行時ファイル
├─ README.md
├─ LICENSE
├─ LICENSE-TRANSLATION-DATA.md
├─ THIRD_PARTY_NOTICES.md
└─ TERMS_AND_DISCLAIMER.md
```

起動後、GUIでDishonoredのゲームフォルダと基準日本語DBを確認してから操作してください。ゲームフォルダの選択例:

```text
D:\SteamLibrary\steamapps\common\Dishonored
```

### DBの役割

| ファイル | 用途 |
| --- | --- |
| `dis.db` | UPKオブジェクトとGUIDの対応表 |
| `upklist.db` | パッチ対象UPK一覧 |
| `texts.db` | 初期データとして保持。ツールは自動上書きしない |
| `texts_ja_compatible.db` | 基本となる日本語字幕DB |
| `texts_ja_edited.db` | TSVで追加した翻訳を統合したDB |

## GUIでの操作（推奨）

`DishonoredJPTool.exe` をダブルクリックして起動します。Windowsの保護画面が出る場合は、配布元とファイルのハッシュを確認したうえで利用してください。

### 字幕の未翻訳を追加する

1. **未翻訳をTSVへ出力** を実行します。
2. **TSV翻訳エディターを開く** を押します。
3. GUID、UPK、オブジェクト、英語セリフを確認し、日本語セリフ欄を編集して保存します。
4. **編集TSVをDBへ反映** を実行し、`texts_ja_edited.db` を作ります。
5. 基準日本語DBに `texts_ja_edited.db` を選び、**パッチを適用** を実行します。

翻訳を追加しない場合は、`texts_ja_compatible.db` を選んで直接パッチできます。

### INT・フォントを適用する

1. **INT未翻訳TSVを出力** を実行します。
2. **INT翻訳エディターを開く** で、ファイル名・セクション・キー・英語・日本語を編集します。
3. **INT編集を反映してフォント・INTを適用** を実行します。

上書き対象は次へ退避されます。

```text
DishonoredJPN\backups\YYYYMMDD-HHMMSS\
```

## よくある問題

### 編集したセリフが英語のまま

`texts_ja_edited.db` を作っただけではゲームへ反映されません。そのDBを基準日本語DBに選び、**パッチを適用**してください。`log.txt` にUPKごとの処理結果が出ます。

### 日本語フォントが表示されない

`Font` の2つのUPKがあることを確認し、GUIからINT・フォント適用を実行してください。

## 仕組み

`dis.db` はUPK内の字幕オブジェクトとGUIDの対応を持ちます。ツールは翻訳テキストをUTF-16LE・NUL終端で生成し、UPK末尾へ追記します。ExportTableには新しいサイズとオフセットをリトルエンディアンで書き戻します。ファイル全体を元サイズに合わせる方式ではありません。

## 謝辞

DGOTYCNv1.4の解析と、[awgs氏によるDLC字幕日本語化の記事](https://awgsfoundry.com/blog-entry-541.html) を参考にしています。

## ライセンス・利用条件

- プログラム本体のソースコードおよび実行ファイルは [MIT License](LICENSE) で公開します。
- このツールで**新たに作成した翻訳文**の利用条件は [LICENSE-TRANSLATION-DATA.md](LICENSE-TRANSLATION-DATA.md) を参照してください。
- ゲーム資産、フォント、INTはこの許諾の対象外です。初期DBの元となった既存日本語化データは、元翻訳者「ファックマン」氏から本プロジェクトでの作成・利用に関する明示許諾を得ています。配布時はクレジットと許諾条件を保持してください。
- `decompress.exe` を同梱する場合は [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) のライセンス通知を必ず含め、同じ実行ファイルのライセンスであることを確認してください。
- 利用時のバックアップ、無保証、責任範囲は [TERMS_AND_DISCLAIMER.md](TERMS_AND_DISCLAIMER.md) を参照してください。
