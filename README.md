## Introduction
CodeGen������ģ������Salesforce������һ��ǿ����Զ����������ɹ��ߡ���ּ��ͨ����Ԫ����ת��Ϊ�ɶ�����ά����Դ���룬���������߸�Ч�ع����͹������Apex��Salesforce�Ķ��Ʊ�����ԣ���Ӧ�ó���ԭ��Ŀ��[CodeGen](https://github.com/salesforce/CodeGen)

����Ŀ�ǻ���CodeGen������ģ�͵�һ��Flask�汾��ʹ��Python���׿����ṩ���ӻ�web�������ԭ��Ŀ�ṩ�˿��ӻ�ƽ̨�͸�����ϸ�Ľ̡̳�

---

CodeGen Large Language Model is a powerful automated code generation tool developed by Salesforce. It is designed to help developers efficiently build and manage applications based on Apex (Salesforce's customized programming language) by transforming metadata into readable, maintainable source code, Original project: [CodeGen](https://github.com/salesforce/CodeGen)

This project is based on CodeGen large language model of a Flask version, using Python simple development to provide visualization web services. Compared with the original project, it provides a visualization platform and more detailed tutorials.

## Model Architecture
CodeGenʹ���Իع���ʽ��transformer����Ȼ���Ժͱ���������ݼ��Ͻ���ѵ����ģ�ͳߴ������350M��2.7B��6.1B��16.1B��

CodeGenģ��������������ݼ���˳��ѵ����THEPILE��BIGQUERY��BIGPYTHON��

THEPILE��һ���������Խ�ģ��825.18GBӢ�����ݼ��������ݼ��ǻ���22���������Ӽ�����ģ�����һ���Ǵ�GitHub���ռ��ı���������ݣ�ռ�������ݼ���7.6%�����ڸ����ݼ�ѵ����ģ�ͳ�Ϊ��Ȼ����CodeGenģ��(CodeGen-NL)��

���������ݼ�BIGQUERY��Google�������ݼ�BigQuery���Ӽ��������ɶ��ֱ�����Թ��ɵġ�ѡ��6�ֱ������C��C++��Go��Java��JavaScript��Python���ж�����ѵ��������BIGQUERY��ѵ����ģ��Ϊ������CodeGenģ��(CodeGen-Multi)��

���������ݼ�BIGPYTHON�����˴���python�����ݡ������ݼ��а�������2021��10�µĹ������ɻ�ȡ�ҷǸ��˵�python���롣����BIGPYTHON��ѵ����ģ��Ϊ������CodeGenģ��(CodeGen-Mono)��

releases�����ṩ��350M��momoģ�͹��������ԣ�����ģ�Ϳ�����[huggingface����](https://huggingface.co/models?search=salesforce+codegenl)�������ء�

---

CodeGen is trained on natural language and programming language datasets using an autoregressive form of transformer. Model sizes include: 350M, 2.7B, 6.1B, and 16.1B.

CodeGen model families are trained sequentially on three datasets: THEPILE, BIGQUERY and BIGPYTHON.

THEPILE is an 825.18GB English dataset used for language modeling. The dataset is constructed based on 22 high-quality subsets, one of which is programming language data collected from GitHub, which accounts for 7.6% of the entire dataset. The model trained based on this dataset is called CodeGen model for natural languages (CodeGen-NL).

The multilingual dataset BIGQUERY is a subset of Google's publicly available dataset BigQuery, which is composed of multiple programming languages. Six programming languages C, C++, Go, Java, JavaScript and Python are selected for multilingual training. Call the model trained on BIGQUERY a multilingual CodeGen model (CodeGen-Multi).

The monolingual dataset BIGPYTHON contains a large amount of data for python. The dataset contains publicly available, accessible and non-personal python code through October 2021. Call the model trained on BIGPYTHON a monolingual CodeGen model (CodeGen-Mono).

The releases section provides a 350M momo model for environmental testing, and other models can be downloaded at [huggingface official website](https://huggingface.co/models?search=salesforce+codegenl).

## Quick Start
After setup the environment, run the CodeGenFlask server using `start.sh`.
```bash
Usage: ./start.sh [-h <host>] [-p <port>]
Example:
./start.sh  # default host: 0.0.0.0, port: 5000
./start.sh -p 8000
./start.sh -h 0.0.0.0 -p 8000
```
Follow the link: http://localhost:5000/, the expected output is that:
![alt text](images/image.png)

Input the test prompt:
```python
import smtplib
from email.mime.text import MIMEText
from email.utils import formataddr
 
sender='abc@163.com'    # �����������˺�
passtoken = 'PASSWORD'              # ��������������
receiver='123@qq.com'      # �ռ��������˺�
def mail():
```
After generating the code, the result will show in the web page below.
![alt text](images/image-1.png)

The history will be saved at end.
![alt text](images/image-2.png)

You can change the model by editing the model path in `app.py`.

## Environment Setup 
(1) Download Python 3.10 from [here](https://www.python.org/downloads/) and install it. Or you can install it using wget and compile it yourself.

```bash
# download the python 3.10 source code
wget https://www.python.org/ftp/python/3.10.11/Python-3.10.11.tar.xz
tar -xf Python-3.10.11.tar.xz
cd Python-3.10.11
# set the installation path
./configure --enable-optimizations  --prefix=/usr/local/python3.10
# compile and install
make -j8
sudo make altinstall
# set up the symbolic links for python3.10 and pip3.10
sudo ln -s -b /usr/local/python3.10/bin/python3.10 /usr/bin/python3.10
sudo ln -s -b /usr/local/python3.10/bin/pip3.10 /usr/bin/pip3.10
```
(2) Create the virtual environment with python 3.10 using venv.
```bash
# Download the CodeGenFlask source code
cd ~
git clone https://github.com/d0v0e/CodeGenFlask.git
# Create a virtual environment with python 3.10
cd CodeGenFlask
mkdir ./env && cd ./env
python3.10 -m venv ./env/py3-10_env
```
(3) Download the test model from huggingface or [here](https://github.com/d0v0e/CodeGenFlask/releases), and unzip it to the CodeGenFlask `CodeGenFlask/model/` folder.
```bash
wget https://github.com/d0v0e/CodeGenFlask/releases/download/v0.1.0/codegen-350M-mono.tar.gz
tar -xzvf codegen-350M-mono.tar.gz
mv codegen-350M-mono ~/CodeGenFlask/model/
```
(4) Activate the env, install and test the requirements using `set_up.sh`.
```bash
cd ~/CodeGenFlask
./set_up.sh
```
The expected output is that:
<img src=./images/image1.png style=zoom:50%;>

If your torch and transformers throw errors, you can check your cuda version by command `nvidia-smi`, and goto [Pytorch's official website](https://pytorch.org/) to find the compatibale version.