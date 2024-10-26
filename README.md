# Nishika_2024_practice  
Nishika [Training] 中古マンション価格予測

---

## 📌 プロジェクトの概要（Introduction）  
このプロジェクトは、国内の中古マンションの価格を予測することを目的としています。  
使用したデータは、Nishikaによって土地総合情報システムから収集・加工されたものです。

- **目的変数**：取引価格（総額）の常用対数「`取引価格（総額）_log`」  
- **評価指標**：予測精度は**平均絶対誤差（MAE：Mean Absolute Error）**で評価します。

---

## 📊 データの概要（Dataset Description）

### 1. [学習データ（train.zip）](https://s3.ap-northeast-1.amazonaws.com/nishika.assets.private/competitions/13/data/train.zip?response-content-disposition=attachment%3B%20filename%3Dtrain.zip&AWSAccessKeyId=ASIA3NMWWMCVZQA6TRKO&Signature=o5NpTQJ%2BFiBZx6FLYfzzwwcK4qI%3D&x-amz-security-token=IQoJb3JpZ2luX2VjEI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaDmFwLW5vcnRoZWFzdC0xIkgwRgIhAJu%2BZcxbubpcyVFb8KNKatPo2FuguPOZcJ0%2BPSvQvOGLAiEA0DBD1N9CGvqmutf2t9MIUdrpsK3%2FJPuzwGITaV46v2oqiwQI9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAFGgw3ODQ2ODQ0NDE3NzEiDDC8ugl5NWu%2BqfxmmCrfAz0iAuSK5tMugfJgD3ceCC08ekYHx1jj8M8NHqxGA9VnvIyiOUEoI441aJd9by%2FQOvePSFw7UalBkKQMta%2BgOd0ljfVO1tKKxqBKIIKDWVd9KB2Tl0NelKIk0lH1vFHDiZdKdw5vhwQncaNczKQxt%2FM3rMZVIh2LrZX0RJ76udF15Jo%2Fak6aMgPcRGRMhL0vbj%2BPd9uEzli4P0OG%2FX8FX%2FYf%2FkPI%2BgWaEihcuLEvcXvIGMEBetoGdYa7YerJX5Aa4ih3Uut%2FczDyv1mNeEKxLwZEvqCqa1Ribck288dyRzYThu45jJpc%2BVnLGdspSjibD%2FcnIKvUyotN1JEmdD%2FmZ%2BJaS3dBktDXalXnlNR0x3V8aLWQdE3X6r4PiwgzKLXUSQ9f3WAVhKc1tLB14SNi3MytQ0wxfWYavUG9Y97OUU%2Bo%2Fmb9gOJ6r7dcuHdIAlbGHezuMNcaZZomB2ff5xpvPDQBJOfKzZ7fCHW%2BJmG%2F8s0CUbJx7KkBmzT1QjvFnZUxSbhPcwB5v%2Bt6AelOr6CzrJkSPHoO%2BHLsHJ7NVIcLy6ROz9nerOKeBeu0tj4Tb2OthwDU1yPKkUH4a5tCPXxa%2FlgmkSeFHsWGXiu7Rw91MBgeISCIHwJniE8kC94WBygGMK2i8LgGOqQBvvcsvMZLVjW0iJ6mjoh0BQMzSxa2SObt2ROCLRPqnD%2FT8Btsl10aJ46y8cLNU9yf7G%2FctBlDsMIfDZilc7r2RgKRcEQ8h5Fj6qZv3HfOk%2FaeOB3ngzDAEhes8HYo0gGI0dsMiE9fuS%2FvNOf%2Bk76WoEAKfePLkeBhv1nA06UeYPWvHq%2B1yJPJb%2FD2PB%2BCf9BXuR7vFwfkmLePJFs1smCgtrUDNG8%3D&Expires=1729908716)
- **内容**：2019年以前に取引された中古マンションの価格データ  
- **使用目的**：モデルの学習

### 2. テストデータ（test.csv）
- **内容**：2020年第一四半期および第二四半期に取引された中古マンションのデータ  
- **使用目的**：価格予測モデルの評価および提出用データの生成

---

## 使用したライブラリ
glob
- ディレクトリ内のファイル名を取得するために使用。複数のCSVファイルを一括で読み込む際に便利。

pandas
- データの読み込み・前処理・分析に使用。
- データフレーム形式での操作を提供。

numpy
- 数値計算をサポートするライブラリ。
- 欠損値処理や配列操作に利用。

matplotlib.pyplot
- グラフや図の描画に使用。特徴量の重要度やデータの分布を可視化。

seaborn
- データの分布や相関関係の可視化をサポート。
- matplotlibの拡張ライブラリとして使用。

os
- ファイルのパス操作やディレクトリの作成に使用。

lightgbm
- 高速なブースティングモデルで、特徴量の重要度が高いアルゴリズム。
- 回帰モデルの構築と学習に使用。

catboost
- カテゴリカル変数の処理に強いブースティングモデル。
- モデルの精度向上と予測に使用。

---

## **作業工程（Workflow）**

#### 1️⃣ データの読み込みと前処理  
- **`glob`**を使って、複数のCSVファイルを読み込み、**`pandas`**でデータフレームに結合。  
- **不要な列の削除**（例：`市区町村名`などの削除）。  

---

#### 2️⃣ データの分割  
- **学習用と検証用**にデータを分割（例：2019年以前と2020年以降）。  
- **train**と**validation**データの作成。

