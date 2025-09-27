# Data
## FeMNIST
* [x] 下载完整数据集`all_data`(json文件)
* [x] 以non-iid方式划分数据给`m`个agents
![[Pasted image 20250731011602.png]]
需要手动对原始json数据进行合并和重分配（调整参数$\alpha$的值进行不同`non-iid`程度的模拟）,是目前学术界FL领域对`non-iid`进行模拟的标准方法。
```bash
cd datasets/femnist
source preprocess.sh --name femnist -s dirichlet -sf 0.1 --agents 10 --alpha 0.5 --smplseed 42 -t sample -tf 0.9
source gen_pickle_dataset.sh "femnist" "../datasets" "./pickle_datasets"
```

```bash
cd datasets/sent140
# source preprocess.sh -s dirichlet -sf 0.05 --agents 10 --alpha 10 --smplseed 42
python ../utils/dirichlet_partition.py --name sent140 --num_agents 10 --alpha 10 --fraction 0.2 --train_frac 0.9 --seed 42
source gen_pickle_dataset.sh "sent140" "../datasets" "./pickle_datasets"
```

```bash
cd datasets/shakespeare
# source preprocess.sh -s dirichlet -sf 0.2 --agents 10 --alpha 0.5 --smplseed 42
python ../utils/dirichlet_partition.py --name shakespeare --num_agents 10 --alpha 0.5 --fraction 0.2 --train_frac 0.9 --seed 42
source gen_pickle_dataset.sh "shakespeare" "../datasets" "./pickle_datasets"
```

# Model

# Algorithm
