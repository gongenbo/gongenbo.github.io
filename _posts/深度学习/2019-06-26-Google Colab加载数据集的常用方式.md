---
layout: post
title: Google Colab加载数据集的常用方式
categories: [深度学习]
description: colab加载数据集
keywords: colab,gpu,加载数据集
---
### 1.colab介绍
#### 1.1 简介
colab是google推出的一个 Jupyter 笔记本云端环境，提供免费的gpu，其笔记本存储在 Google 云端硬盘中，可以方便的使用Keras,TensorFlow,PyTorch等框架进行深度学习应用的开发。

Google Drive: [https://drive.google.com/drive](https://drive.google.com/drive)

Colab: [https://colab.research.google.com/drive/](https://colab.research.google.com/drive/)

#### 1.2 使用方法
- 将.ipynb文件上传到谷歌云盘，点选邮件使用"colab"打开

#### 1.3 配置gpu加速
- 点击"修改"-->"笔记本配置"--->"硬件加速配置"选择gpu即可

![Alt text]({{site.url}}/img/deeplearn/20190626_start01.png)

### 2.数据集方式

#### 2.1 使用!wget加载数据集

Colab其实是一台带有GPU的Centos虚拟机，可以直接使用linux的`wget`命令下载数据集，下载速度可以达到60-130mb/s

- 下载并解压数据集命令：
```
#!wget https://download.pytorch.org/tutorial/hymenoptera_data.zip
#!unzip hymenoptera_data.zip -d ./
```
- 加载数据集命令：
```
# 使用 ImageFolder 定义数据集
# 定义数据预处理
train_tf = tfs.Compose([
    tfs.RandomResizedCrop(224),
    tfs.RandomHorizontalFlip(),
    tfs.ToTensor(),
    tfs.Normalize([0.485, 0.456, 0.406], [0.229, 0.224, 0.225]) # 使用 ImageNet 的均值和方差
])
train_set = ImageFolder('./hymenoptera_data/train/', train_tf)
```

#### 2.2 挂载谷歌云盘加载数据集
- 需要先将文件上传到谷歌云盘，比如data/data.csv
- 在colab中挂载谷歌云盘的命令如下，执行后会要求输入谷歌账号的key即可挂载
```
from google.colab import drive
drive.mount('/content/drive/')
```
- 加载数据集
```
train = pd.read_csv('/content/drive/My Drive/Colab Notebooks/data/data.csv')
```

#### 2.3 使用上传按钮上传

- 使用上传按钮上传如下图，这种方法适合数据集不大的情况：

![Alt text]({{site.url}}/img/deeplearn/20190626_colab01.png)


#### 2.4 从kaggle加载数据集
- 如果你是在kaggle打比赛，上面整好有你需要的数据集，可以直接使用kaggle命令下载，需要在kaggle的my profile里面选择创建api token，然后在本地生成username与key

![Alt text]({{site.url}}/img/deeplearn/20190626_kaggle01.png)

```
{"username":"gongenbo","key":"f26dfa65d06321a37f6b8502cd6b8XXX"}
```

- 下面以驾驶状态检测项目为例，地址:[https://www.kaggle.com/c/state-farm-distracted-driver-detection/data](https://www.kaggle.com/c/state-farm-distracted-driver-detection/data)
- 通过kaggle下载数据的命令
```
!pip install -U -q kaggle
!mkdir -p ~/.kaggle
!echo '{"username":"gongenbo","key":"f26dfa65d06321a37f6b8502cd6b8XXX"}' > ~/.kaggle/kaggle.json
!chmod 600 ~/.kaggle/kaggle.json
!kaggle competitions download -c state-farm-distracted-driver-detection
```
- 下载信息
```
Downloading sample_submission.csv.zip to /content
  0% 0.00/193k [00:00<?, ?B/s]
100% 193k/193k [00:00<00:00, 71.7MB/s]
Downloading imgs.zip to /content
100% 3.99G/4.00G [02:13<00:00, 44.2MB/s]
100% 4.00G/4.00G [02:13<00:00, 32.1MB/s]
Downloading driver_imgs_list.csv.zip to /content
  0% 0.00/92.9k [00:00<?, ?B/s]
100% 92.9k/92.9k [00:00<00:00, 100MB/s]
```
- 训练完将成绩提交到kaggle的命令
```
!kaggle competitions submit -c state-farm-distracted-driver-detection -f submission.csv -m "Message"
```

### 3.其他常用命令

```

#!/opt/bin/nvidia-smi
#!cat /proc/meminfo
#!cat /proc/cpuinfo
#!df -h
```

### 4.总结
1. 需要科学上网才能使用colab，方式请自行查找
2. 使用过程中经常断开连接，点击右上角箭头"重新连接到托管平台"即可
3. Colab最多连续使用12小时，超过时间系统会强制掐断正在运行的程序并收回占用的虚拟机。
4. 在使用过程中训练驾驶员状态检测10分类问题5G数据，Tesla K80 12G显卡也就只能跑个ResNet18层的网络，34层有点吃力，跑50层的就内存溢出了，所以大网络开始有点吃力的。

