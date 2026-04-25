# RFind-explorer 使い方ガイド

**v0.2 draft — 2026-04-23 更新**

このドキュメントは [RFind-explorer](https://tmurano.github.io/RFind-explorer/) の各タブ・コントロールの使い方をまとめたものです。初見の人が 5 分で主要機能を触れるようにすることを目標にしています。

> スクリーンショットの placeholders (`![...](screenshots/...png)`) は埋込予定。現状テキストのみ。

---

## 0. 前提知識 — RFind-sc スコアとは

各セルに対して、あらかじめ定義した「軸 (panel)」ごとに **Running Fisher** を走らせて出した **1 本のスコア値**です。例えば `Aging` 軸では +50 なら「aging 方向に強い」、-30 なら「逆方向 (= 若い側)」の意味。10 panel あれば 1 セル = 10 次元ベクトルになります。

Explorer はこの **セル × panel の高次元スコア行列**を、散布図・相関・LDA などで多角的に俯瞰する UI です。

### AUCell / UCell との本質的な違い

UMAP color overlay など**見た目は AUCell と類似**しますが、背後のデータは根本的に異なります:

| 軸 | AUCell | UCell | **RFind-sc** |
|---|---|---|---|
| Gene list の型 | UP-only gene set | UP-only gene set | **Case vs Ctrl DEG (UP + DOWN)** |
| Score の符号 | 非負 (0〜0.3) | 非負 (0〜1) | **符号付き ±300** |
| Case vs Ctrl 情報 | 無し | 無し | **Panel に内在** |
| Downstream 解析 | UMAP overlay 中心 | 同左 | **Multi-axis LDA / D² pair ranking** |

→ Explorer の **★ Pair Analysis / ★ Full LDA** は符号付きスコア前提の解析で、AUCell/UCell の非負スコアでは構造的に不可能。これが差別化点。

---

## 1. グローバルコントロール (画面上部に常時表示)

![全体レイアウト](screenshots/01_global.png)

### 1-1. Group by セレクタ
サンプルを何でグループ分けするかを選ぶ。Hammond データなら `group` (E14.5 / P100_brain / P540 etc.) か `UMAP cluster` (0-11)。
→ 散布図の色、Pair Analysis の群指定などに連動。

### 1-2. Axis on/off チップ (★★★ 重要機能)
画面上部の `Axes:` 横に並ぶ小さなチップ。**クリックで各 panel を ON/OFF**。最低 2 本は ON 必須。

**どこに効く**:
- Scatter Matrix
- Correlation heatmap
- Pair Analysis の D² ランキング (候補ペア集合)
- Full LDA の入力軸

**どこに効かない**:
- 2D / 3D scatter (X/Y/Z は個別セレクタで選ぶため)
- UMAP (Panel セレクタで単独指定)

**典型的な使い方**: 「Histone 3 軸だけで群分離がどう見えるか？」→ H3K27ac/H3K4me2/H3K4me1 以外を OFF にして Pair Analysis / Full LDA タブを開く。

### 1-3. クラスターチップ
Axis チップの下に並ぶ群名チップ。**クリックで個別表示/非表示**。

→ 全タブ共通のサンプルフィルタ。特定群だけ見たいとき (e.g. 「P540 と P100_LPC だけで相関を見たい」)。

### 1-4. CSV 差し替え
自前の `rfind_scores.csv` をアップロードするボタン。

- フォーマット: `cell, group, seurat_cluster, umap_x, umap_y, <panel1>, <panel2>, ...`
- `umap_x/umap_y` は省略可 (UMAP タブが無効化されるだけ)
- `_groupings` / `_umap` の埋込みは自動生成される

---

## 2. タブ別ガイド

### 2-1. 2D Scatter

![2D Scatter](screenshots/02_2d_scatter.png)

**目的**: 任意 2 軸の散布図をプロット。最も基礎の可視化。

**コントロール**:
- `X:` `Y:` — 横軸・縦軸の panel
- `色:` — どの metadata 列で色分けするか (group / UMAP cluster)

**読み方**: 点の広がり・クラスター分離が一目で見える。Pair Analysis で「D² 最大ペア」を見つけたら、このタブで再プロットすると分離が直感的にわかる。

---

### 2-2. 3D Scatter

![3D Scatter](screenshots/03_3d_scatter.png)

**目的**: 2D では見えない 3 軸目の情報を加えて回転しながら見る。

**操作**:
- **ドラッグ** → 回転
- **スクロール** → ズーム
- **ダブルクリック** → 視点リセット

**使いどころ**: 2 本で分離しきれないとき、「もう 1 軸足したら分離するのでは？」を探索する。ただし論文 Fig には不向き (静止画で伝わりにくい)。

---

### 2-3. Scatter Matrix

![Scatter Matrix](screenshots/04_scatter_matrix.png)

**目的**: **全 panel ペアの散布図マトリクス**。どの軸の組み合わせで分離が出るか俯瞰する。

**コントロール**: 無し (Axis チップで表示軸を絞る)

**読み方**:
- 対角 = 自分自身 (普通は空 or ヒストグラム)
- 非対角 = 2 軸散布図のサムネール
- パッと見で「この組は分離してる」「この組は相関強くて縮退してる」がわかる

**Tips**: 10 panel だと 45 pair、全部表示すると重い。Axis チップで 4-5 軸に絞ると快適。

---

### 2-4. Correlation

![Correlation](screenshots/05_correlation.png)

**目的**: **全軸間の Pearson 相関行列**を heatmap で表示。panel 同士の冗長性/独立性を見る。

**コントロール**:
- `集計:` 全サンプル (群無視) / 群ごと (cluster 別に行列)

**読み方**:
- `|r| > 0.7` → 強相関 → **2 軸ある意味が薄い** (片方で代用可)
- `|r| ≈ 0` → 独立 → 追加情報あり
- `r < -0.5` → 逆向き → 「発現 UP 軸 vs 抑制 mark 軸」のような対立関係

**典型的発見**: Hammond データでは Aging-LPC r=0.87 → 共通の Ccl4+ 集団由来。H3K27ac と Aging は r=-0.67 → microglia homeostatic mark が aged で失われる。

---

### 2-5. ★ Pair Analysis (主要機能)

![Pair Analysis](screenshots/06_pair_analysis.png)

**目的**: **2 群 (A vs B) を最もよく判別する軸ペアを 45 通りから自動ランキング**。本 Explorer の中核機能。

**コントロール**:
- `Group A:` / `Group B:` — 比較する 2 群 (クラスタチップと別選択)
- `📥 ペアランキング CSV` — 結果を CSV で書き出し

**読み方**:
- 左のテーブル: 全 pair を D² (Mahalanobis 距離²) 降順で表示
- 右の散布図: **上位ペアがクリックで切替**、Group A (緑) vs Group B (赤) の分離が視覚化

**典型ユース**: 「P100_saline vs P100_LPC で最も discriminative な軸ペアは？」→ LPC と KS_DAM など disease response 系が上位に来るはず。

**★の理由**: RFind-sc の売り = 多軸空間での群分離構造の定量的探索。Pair Analysis がその主役。

---

### 2-6. ★ Full LDA

![Full LDA](screenshots/07_full_lda.png)

**目的**: **全軸の最適線形結合 (LDA)** を使って 2 群を判別する。Pair Analysis が "best 2"、Full LDA が "全軸総動員" 版。

**コントロール**:
- `Group A:` / `Group B:`

**表示**:
1. **LDA 1D 分布** (上): 2 群の LDA スコア分布 (density plot)。重なりが少ないほど判別良い
2. **Loadings** (下): 各軸がどれだけ分離に寄与したか (単位ベクトルの棒グラフ)

**読み方**:
- 2 群 LDA の**判別情報は本質的に 1 軸**なので、1D 分布で十分
- Loading の大きい軸 = その群ペアを規定する軸。Loadings が sparse (少数軸に集中) なほど解釈しやすい

**Pair Analysis との使い分け**:
- Pair = 可視化しやすさ (2D 散布図に乗る)
- Full = 最高判別性能 (全軸情報使う)

---

### 2-7. UMAP (E-11)

![UMAP](screenshots/08_umap.png)

**目的**: **UMAP 座標上に panel スコアを色 overlay**。空間分布で panel の意味を確認する。AUCell の主力可視化と**見た目は同じ**だが、符号付き bidirectional スコアなので「**負側 (青)**」領域にも診断情報が載る点が決定的に異なる。

**コントロール**:
- `Panel:` — 表示する panel を 1 つ選択

**読み方**:
- 0 中心の diverging palette (青=負、赤=正)
- クラスタ chip の絞り込み適用
- UMAP の位置情報 + panel スコアで「この cluster は LDAM 軸に強い」等を直感的に把握

**AUCell との対比例**: Hammond Aging panel を overlay すると、fetal/young cell 側が**深い青 (-150 相当)** で、aged cells は薄赤。AUCell だと fetal/young 側は単に「低い aging signature」として 0 付近にまとまるだけで diagnostic power が下がる。

**前提**: `rfind_scores.csv` に `umap_x/umap_y` 列が必要 (Chunk3.R で埋込済)

---

## 3. よくある使い方パターン

### Pattern A: 新しいデータを取り込んで一巡する
1. `rfind_scores.csv` を自前データで上書き → CSV 差し替えボタン
2. Scatter Matrix で全体俯瞰
3. Correlation で軸の冗長性確認 (r>0.7 の pair は片方削る検討)
4. Pair Analysis で興味ある群 vs 対照群を指定、top pair を確認
5. 2D Scatter で top pair を大きく表示して図表化

### Pattern B: Panel 設計の sanity check
1. UMAP で新 panel の score 分布が生物学的に妥当な cluster に偏っているか確認
2. Correlation で既存 panel との相関が中程度 (|r| 0.3-0.6) なら有用
3. 群ごとの violin plot (Chunk3 出力の PNG) と併せて判断

### Pattern C: 論文 Fig 候補作り
1. Pair Analysis で 2 群の best pair 特定
2. 2D Scatter で高解像度スクリーンショット (Plotly 右上のカメラアイコン)
3. Full LDA の loading plot で「どの軸が効いたか」を補助図に

---

## 4. 既知の制限 / 落とし穴

- **データ埋込型**: HTML ファイル自体にデータが書き込まれているため、ファイルサイズは 1-5 MB レベル。n=10 万セル等の巨大データはブラウザが重い可能性あり
- **Axis チップは最低 2 本**: それ以下にしようとすると自動で 1 本戻る
- **UMAP は CSV に umap_x/umap_y が無いと動かない**: Chunk3.R 経由で埋込むか、自前で列追加
- **旧ブラウザ非対応**: regex の `s` flag を使っているので Safari 古いバージョンだと壊れる
- **3D Scatter の群分離可視化は主観的**: 論文 Fig には 2D Scatter 推奨

---

## 5. キーボードショートカット

現状なし。要望あれば issue で。

---

## 6. Data model (埋込 RAW の中身)

HTML 内の `let RAW = {...}` JSON は以下の構造:

```javascript
{
  "cluster": [...],          // primary grouping (default: "group")
  "Aging": [...],            // panel 1 のスコア (cell 数分、符号付き)
  "LPC": [...],              // panel 2
  "...": "...",
  "_groupings": {
    "group": [...],
    "UMAP cluster": [...]
  },
  "_primaryGroup": "group",
  "_umap": {                 // E-11 用
    "x": [...],
    "y": [...]
  },
  "_meta": {
    "n_cells": 5998,
    "n_panels": 10,
    "has_umap": true,
    "updated_at": "2026-04-23T...",
    "panels": ["Aging", "LPC", ...]
  }
}
```

自前データ埋込スクリプト: [update_explorer_embed.py](https://github.com/tmurano/RFind-explorer) (RFind-sc 本体リポジトリに同梱予定)

---

## 変更履歴

- **2026-04-23 v0.2**: tSNE → UMAP 切替、AUCell 差別化ポイント追記、Sample scores CSV ボタン削除、cluster 呼称統一 (Seurat cluster → UMAP cluster)
- **2026-04-23 v0**: Prototype 作成 (10 panel + E-10 + E-11 対応)
