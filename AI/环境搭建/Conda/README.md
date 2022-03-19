# Conda

## 安装Anaconda或者Miniconda

下载Mniconda3，使用默认选项，不需要将Mniconda3加入环境变量，安装完成后打开Anaconda Prompt。

## 更换镜像源

```cpp
conda config --add channels https://mirrors.bfsu.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.bfsu.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.bfsu.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.bfsu.edu.cn/anaconda/cloud/pytorch/
conda config --add channels https://mirrors.bfsu.edu.cn/anaconda/cloud/pytorch-lts/
conda config --set show_channel_urls yes
```



## 常用命令

```bash
conda env list		# 列出环境
conda active xxx		# 使用xxx环境
conda install nump		# 安装软件
conda update numpy
conda remove numpy
conda clean --all		# 清除不完整安装缓存
```

```bash
# 指定python版本为2.7，注意至少需要指定python版本或者要安装的包
# 后一种情况下，自动安装最新python版本
conda create -n env_name python=2.7
conda remove -n env_name --all				# 移除环境
```

```bash
conda list			# 查看环境内可用软件包的指令
pip list			# 查看已下载的软件包
```



### cpuonly

注意查看环境中有没有安装cpuonly这个安装包，pytorch只能使用cpu而不能使用GPU可能因为这个。



## 官方下载库

https://download.pytorch.org/whl/torch_stable.html







## CUDA,CUDNN,NVCC等

https://zhuanlan.zhihu.com/p/91334380



