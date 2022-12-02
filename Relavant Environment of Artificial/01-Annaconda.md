### Annaconda

#### Annaconda安装

安装时，要注意勾选**环境变量配置**

#### 更改conda源（后续安装第三方库可以加快速度）
在Anaconda prompt中操作：

```powershell
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
```


查看是否修改好通道：

```python
conda config --show channels
```

#### Pytorch 安装

直接去Pytorch官网选择对应版本下载，注意要下载到对应的annaconda的环境内