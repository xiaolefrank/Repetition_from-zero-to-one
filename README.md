# 从0开始的项目复现指南

## 复现项目

[ReCAM Github link](https://github.com/zhaozhengChen/ReCAM)
[arXiv link](https://arxiv.org/abs/2203.00962)

## 环境配置

~~1. 安装`requirement.txt`里面各个包,基本就是`conda install + package name`就行.不过记得配置清华源:);
2. 建议在linux环境下配置,不然问题比较多;~~

windows配置破防了,换了WSL,装的Ubuntu22.04LTS
没想到一次复现状况百出:(

一般来说是`git clone`复现链接然后对着`environment.yml`直接conda安装就好了.

```shell
cd /ReCAM #destination folder
git clone + [url] 
conda create -f environment.yml -n #it have your environment name 
#option -python=3.x
```

我这次安装出现`pip install error`,解决方案是把这个yml文件中的`-pip:`后面剩下的部分拆分成一个单独的`requirement.txt`,激活环境上面创建的conda环境然后再`pip`或者`pip3`安装剩下的依赖项.

```shell
conda activate recam #your environment name
pip install -r requirement.txt
```

## training

以这次项目的`VOC`数据集为例:

**Step1:** download dataset到指定目录,本项目要求是新建一个`voc12_root`的folder;

```shell
aria2c http://host.robots.ox.ac.uk/pascal/VOC/voc2012/VOCtrainval_11-May-2012.tar
```

**Step2:** 解压tar文件;

```shell
tar -xvf #filename
rm #filename
```

**Step3:** 激活seed;

首先指定一个workspace存放models和logs,specify为folder `workspaces`.

然后run python脚本:

```shell
CUDA_VISIBLE_DEVICES=0 python run_sample.py --voc12_root ./VOCdevkit/VOC2012/ --work_space workspaces --train_cam_pass True --train_recam_pass True --make_recam_pass True --eval_cam_pass True 
```

