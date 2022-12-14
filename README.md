# Graduate Project : Night-time data Neural Architecture Search for image segmentation
our project is testing Neural Architecture Search with Night-time data image segmentation 
we had a some question like "is NAS works even night time? and is there any difference between night-time and day-time?" 
so we did night-time Neural Architecture Search 
we use [Auto-Deeplab](https://arxiv.org/abs/1901.02985) for NAS Method and we use [NightCity](https://dmcv.sjtu.edu.cn/people/phd/tanxin/NightCity/index.html) for our dataset 

we clone this [repo](https://github.com/NoamRosenberg/autodeeplab) for base-line of Auto-Deeplab implementation and we change some code like dataloader or argment file  


## Auto-Deeplab search space and search parameter
Following the popular trend of modern CNN architectures having a two level hierarchy. Auto-Deeplab forms a dual level search space, searching for optimal network and cell architecture.
![network and cell level search space](./images/networkandcell.png)


Auto-Deeplab performance 
![model results](./images/results.png)

## Our Results
![Our Search Results image of NightCity data](./images/our_search_result.png)
![Our Results image of NightCity validation](./images/NightCityResult.png)
**Our performance of NightCity validation : 44.2% mIoU** 
**Our performance of NightTimeDriving test : 31.5% mIoU**


## Training Proceedure

**All together there are 3 stages:**

1. Architecture Search - Here you will train one large relaxed architecture that is meant to represent many discreet smaller architectures woven together.

2. Decode - Once you've finished the architecture search, load your large relaxed architecture and decode it to find your optimal architecture.

3. Re-train - Once you have a decoded and poses a final description of your optimal model, use it to build and train your new optimal model

<br/><br/>

**Hardware Requirement**

* For architecture search, you need at least an 15G GPU, or two 11G gpus(in this way, global pooling in aspp is banned, not recommended)

* For retraining autodeeplab-M or autodeeplab-S, you need at least n more than 11G gpus to re-train with batch size 2n without distributed

* For retraining autodeeplab-L, you need at least n more than 11G gpus to re-train with batch size 2n with distributed
 
 ## Architecture Search


***Begin Architecture Search***

**Start Training**

```
python train_autodeeplab.py --batch-size 8 --dataset <datasetname> --checkname <check_dir_name> --alpha_epoch 2
0 --filter_multiplier 8 --resize 512 --crop_size 321 --gpu-ids 0,1
```

**Resume Training**

```
python train_autodeeplab.py --batch-size 8 --dataset<datasetname> --checkname <check_dir_name> --alpha_epoch 2
0 --filter_multiplier 8 --resize 512 --crop_size 321 --gpu-ids 0,1 --resume ./checkpoint.pth.tar
```

## Re-train

***Now that you're done training the search algorithm, it's time to decode the search space and find your new optimal architecture. 
After that just build your new model and begin training it***


**Load and Decode**
```
python decode_autodeeplab.py --dataset <datasetName> --resume ./checkpoint.pth.tar

```

## Retrain

**Train without distributed**
```
python train.py --dataset <datasetName> --batch_size 16 --net_arch ./network_path_space.npy --cell_arch ./genotype.npy --network_path ./network_path.npy --checkname <check_dir name>
```





## Requirements

* Pytorch version 1.1

* Python 3

* tensorboardX

* torchvision

* pycocotools

* tqdm

* numpy

* pandas

* apex

## References
[1] : [Auto-DeepLab: Hierarchical Neural Architecture Search for Semantic Image Segmentation](https://arxiv.org/abs/1901.02985)

[2] : [Thanks for jfzhang's deeplab v3+ implemention of pytorch](https://github.com/jfzhang95/pytorch-deeplab-xception)

[3] : [Thanks for MenghaoGuo's autodeeplab model implemention](https://github.com/MenghaoGuo/AutoDeeplab)

[4] : [Thanks for CoinCheung's deeplab v3+ implemention of pytorch](https://github.com/CoinCheung/DeepLab-v3-plus-cityscapes)

[5] : [Thanks for chenxi's deeplab v3 implemention of pytorch](https://github.com/chenxi116/DeepLabv3.pytorch)

[6] : [Thanks for NoamRosenberg's autodeeplab implementation of pytorch](https://github.com/NoamRosenberg/autodeeplab) 