---

#### 3️⃣ モデルの構築とトレーニング  
- **LightGBM**と**CatBoost**のモデルを構築し、それぞれで学習を実施。  
- **早期終了（early stopping）**や**ハイパーパラメータの調整**を使い、精度を向上。

---

#### 4️⃣ 予測とモデル評価  
- **MAE（Mean Absolute Error）**で予測の精度を評価。  
- **検証用データ**に対する予測結果を出力し、モデルの性能を確認。

---

### **モデルが重視する特徴量の説明（Feature Importance）**

#### **[LightGBM] モデルの精度**
- **MAE（Mean Absolute Error）**: `0.0803`  
  モデルは比較的高い精度で価格を予測できています。

---

#### **特徴量の重要度（Feature Importance）**

以下は、LightGBMモデルが最も重視した**上位5つの特徴量**です：

| 特徴量           | 重要度（Feature Importance） |
|------------------|-----------------------------|
| 建築年            | 139,944.94                  |
| 面積（㎡）         | 73,863.11                   |
| 都道府県名         | 58,091.66                   |
| 最寄駅：名称       | 39,843.35                   |
| 地区名            | 19,503.95                   |

---

#### **特徴量の重要度の可視化**

以下のグラフは、LightGBMモデルが重視した特徴量を**重要度の高い順に視覚化**したものです：

![特徴量の重要度](./light_gbm.png)


---

### **解説**

- **建築年**：価格の推定において、物件の築年数は最も重要な特徴量となりました。築年数が新しい物件ほど、高い価格が予測される傾向が見られました。
- **面積（㎡）**：物件の広さも価格に大きく影響します。広い物件は高価格帯である可能性が高いため、モデルにとって重要です。
- **都道府県名**：地域ごとの不動産市場の違いも価格に反映されています。地域によって同じ間取りや広さでも価格に違いが生じます。
- **最寄駅：名称**：駅に近い物件ほど価格が高い傾向があり、最寄駅も重要な特徴量となりました。
- **地区名**：細かな地域ごとの違いもモデルに影響を与えます。

---

## **CatBoost モデルの精度と特徴量の重要度**

---

### [CatBoost] モデルの精度**
- **MAE（Mean Absolute Error）**: `0.07895958437579434`  
  → 今回のトレーニングではCatBoostモデルがLightGBMモデルよりも、高い精度**で価格を予測しました。

---

### 特徴量の重要度（Feature Importance）**

以下は、CatBoostモデルが最も重視した**上位5つの特徴量**です：

| **特徴量**         | **重要度（Feature Importance）** |
|--------------------|---------------------------------|
| 面積（㎡）         | 30.207216                      |
| 建築年             | 19.689864                      |
| 最寄駅：名称       | 12.856488                      |
| 都道府県名         | 10.503524                      |
| 市区町村コード     | 7.203636                       |

---

### 特徴量の重要度の可視化**

以下のグラフは、**CatBoostモデルが重視した特徴量の重要度**を、重要度の高い順に視覚化したものです：![CatBoost 特徴量の重要度](./catboost.png)  

---

### **CatBoostモデルの学習曲線の可視化（Learning Curve）**

以下は、**CatBoostモデルの学習の進捗と精度の向上**を示す学習曲線（Learning Curve）です：

![CatBoost 学習曲線](./learning_curve.png)  

---

#### **グラフの解説**

1. **急激な誤差の減少（初期学習フェーズ）**  
   - グラフの最初の部分では、**0〜250回程度のイテレーションで急激に誤差が減少**しています。  
   - これは、モデルが早い段階でデータのパターンを学習し、**大まかな予測精度が向上**したことを示しています。

2. **誤差の安定化（後半の学習フェーズ）**  
   - イテレーションが500を超えるあたりから、**トレーニングセットと検証セットのMAEがほぼ平行に近づいています**。  
   - これは、モデルが**新たな改善が見込めない状態**に到達したことを意味します。

3. **トレーニングと検証の一致**  
   - 学習曲線では、**トレーニングセットと検証セットのMAEが非常に近い**状態で推移しています。  
   - これにより、**過学習（Overfitting）が発生していない**ことが示唆されます。

4. **早期終了（Early Stopping）の活用**  
   - モデルは、**学習の進行に伴って改善が見られなくなった時点で自動的に停止**しました。  
   - これにより、**不要なイテレーションを回避**し、トレーニング時間を節約しています。

---

### **このグラフが示すインサイト**

- **トレーニングと検証のMAEがほぼ一致**しているため、モデルは適切なバランスで学習しています。  
- **平行な曲線**が示すのは、**データの本質的なパターンを十分に捉えた**状態であることを意味します。
- この状態は、モデルが**新しいデータに対しても良い予測性能を発揮する可能性**が高いことを示唆します。

---

### **ステークホルダー向けの解釈**

- このグラフにより、**モデルが効果的に学習し、過学習の兆候がない**ことが確認できました。  
- さらに、モデルの性能が**トレーニングデータと検証データの両方で一貫して高い**ことから、**本番環境でも安定した予測が期待できる**と伝えることができます。

---

### 改善点や次のステップ**
- **スタッキングモデル**などの手法を導入し、より精度の高いモデルを作成します。
- 他のアルゴリズムとの比較やハイパーパラメータのさらなる調整も検討します。

---

### 著者情報（Author）**
- **名前**：  capri7
- **メール**：kazuehayakawa@gmail.com  

---

