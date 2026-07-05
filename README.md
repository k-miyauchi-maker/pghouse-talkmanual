# pghouse-talkmanual — ピュアグロース 業務自動化・分析資産

ピュアグロース株式会社の「Claudeと一緒に作った道具・分析・ドキュメント」の集約リポジトリ。

## 中身の地図

| 場所 | 内容 | 状態 |
|---|---|---|
| [`ichijo_dashboard.html`](ichijo_dashboard.html) | 一条工務店 都道府県別ダッシュボード（棟数・拠点・PH・HTC・地図・拠点ランキング） | ✅ 完成。ブラウザで開くだけ |
| [`touki-tool/`](touki-tool/) | 謄本上げ自動化: 法務省地図データ→坪数レンジ抽出→Excel→所有者統合→DMリスト | ✅ ツール完成。実データ投入待ち |
| [`docs/01_業務棚卸し.md`](docs/01_業務棚卸し.md) | 実データに基づく業務インベントリと自動化優先度 | 2026-07版 |
| [`docs/02_自動化資産の整理.md`](docs/02_自動化資産の整理.md) | 散らばっていた資産の大掃除の記録・運用ルール | 2026-07版 |
| `.claude/skills/` | 繰り返し依頼をスキル化（下記） | 4本収録 |
| `CLAUDE.md` | Claudeセッション用の会社・リポジトリ文脈 | — |

## スキル（Claudeへの定型依頼）

| 一言で頼むなら | スキル |
|---|---|
| 「〇〇（競合他社）のダッシュボード作って」 | `competitor-dashboard` |
| 「浜松の150〜200坪のDMリスト作って」 | `touki-list` |
| 「〇〇地方版の住宅市場レポート作って」 | `market-report` |
| 「トークマニュアル作って／更新して」 | `talkmanual` |

## touki-tool クイックスタート

```bash
pip install openpyxl

# ① 候補地抽出（G空間情報センターの無料GeoJSONを使用）
python3 touki-tool/extract_parcels.py 浜松市中央区.geojson --min-tsubo 150 --max-tsubo 200 \
    --buffer 0.1 -o 浜松_150-200坪.xlsx --csv 一括請求用.csv

# ② 一括請求用.csv を所有者一括取得サービス（ホームズ等）に投入 → 所有者.csv を受領

# ③ 統合してDMリスト完成
python3 touki-tool/merge_owners.py 浜松_150-200坪.xlsx 所有者.csv -o 浜松_DMリスト.xlsx
```

詳細は [`touki-tool/README.md`](touki-tool/README.md)。
