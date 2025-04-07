# Detectron2を用いたレイアウト検出モデル学習用スクリプト

## 使用方法

### ディレクトリ構造

- tools/では、データフォーマットの変換やモデルのトレーニングに便利なスクリプトを提供している。
- scripts/には、与えられたデータセットを処理するためのコードを実行するための特定のコマンドがリストされている。 
- configs/には様々なディープラーニングモデルのコンフィギュレーションが含まれており、データセットごとに整理されている。

### モデルのトレーニング方法は？

- データセットとアノテーションを手に入れる -- よくわからない場合は、お気軽にお問い合わせください。 [this tutorial](https://github.com/Layout-Parser/layout-parser/tree/main/examples/Customizing%20Layout%20Models%20with%20Label%20Studio%20Annotation). 
- 設定ファイルとトレーニングスクリプトの複製と修正
    - たとえば、configs/prima/fast_rcnn_R_50_FPN_3x を configs/your-dataset-name/fast_rcnn_R_50_FPN_3x にコピーし、scripts/train_prima.sh に基づいて独自の scripts/train_<your-dataset-name>.sh を作成することができます。
    - You'll modify the `--dataset_name`, `--json_annotation_train`, `--image_path_train`, `--json_annotation_val`, `--image_path_val`, and `--config-file` args appropriately. 
- If you have a dataset with segmentation masks, you can try to train with the [`mask_rcnn model`](configs/prima/mask_rcnn_R_50_FPN_3x.yaml); otherwise you might want to start with the [`fast_rcnn model`](configs/prima/fast_rcnn_R_50_FPN_3x.yaml)
    - If you see error `AttributeError: Cannot find field 'gt_masks' in the given Instances!` during training, this means you should not use 

## 対応データセット

- Prima Layout Analysis Dataset [`scripts/train_prima.sh`](https://github.com/Layout-Parser/layout-model-training/blob/master/scripts/train_prima.sh)
    - You will need to download the dataset from the [official website](https://www.primaresearch.org/dataset/) and put it in the `data/prima` folder. 
    - 元のデータセットは[PAGEフォーマット](https://www.primaresearch.org/tools/PAGEViewer)で保存されているので、スクリプトは[`tools/convert_prima_to_coco.py`](https://github.com/Layout-Parser/layout-model-training/blob/master/tools/convert_prima_to_coco.py)を使ってCOCOフォーマットに変換する。
    - 最終的なデータセットのフォルダ構造は次のようになります。:
        ```bash
        data/
        └── prima/
            ├── Images/
            ├── XML/
            ├── License.txt
            └── annotations*.json
        ```

## 参考 

- **[cocosplit](https://github.com/akarazniewicz/cocosplit)**  ココのアノテーションを訓練セットとテストセットに分割するスクリプト。
- **[Detectron2](https://github.com/facebookresearch/detectron2)** Detectron2はFacebook AI Researchの次世代ソフトウェアシステムで、最先端の物体検出アルゴリズムを実装しています。
