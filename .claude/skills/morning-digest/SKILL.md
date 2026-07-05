---
name: morning-digest
description: 反響・セミナー申込・採用の直近24時間分をGmailから集計し朝ダイジェストを作成する。毎朝7時(JST)のルーチンから自動実行される。「朝ダイジェスト作って」「昨日の反響まとめて」で使用。
---

# 朝ダイジェスト作成スキル

## Gmail検索クエリ（Gmail MCPの search_threads で3本並列）

1. **反響（PG HOUSE・FC各社）**: `newer_than:1d (from:contact@auka.jp OR from:no-reply@pg-house.jp OR from:no-reply@event.pg-house.jp)`
2. **セミナー申込・HP問い合わせ**: `newer_than:1d from:info@pure-growth.co.jp`
3. **採用・その他**: `newer_than:1d (from:no-reply-crs@bizreach.co.jp OR subject:採用エントリー OR from:no-reply@pg-cloud.cloud)`

## 集計ルール

- **重複排除**: ALL GRIT（auka.jp）は同一の出来事で同文の通知を複数回送ってくる。
  顧客URL（line-saas.auka.jp/builder/customers/の番号）または氏名+件名で1件にまとめる
- **カテゴリ分け**: ①PG HOUSE反響（資料請求/イベント予約/問い合わせ） ②ウラ側ハウスLINE
  ③FC各社LINE（紀の国住宅等、アカウント名で判別） ④セミナー申込 ⑤採用（ビズリーチ/HPエントリー）
  ⑥その他（PGクラウド等）
- スニペットから **氏名・企業名・種別・きっかけ** を抽出。参加人数があれば書く
- 営業メール・明らかなスパム（英語の売り込み等）は「その他」で1行に落とす
- 判断コメントは付けてよい（例: 紹介経由・複数筆の申込・過去接点あり等の注目フラグ）

## 出力フォーマット

```
# 朝ダイジェスト YYYY-MM-DD(曜)
昨日24hの反響: 反響◯件 / セミナー申込◯件(計◯名) / 採用◯件

## 🏠 PG HOUSE・FC反響 (◯件)
- [資料請求] 氏名（住所地、建築予定地）
- [LINE友だち追加/ウラ側ハウス] 氏名
...
## 🎓 セミナー申込 (◯件)
- 氏名（企業名・役職）◯名 — セミナー名（きっかけ）
## 👤 採用 (◯件)
- [ビズリーチ応募] ◯件 / [HPエントリー] 氏名（学校・専攻）
## 📎 その他
```

## 配信

1. ダイジェスト全文をセッションの返信として出力
2. `PushNotification` で件数サマリーのみ送信（200字以内・**個人名や電話番号は入れない**）。
   例: `朝ダイジェスト: 反響8件(PG HOUSE 2/LINE 6)・セミナー申込2件16名・採用応募4件。詳細はセッションで`

## ルーチン設定（記録）

- Claude Code Remoteのトリガー（cron `0 22 * * *` UTC = 毎朝7:00 JST）でこのセッションに
  「朝ダイジェストを作成して（morning-digestスキル使用）」が届く設定（2026-07-05設定）
- 停止・変更は「朝ダイジェストのルーチン止めて/8時にして」とClaudeに言えばよい
