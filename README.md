# [NeurIPS-22] Label-Aware Global Consistency for Multi-Label Learning with Single Positive Labels

The implementation for the paper [Label-Aware Global Consistency for Multi-Label Learning with Single Positive Labels](http://www.xiemk.pro/publication/neurips22-lac.pdf) (NeurIPS 2022). 

See much more related works in [Awesome Weakly Supervised Multi-Label Learning!](https://github.com/milkxie/awesome-weakly-supervised-multi-label-learning) 

## Preparing Data 

See the `README.md` file in the `data` directory for instructions on downloading and preparing the datasets. (The detailed procedures follow [Multi-Label Learning from Single Positive Labels](https://github.com/elijahcole/single-positive-multi-label))

## Training Model

To train and evaluate a model, the next two steps are required:

1. For the first stage, we warm-up the model with the AN loss and the PLC regularization. Run:
```
python first_stage.py --dataset_name=coco --dataset_dir=./data \
--lambda_plc=1 --threshold=0.6 \
--batch_size=32
```

2. For the second stage, we train the model by adding the LAC regularization. Run:
```
python second_stage.py --dataset_name=coco --dataset_dir=./data \
--lambda_plc=1 --threshold=0.9 \
--lambda_lac=1 --temperature=0.5 --queue_size=512 \
--batch_size=32 --is_proj
```

## Hyper-Parameters
To obtain the results reported in the paper, please modify the following parameters:
1. `dataset_name`: The dataset to use, e.g. 'coco', 'voc', 'nus', 'cub'.
2. `dataset_dir`: The directory of **all datasets**. 
3. `batch_size`: The batch size of samples (images).
3. `lambda_plc`: The weight of PLC regularization item.
4. `lambda_lac`: The weight of LAC regularization item.
4. `threshold`: The threshold for pseudo positive labels.
4. `temperature`: The temperature for LAC regularization.
4. `queue_size`: The size of the Memory Queue.
4. `is_proj`: The switch of the projector which generates label-wise embeddings.
4. `is_data_parallel`: The switch of training with multi-GPUs.


## Misc

* The range of hyper-parameters can be found in the paper.
* There are four folders in this directory `dataset_dir` --- 'coco/', 'voc/', 'nus/', 'cub/'. Please make sure that the path of the dataset is correct before training.
* We performed all experiments on two GeForce RTX 3090 GPUs, so the `os.environ["CUDA_VISIBLE_DEVICES"] = "0, 1"`. The switch of training with multi-GPUs is `False` by default, and you can open it with `--is_data_parallel`.

## Reference
If you find the code useful in your research, please consider citing our paper:
```
@inproceedings{
	xie2022labelaware,
	title={Label-Aware Global Consistency for Multi-Label Learning with Single Positive Labels},
	author={Ming-Kun Xie and Jia-Hao Xiao and Sheng-Jun Huang},
	booktitle={Advances in Neural Information Processing Systems},
	year={2022}
}
```