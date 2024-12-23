# 実行場所
- https://toshiko1045.github.io/Cactus3-Coloring/

# 制作目的
- 過去の卒業論文の提案アルゴリズムの, 実装を通して理解すること

# 主にできること
- 任意のカクタスグラフの作成
- 任意のカクタスグラフの3-彩色

# 操作説明
## 画面左
- 何もない所をダブルクリック
  - ノード生成
- ノード2つをクリック
  - エッジ生成
- ノードをダブルクリック
  - ノード削除
- エッジ両端のノード2つをクリック
  - エッジ削除
- ノードをクリック
  - ノード選択
- ノードをクリックしてドラッグ
  - ノード移動
- 何もない所をクリックしてドラッグ
  - グラフ移動
- スクロール
  - グラフ拡大縮小
## 画面右上
- connected graph numb
  - カクタスグラフ自動生成時のパラメータ入力欄
- connected graph size
  - カクタスグラフ自動生成時のパラメータ入力欄
- random x range
  - カクタスグラフ自動生成時のパラメータ入力欄
- auto make cactus graph
  - カクタスグラフを自動生成するボタン
- clear
  - グラフを削除するボタン
- change color
  - 選択されたノードが持つ色を変更するボタン
- x : value :
  - 選択されたノードが持つ変数xに設定したい値入力欄
- change x
  - 選択されたノードが持つ変数xをx : value :に設定された値に変更するボタン
- check cactus graph
  - グラフがカクタスグラフか否か判定するボタン
- auto coloring graph
  - 彩色アルゴリズムのステップ実行を一定時間毎に行うボタン
- step coloring graph
  - 彩色アルゴリズムのステップ実行を行うボタン
- skip coloring
  - 彩色結果のみを表示するボタン
- show directed graph
  - 変数xの小さい方から大きい方への有向辺を表示するボタン
- show status
  - idと変数xを表示するボタン
- make balanced graph
  - 力学モデルに従ってグラフの描画座標を調整するボタン
- save graph name
  - グラフを保存する際の名前の入力欄
- save
  - グラフを保存するボタン
- load, add, delete graph name :
  - データベースから読み込む, 追加する, 削除するグラフを選択する欄
- load
  - データベースからグラフを読み込む
- add
  - データベースからグラフを追加する
- delete
  - データベースからグラフを削除する
- change node size :
  - ノードの大きさを変更するバー
- change gravity size :
  - ノードの密集度合を変更するバー
- change coulomb size :
  - ノード間の反発力を変更するバー
- undo
  - 操作を1つ戻すボタン
- redo
  - 操作を1つ進めるボタン

# 表示説明
## 画面左
- 彩色アルゴリズム実行時
  - 彩色アルゴリズム実行中のノードは白線で囲まれる
## 画面右下
- 各操作時
  - 操作内容が表示される

# 主な実装アルゴリズムの説明
## 任意のカクタスグラフを3彩色する自己安定アルゴリズム(参照した卒業論文における提案アルゴリズム)
- 前提
  - (ノード内変数x, ノードのid)の辞書順で小さい方から大きい方へ有効辺が存在
  - 色集合C={i,j,k} (i<j<k) であり, c∈C
- 各ノードが実行するアルゴリズム
```
if(出次数>2) x ← 隣接ノードの中でのxの最大値+1
else if(出辺先に自身と同じ色がある) c ← 出辺先に無い色の中で最小の色
```
## カクタスグラフの自動生成
- 入力 : 正整数A,B
- 出力 : カクタスグラフG
- 計算方法 :
  1. A,Bを入力として受け取り, ノード数A以下の連結な木, またはノード数A以下の連結な円をグラフGとする
  2. ノード数A以下の連結な木, またはノード数A以下の連結な円を, Gに含まれる点をちょうど1点含むように作る
  3. 2.をB回行ったら, Gを出力して終了
## Dデーモンの再現
- 入力 : ノード配列 N
- 出力 : Nに含まれる1個以上の任意のノードからなる配列 N'
- 計算方法 :
  1. Nを入力として受け取り, 配列 I = [0,1,...,N.length] を作成
  2. N'を[]で初期化
  3. 1以上N.length以下の任意の整数をrとする
  4. Iから任意の要素iを取り出し, Iから削除
  5. N[i]をN'に追加
  6. 4.,5.をr回行ったらN'を出力して終了
## 分散アルゴリズムの再現
- 入力 : グラフG
- 出力 : グラフG'
- 計算方法 :
  1. 入力して受け取ったGをグラフGa, Gbとして複製
  2. 1ステップを実行
    - Gbを参照してGaを更新
  3. GaをGbに上書き
  4. 2.,3.を, Gaが正当な状況になるまで繰り返したら, GaをG'として出力して終了
## カクタスグラフか否かの判定
- 入力 : グラフG
- 出力 : 真偽値t
- 計算方法 :
  1. グラフGを入力として受け取る
  2. 選択済みでない任意の関節点aを選択
  3. aを削除することで生まれる任意の連結成分cとaを接続していたエッジの本数dを求める
    - d>=3の場合 : t=false としてtを出力して終了
    - d==2の場合 :
    - aと隣接していたc上の2点がc上の円の一部である場合 : t=false としてtを出力して終了
  4. 全ての関節点を選択し終えるまで2.,3.を繰り返したら, t=true としてtを出力して終了

# 参照した卒業論文
- カクタスグラフを3彩色する自己安定アルゴリズムに関する研究 (2023年度 卒業論文) (著者名省略) 
