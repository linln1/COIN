**# COIN**



COIN is a graph contrastive learning framework with orthogonal continuous augment and Information Balance.



## The statistics of datasets

| 数据集    | 节点数 | 边数    | 特征维度 | 类别数 | 异质度 | 稀疏度  |
| --------- | ------ | ------- | -------- | ------ | ------ | ------- |
| Cora      | 2,708  | 5,429   | 1,433    | 7      | 0.19   | 0.00074 |
| CiteSeer  | 3,327  | 4,732   | 3,703    | 6      | 0.26   | 0.00042 |
| PubMed    | 19,717 | 44,338  | 500      | 3      | 0.20   | 0.00011 |
| Photo     | 7,650  | 119,081 | 745      | 8      | -      | 0.00203 |
| Computers | 13,752 | 245,861 | 767      | 10     | -      | 0.00130 |
| CS        | 18,333 | 81,894  | 6,805    | 15     | -      | 0.00024 |
| Phy       | 34,493 | 247,962 | 247,962  | 5      | -      | 0.00020 |
| Chameleon | 2,277  | 36,101  | 2,325    | 5      | 0.77   | 0.00696 |
| Squirrel  | 5,201  | 217,073 | 2,089    | 5      | 0.78   | 0.00802 |

<!--**## Supplymentary results**

1. Time cost  on RTX A6000 

| Stage             | Cora | CiteSeer | PubMed | Photo | Computers | CS    | Phy   |
| ----------------- | ---- | -------- | ------ | ----- | --------- | ----- | ----- |
| preprocessing (s) | 0.52 | 0.99     | 71.53  | 7.10  | 31.44     | 19.56 | 15.24 |
| training (s)      | 0.56 | 0.22     | 3.30   | 1.49  | 1.34      | 4.28  | 16.42 |

2. To be continued...
-->


**## Requirement**

Code is tested in ***\*Python 3.8.10\****. You can prepare the environment for running COIN follow the command:



\```bash

pip install -r requirements.txt

\```



**## Quick Strat**



**### Make directories to records the outputs.**



Before you runing COIN.py on dataset ```$ds``` , you should make directories for keeping output by using the following command:

\```bash

mkdir -p ./outputs/${ds}

\```



**### Prepare data.**

This step is to prepare the data and reuse the results of the decomposition of the topological matrix A and the node feature matrix X. The decomposition result of the ```$ds``` dataset are stored in ```./data/npy``` by using the following command:



\```bash

nohup python decompose_A_X.py --dataset $ds

\```



**### How to run COIN with hyperparameters search?**





Next, you can search hyperparameters of COIN on the GPU ```$gpu_id``` with or withoud mask mechanism by using the following commands.



\```bash

CUDA_VISIBLE_DEVICES=$gpu_id, nohup python COIN.py --dataset $ds --use_mask_ratio --botune &> ./outputs/${ds}/ULA_mask.out&

CUDA_VISIBLE_DEVICES=$gpu_id, nohup python COIN.py --dataset $ds --botune &> ./outputs/${ds}/ULA_nomask.out&

\```



**### How to run COIN with cetain hyperparameters?**



For example, if you want to run the COIN on Cora dataset, you can use the following command:



\```bash

CUDA_VISIBLE_DEVICES=$gpu_id, nohup python COIN.py --dataset Cora --lr1 5e-4 --lr2 5e-4 --wd 1e-5 --hid_dim 256 --proj_dim 256\ 

​                         --use_mask_ratio --mask_ratio 0.15 --alpha 0.55 --gamma 0.55 \ 

​                         --tau 0.6  --sim_method exp \

​                         --num_epochs 300 --early_stop --patience 10 &> ./outputs/Cora/COIN.out&  



CUDA_VISIBLE_DEVICES=$gpu_id, nohup python COIN.py --dataset CiteSeer --lr1 5e-4 --lr2 5e-4 --wd 1e-5 --hid_dim 256 --proj_dim 128\ 

​                         --alpha 0.7 --gamma 0.65 \ 

​                         --tau 0.9  --encoder_layer 2 \ 

​                         --num_epochs 300 --early_stop --patience 10 &> ./outputs/CiteSeer/COIN.out&  

\```



You can change all the hyperparameters as you need. To run COIN on different dataset, setting dataset='xxx' and we support 'Cora', 'CiteSeer', 'PubMed', 'Coauthor-Phy', 'Coauthor-CS', 'Amazon-Photo', 'Chameleon', 'Texas', 'Cornell', 'Squirrel'. You can see further information in datasets.py