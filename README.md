# Human ReID　（歩行者OD作成のための複数カメラにおける人物再同定手法）

### 使用データ
- 広島大学内に設置された６台のカメラ（各カメラの画角は被らない）
- 事前に集めた被験者６人にGPSをつけ，一般歩行者に紛れて歩行させる
- アノテーションは，ビデオに映った被験者のみに固有ラベルを付し，その他歩行者には【未知】ラベルを付与する．よって計７つのラベルを付与する．

### 手順
1. 撮影動画をStrongSORT-YOLOに入力し人物画像を切り出す．また追跡結果より同一人物と思われる画像群であるトラックレットを得る（track.ipynb）
2. すべての画像をmediapipe-poseに入力し，骨格特徴量を得る．また，骨格特徴量を基に基準を満たしていない低品質なサンプルを訓練データから削除する(mediapipe.ipynb)（理由は山崎の論文参照）
3. すべての画像にデータオーギュメンテーションを適用する（  data_augumentation.ipynb）
4. すべての画像をOSNet(特徴量抽出部分のみ使用)に入力し，外観特徴量を得る（osnet.ipynb）
6. 外観特徴量を基に人物判別のためのDNNを距離学習する.（損失関数 triplet Loss）(learn_DNN.ipynb)
7. 距離学習モデルを用いて，OSNetから得た特徴量を変換する，（apply_model.ipynb）
8. 7で出力された特徴量を基に最適輸送を使用して，トラックレット単位でギャラリーデータとクエリデータのベクトル空間上の距離を計算する．結果を用いて各トラックレットの人物ラベルを決定する(OT.ipynb)



![スクリーンショット 2024-11-05 171226](https://github.com/user-attachments/assets/454ab62e-d377-4b28-9675-358d89c1ad67)

![スクリーンショット 2024-11-05 171700](https://github.com/user-attachments/assets/d821ad8a-8354-46a5-8196-f8cfa7d0b636)
