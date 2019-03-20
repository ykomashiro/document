
# DEMO

主要是用于记录平时在编程中遇到问题时从网上站到的解决方法

## 指定Google Drive文件夹

### Google drive授权

下面代码存储了google的授权过程，执行下面的代码，中间会出现两次提示，要求打开授权地址，填写drive授权码。两次的授权权限不一样，都要填写。

```python
!apt-get install -y -qq software-properties-common python-software-properties module-init-tools
!add-apt-repository -y ppa:alessandro-strada/ppa 2>&1 > /dev/null
!apt-get update -qq 2>&1 > /dev/null
!apt-get -y install -qq google-drive-ocamlfuse fuse
from google.colab import auth
auth.authenticate_user()
from oauth2client.client import GoogleCredentials
creds = GoogleCredentials.get_application_default()
import getpass
!google-drive-ocamlfuse -headless -id={creds.client_id} -secret={creds.client_secret} < /dev/null 2>&1 | grep URL
vcode = getpass.getpass()
!echo {vcode} | google-drive-ocamlfuse -headless -id={creds.client_id} -secret={creds.client_secret}

!mkdir -p drive
!google-drive-ocamlfuse drive
```

### 切换工作路径

在colab环境中，我们挂载Google Drive的位置是 /content/drive/。colab中的notebook和py文件默认都是以/content/作为工作目录，需要执行一下命令手动切换工作目录：

```python
import os

path = "/content/drive/colab-notebook/lesson1-week2/assignment2"
os.chdir(path)
os.listdir(path)
```

## Tensorflow objection dection API

以下是如何在Linux和Windows上部署Tensorflow的具体步骤。

### 安装必要的库

Tensorflow Object Detection API 依赖如下组件包:

    Protobuf 2.6
    Python-tk
    lxml
    tf Slim (已在 "tensorflow/models/research/" 目录下)
    cocoapi

coco api主要是用于度量模型在ＣＯＣＯ数据集上的分值（mAP指标等，目前暂时不需要，以后需要时可在进行安装

```bash
git clone https://github.com/cocodataset/cocoapi.git
cd cocoapi/PythonAPI
make
cp -r pycocotools <path_to_tensorflow>/models/research/
```

