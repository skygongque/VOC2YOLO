# 将VOC数据集转化为yolov5 yolov8可使用的数据集

> 以yolov8 的训练过程为例说明训练过程

## 数据集准备
可以使用官方推荐的`roboflow`对数据进行标注，是个在线的数据标注网站，标注后支持导出yolo各个版本的训练数据集，唯一的缺点是因为网络原因太卡了。  

使用`精灵助手`进行数据VOC数据集的标注导出VOC格式的数据集  
- 将图片放入`JPEGImages`目录
- 将xml文件放入`Annotations`目录  


使用脚本将VOC数据集转化为yolov8可使用的数据集  
编写数据集的配置文件如`mydataset.yaml`  
```
path: ../pathtodataset 
train: images 
val: images

nc: 2
names: ['b1','b2']
```

## 训练过程

### 安装依赖
Pip install ultralytics and dependencies and check software and hardware.
```
%pip install ultralytics
import ultralytics
ultralytics.checks()
```


### 训练
```
!yolo train model=yolov8n.pt data=/content/b1b2datebase/my.yaml epochs=50 imgsz=640
```
### 验证
```
!yolo val model=runs/detect/train4/weights/best.pt data=/content/datasets3/data2.yaml
```

### 预测
Run inference on an image with YOLOv8n
```
!yolo predict model=yolov8n.pt source='https://ultralytics.com/images/zidane.jpg'
```
Run inference on an image with trained model
```
!yolo predict model=runs/detect/train4/weights/best.pt  source='/content/datasets3/images/val/16834710259451773.jpg'
```


### python使用
```
from ultralytics import YOLO

# Load a model
# model = YOLO('yolov8n.yaml')  # build a new model from scratch
model = YOLO('runs/detect/train4/weights/best.pt')  # load a pretrained model (recommended for training)

# Use the model

results = model('/content/datasets3/images/val/16834710259451773.jpg')  # predict on an image
print(results)
```


### 其他

若上传zip格式的数据集到云端，可用以下命令解压数据集
```
!unzip -q temp.zip -d b1b2datebase && rm temp.zip  # unzip
```
```
import locale
locale.getpreferredencoding = lambda: "UTF-8" # 用于解决编码相关错误
```