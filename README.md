# Tarkov Key Lens - Web公開用

このフォルダーはHTTPSの静的ホスティング用です。`index.html`を入口に、`data.js`と`vendor`フォルダーをまとめてアップロードしてください。

## Cloudflare Pages（無料枠）の公開手順

※ Cloudflare画面で `workers.dev` の「Worker URL」や `Manually deployed` と表示されている場合は、PagesではなくWorkersとして公開されています。その場合は下の「Cloudflareへの自動反映」を使って、現在のWorkerにGitHubを接続してください。

1. Cloudflareにログインする。
2. Workers & Pages → Create application → Pages → Direct Uploadを選ぶ。
3. プロジェクト名を入力し、この`TarkovKeyLens-Web`フォルダーをアップロードしてDeployする。
4. 発行された`*.pages.dev`のURLを友人に共有する。

Netlifyを使う場合は、Projects → Add new project → Deploy manuallyから同じフォルダーをアップロードできます。

このWeb公開用フォルダーではHTTPSで不要な`file://`向けOCRフォールバックを除外しています。ローカルでダブルクリックして使う場合は、元の`TarkovKeyLens`フォルダーを使ってください。

## 改善版の認識処理

- 小さい部屋番号ラベルをDBの形式と照合し、`WZ21B.San`のようなOCR誤読から`W218 San`を優先します。
- 端末が対応している場合はOCRワーカーを2本使い、セル名と残り使用回数を並列処理します。
- 不確かなセルだけ追加の画像処理を行い、全セルを何度も再OCRしない構成です。
- 「OCR補正」と表示された項目は、表示されたOCR文字列を残したまま、DBの正式な短縮名で補正照合した項目です。
- `Key tool` は物理キーではないため、鍵収納ケースとして「対象外」に分類します。
- `Store` や `Safe` のようにゲーム内表示名が複数の鍵で共通する場合は、誤った鍵に決め打ちせず「候補を確認」として候補を表示します。
- アイコン照合はOCRの補助に限定し、画像だけで誤判定が増える場合は採用しない安全側の判定です。

## Cloudflareへの自動反映

現在のプロジェクトがダッシュボードの手動アップロード（`Manually deployed`）の場合、ローカルの変更だけでは自動反映されません。

自動反映するには、GitHubにこのフォルダーを登録し、Cloudflareの対象Workerで`Settings → Builds → Connect`からGitHubを一度だけ接続します。以後、設定した本番ブランチへのpushで自動ビルド・デプロイされ、既存の`workers.dev` URLが更新されます。

