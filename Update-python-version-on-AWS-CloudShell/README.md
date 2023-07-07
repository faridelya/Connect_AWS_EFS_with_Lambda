# Update Python version on AWS-CloudShell

### Follow these steps.

```
sudo yum update -y

```
```
sudo yum groupinstall -y development

```
- I am using python3.8 you can get tgz python package link from offical webpage and use with wget
```
wget https://www.python.org/ftp/python/3.8.12/Python-3.8.12.tgz

```
```
tar xzf Python-3.8.12.tgz

```
```
cd Python-3.8.12

```
```
./configure --enable-optimizations

```
```
sudo make altinstall

```
```
python3.8 --version

```

- Install any thing in folder use this
```
pip3.8 install -r requrements.txt -t packages/
```
- if you want to add this folder to s3 for other use like to connect to lambda layer
```
zip -r packages.zip  packages
```
- copy this zip to s3
```
aws s3 cp packages.zip s3://you-bucket-name
```
- now check s3 and confirm that this zip folder is copied successfully 
