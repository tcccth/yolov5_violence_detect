## 一. 模型修改  

### 1. 使用模型  

* 本次训练使用了 yolov5s 模型，所以需要对部分配置文件进行修改：

  > 需要对位于文件夹内 ./models/yolov5s.yaml 进行修改，训练修改了其的复制文件：
  >
  > ![image-20240702130507391](C:\Users\13371\AppData\Roaming\Typora\typora-user-images\image-20240702130507391.png)
  >
  > 根据自身数据集标签数量对 nc 进行修改。
  >
  > 同时，对 ./data/train.yaml 进行修改：
  >
  > ![image-20240702130935631](C:\Users\13371\AppData\Roaming\Typora\typora-user-images\image-20240702130935631.png)   
  >
  > 最后修改训练文件：
  >
  > ![image-20240702131035186](C:\Users\13371\AppData\Roaming\Typora\typora-user-images\image-20240702131035186.png)
  >
  > 并且调整其中参数。

  

## 二. 训练数据

## 1. 性能指标分析

### (1) 模型数据

> 训练好的模型保存在 ./runs/train/exp2/weights 中
>
> 其分析数据存放于同级文件夹下
>
> ![image-20240702131927555](C:\Users\13371\AppData\Roaming\Typora\typora-user-images\image-20240702131927555.png)

### (2). 数据分析

* lables 反映了训练集的数据量，以及其图片和标签的部分参数

  > ![image-20240702132751484](C:\Users\13371\AppData\Roaming\Typora\typora-user-images\image-20240702132751484.png)
  >
  > ![image-20240702132812189](C:\Users\13371\AppData\Roaming\Typora\typora-user-images\image-20240702132812189.png)

* results 反映了训练集和数据集的结果，包含了：训练次数，GPU消耗，边界框损失，目标检测损失，total，targets, 图片大小，P，R，mAP@.5, mAP@.5:.95, 验证集val Box, 验证集val obj, 验证集val cls

> ![image-20240702132439965](C:\Users\13371\AppData\Roaming\Typora\typora-user-images\image-20240702132439965.png)
>
> 上图为 train.csv

同时，也可以通过损失函数评估模型性能。

- 定位损失box_loss：预测框与标定框之间的误差（GIoU）

- 置信度损失obj_loss：计算网络的置信度

- 分类损失cls_loss：计算锚框与对应的标定分类是否正确

  > ![image-20240702132925434](C:\Users\13371\AppData\Roaming\Typora\typora-user-images\image-20240702132925434.png)
  >
  > 上图为train.png

**特别注意到：在最后数轮次的训练中，验证集的损失有所上升，可能需要更换置信度损失** 

其他曲线趋于稳定，**cls_loss则始终为0，可能是class设置不多，模型训练未将其丢失** 

### (3). 模型构成

于pycharm终端 ，输入 `tensorboard --logdir runs`，将结果可视化

> ![image-20240702134122722](C:\Users\13371\AppData\Roaming\Typora\typora-user-images\image-20240702134122722.png)

通过浏览器查看可视化训练结果

> ![image-20240702134256003](C:\Users\13371\AppData\Roaming\Typora\typora-user-images\image-20240702134256003.png)

于其中查看本次模型的构成：

> ![image-20240702134339607](C:\Users\13371\AppData\Roaming\Typora\typora-user-images\image-20240702134339607.png)
>
> ![train_exp2](C:\Users\13371\Downloads\train_exp2.png)
>
> 可以看到，模型通过多层卷积，采样，实现深度学习。
>
> ![image-20240702134652066](C:\Users\13371\AppData\Roaming\Typora\typora-user-images\image-20240702134652066.png)
>
> 上图yolov5s的代码部分

## 训练结果 

模型将会检测视频或图片中的暴力行为

![image-20240702140535731](C:\Users\13371\AppData\Roaming\Typora\typora-user-images\image-20240702140535731.png)

![image-20240702140509069](C:\Users\13371\AppData\Roaming\Typora\typora-user-images\image-20240702140509069.png)

![image-20240702140419713](C:\Users\13371\AppData\Roaming\Typora\typora-user-images\image-20240702140419713.png)

![image-20240702140610667](C:\Users\13371\AppData\Roaming\Typora\typora-user-images\image-20240702140610667.png)

**注意到： 模型在面对人体剧烈姿态时，可能会出现假正例，以及部分假负例。可能是模型对人体姿态和二者距离之间产生错误联系。需要对数据集标签重新精确标定，并且加大数据集训练** 