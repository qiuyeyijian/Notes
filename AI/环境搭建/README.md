# 框架搭建

## TensorFlow

```bash
# Requires the latest pip
pip install --upgrade pip

# Current stable release for CPU and GPU
pip install tensorflow

# Or try the preview build (unstable)
pip install tf-nightly
```



## Pytorch

```bash
# 后面不要加-c pytorch
conda install pytorch torchvision torchaudio cudatoolkit=11
conda install cudnn

cpu uninstall cpuonly		# 注意查看有没有安装cpuonly
```



