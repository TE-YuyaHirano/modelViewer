# modelViewer

この `modelViewer` は、URL パラメータで **モデル読み込み**・**表示調整（露出/トーンマップ/HDR）**・**色チューニング（色グループ別の彩度/明るさ/色相）**・**デバッグ表示** などを切り替えできます。

---

## 起動と基本URL

ローカル例:

```txt
http://127.0.0.1:5500/index.html?model=asset/QR/QR.obj&mtl=asset/QR/QR.mtl&ui=off
```

Vercel 例:

```txt
https://model-viewer-lilac-nu.vercel.app/?model=asset/QR/QR.obj&mtl=asset/QR/QR.mtl&ui=off
```

---

## 必須パラメータ

| key | 例 | 説明 |
|---|---|---|
| `model` | `asset/QR/QR.obj` | OBJ のパス |
| `mtl` | `asset/QR/QR.mtl` | MTL のパス |

---

## UI / 演出

| key | 例 | 説明 |
|---|---|---|
| `ui` | `off` / `on` | UI の表示切替 |
| `intro` | `off` / `1.6` | イントロ演出（数値は秒想定） |

---

## 環境光 / トーン / 露出

| key | 例 | 説明 |
|---|---|---|
| `hdr` | `/asset/studio.hdr` | HDRI のパス |
| `exposure` | `1.0` | 露出 |
| `tonemap` | `neutral` / `aces` / `linear` | トーンマッピング |

---

## 背面ライト（裏側が暗い対策）

裏側から見たときに暗く沈む場合、背面方向からのライトを追加できます。

| key | 例 | 説明 |
|---|---|---|
| `backLight` | `0.28` | 背面ライト強度（0で無効） |

例:

```txt
...&backLight=0.35
```

---

## 色チューニング（ColorTune）

色チューニング機能を有効化すると、特定の色グループだけを **彩度/明るさ/色相** で調整できます。

| key | 例 | 説明 |
|---|---|---|
| `colorTune` | `on` / `off` | 色チューニングの有効/無効 |
| `sat` | `0.95` | 単色（全体）をまとめて調整するための全体彩度（任意） |

> `colorTune=off` の場合、以下のグループ別パラメータを入れても基本的に反映されません。

---

## 赤 / ピンク

| key | 例 | 説明 |
|---|---|---|
| `redSat` | `0.9` | 赤グループ彩度 |
| `pinkSat` | `0.4` | ピンクグループ彩度 |
| `pinkLight` | `1.04` | ピンクの明るさ（リフト） |
| `pinkHue` | `0.015` | ピンクの色相微調整 |

### マテリアル固定（誤判定対策）

自動判定が難しいモデルでは、**対象マテリアル名を URL で固定**できます。

| key | 例 | 説明 |
|---|---|---|
| `redMats` | `MB_21` | 赤として扱うマテリアル名（カンマ区切り） |
| `pinkMats` | `MB_353` | ピンクとして扱うマテリアル名（カンマ区切り） |

例:

```txt
...&pinkSat=0.4&redSat=0.9&redMats=MB_21&pinkMats=MB_353
```

---

## グレー / 茶 / 黄 / 青 / シアン / オレンジ（モデル別チューニング）

QR 系モデルで追加した「色グループ別」調整です。

### グレー
| key | 例 | 説明 |
|---|---|---|
| `grayLift` | `2` | グレー明るさ |
| `graySat` | `0.92` | グレー彩度 |

### 茶（床面など）
| key | 例 | 説明 |
|---|---|---|
| `brownLift` | `2` | 茶の明るさ |
| `brownSat` | `1.5` | 茶の彩度 |

### 黄（ミニフィグ顔など）
| key | 例 | 説明 |
|---|---|---|
| `yellowHue` | `0.02` | 黄の色相補正 |
| `yellowLift` | `1` | 黄の明るさ |
| `yellowSat` | `0.7` | 黄の彩度 |

### 青
| key | 例 | 説明 |
|---|---|---|
| `blueLift` | `1.5` | 青の明るさ |
| `blueSat` | `0.8` | 青の彩度 |

### シアン（水色）
> 実装済み（Blue/Cyan は動作確認済み）

- `cyanLift`（明るさ）
- `cyanSat`（彩度）

※URL 例は下記の「まとめ例」を参照

### オレンジ
| key | 例 | 説明 |
|---|---|---|
| `orangeLift` | `2` | オレンジの明るさ |
| `orangeSat` | `1.0` | オレンジの彩度（対応している場合） |

> オレンジが誤判定される場合も、`*Mats` 固定（例: `orangeMats=...`）方式で救済できます。

---

## デバッグ

| key | 例 | 説明 |
|---|---|---|
| `debugColor` | `on` / `off` | 色判定・補正のログ出力 |
| `debugMat` | `MB_21` | 特定マテリアルだけログを詳しく表示 |

---

## まとめURL例（QRモデル）

```txt
...?model=asset/QR/QR.obj&mtl=asset/QR/QR.mtl&ui=off&tonemap=neutral&exposure=1.0&colorTune=on
&grayLift=2&graySat=0.92
&brownLift=2&brownSat=1.5
&yellowHue=0.02&yellowLift=1&yellowSat=0.7
&blueLift=1.5&blueSat=0.8
&cyanLift=1.2&cyanSat=0.85
&orangeLift=2
&backLight=0.28
&debugColor=off
```

## 例
https://model-viewer-lilac-nu.vercel.app/?model=asset/QR/QR.obj&mtl=asset/QR/QR.mtl
https://model-viewer-lilac-nu.vercel.app/?model=asset/QA/QA.obj&mtl=asset/QA/QA.mtl
https://model-viewer-lilac-nu.vercel.app/?model=asset/QM/QM.obj&mtl=asset/QM/QM.mtl
https://model-viewer-lilac-nu.vercel.app/?model=asset/Q/Q.obj&mtl=asset/Q/Q.mtl
https://model-viewer-lilac-nu.vercel.app/?model=asset/GIFT/gift.obj&mtl=asset/GIFT/gift.mtl
---

## トラブルシュート

- **特定色が別の色として判定される**
  - `debugColor=on` で `mat`（マテリアル名）を確認し、`redMats=...` のように固定指定で救済してください。
- **パラメータを変えても見た目が変わらない**
  - `colorTune=on` になっているか、`mtl` が正しく読み込めているか確認してください。
