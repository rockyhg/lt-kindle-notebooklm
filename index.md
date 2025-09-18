---
layout: default
---

# NotebookLM 学習活用法

**動画も書籍も、すべてを使える知識に変える活用術**

発表者: はがたす

---
## 学習のあるあるな悩み

- 学習したい動画や書籍がたまっていく一方
- 動画や書籍で学んだことを、実際の仕事や生活で活かせない
- どこが重要だったか忘れてしまう

### NotebookLM で解決！

**公式サイト:** https://notebooklm.google.com/

- **ソース至上主義:** ユーザーが与えた資料**だけ**をもとに回答
- **引用元を表示:** 回答に引用元を明示
- **対話型:** 質問を重ねて深掘り可能
- **無料:** 無料プランでも十分に使える

---
## 【デモ】はがたす式NotebookLMの使い方

1. **ソース投入:** YouTube動画のリンクやPDFをアップロード
2. **概要説明資料 生成:** 全体像をつかむ
3. **マインドマップ 生成:** 視覚的に構造を理解
4. **マインドマップの要素をクリック:** 詳細情報を確認
5. **チャットで深堀り:** 質問を重ねて理解を深める
6. 「**テスト**」「**フラッシュカード**」もいい感じ（2025年9月アップデート）

---
## どうやって書籍をPDF化してる？
### 【デモ】Kindle本をPDFに変換してNotebookLMに取り込むTIPS

#### 必要なもの:
- Mac
- Kindle本
- Kindle for Mac
- スクリプトエディタ（Mac標準アプリ）

#### 手順
1. **Apple Script** でKindleアプリを操作して、ページを自動スクショ
2. **クイックアクション** > **PDFを作成**
3. **NotebookLMにアップロード**

---
## ⚠️ 超重要：著作権について
**必ず、個人利用の範囲で！** ⚖️

#### **⭕️ OKなこと**
自分が**正規に購入した**書籍などを、**自分自身が学習で使うため**にPDF化すること（私的利用）

#### **❌️ 絶対にNGなこと**
作成したPDFを**他人に渡したり、ネットにアップロード**すること（著作権侵害！）

---
## まとめ
- NotebookLMは学習のゲームチェンジャー

---
## 【参考】自動スクショ撮影スクリプト（Mac用）

- Macの **スクリプトエディタ** に以下のコードを貼り付けて実行

```applescript
-------------------------------------------------------------
-- Kindle自動撮影＆画像保存スクリプト
-- (スペースキーでページめくり)
-------------------------------------------------------------

-- ◆ STEP 1: 初期設定をユーザーに尋ねる ◆
-- -----------------------------------------------------------
try
	set outputFolder to choose folder with prompt "スクリーンショットの保存先フォルダを選択してください。"
	set totalPages to text returned of (display dialog "撮影する総ページ数を入力してください（例: 350）：" default answer "") as integer
	set filePrefix to text returned of (display dialog "保存ファイル名の先頭に付ける名前を入力してください（例: 本のタイトル）：" default answer "MyBook")

on error number -128
	return
end try

-- ◆ STEP 2: Kindleアプリを操作して撮影を実行 ◆
-- -----------------------------------------------------------
try
	tell application "Amazon Kindle" to activate
	delay 2
	set outputFolderPosix to POSIX path of outputFolder

	tell application "System Events"
		tell process "Kindle"
			set targetWindow to window 1
			if not (exists targetWindow) then
				error "ウィンドウが見つかりませんでした。アクセシビリティ権限が有効か再確認してください。"
			end if

			repeat with i from 1 to totalPages
				set {x, y} to position of targetWindow
				set {w, h} to size of targetWindow

				set pageNumber to text -3 thru -1 of ("000" & i)
				set fileName to filePrefix & "-" & pageNumber & ".png"

				set fullPosixPath to outputFolderPosix & fileName
				set quotedPosixPath to quoted form of fullPosixPath

				do shell script "screencapture -x -t png -R " & x & "," & y & "," & w & "," & h & " " & quotedPosixPath

				-- ===== ここが今回の修正部分 =====
				-- スペースキーを押してページをめくる
				key code 49
				-- ===== ここまでが今回の修正部分 =====

				delay 1
			end repeat
		end tell
	end tell

	-- ◆ STEP 3: 完了メッセージ ◆
	-- -----------------------------------------------------------
	display notification "全" & totalPages & "ページの保存が完了しました。" with title "Kindle撮影完了！" subtitle "指定のフォルダを確認してください。" sound name "Glass"

on error errMsg number errNum
	display dialog "エラーが発生しました。" & return & "エラー番号: " & errNum & return & "内容: " & errMsg buttons {"OK"} default button "OK"
end try
```

- Windowsの場合は、Pythonの `PyAutoGUI` ライブラリを使う方法などで同様の自動化が可能だと思います（未検証です）

■
