1. 下载zip
2. pip install -r requirements.txt
3. python setup.py develop
- 我提示了：No such file or directory: ':/usr/local/cuda-11.2/bin/nvcc'
- 在bashrc里“export CUDA_HOME=/usr/local/cuda”就OK了
- 又提示`[Errno 13] Permission denied: '/home/xilm/anaconda3/lib/python3.9/site-packages/setuptools.pth'`
- 找到该文件，使用`chmod 777 + filesname`获取权限
- 最终运行成功，log文件放在该文件夹下
