---
title: Running a Jupyter Notebook from Kaggle
tags: 
keywords: rouge, pygments, prettify, color coding,
last_updated: Sept 5, 2022
summary: "Create a Kaggle notebook and copy the codes from below to run a Jupyter notebook using Kaggle memory and GPUs on ngrok server. "
sidebar: mydoc_sidebar
permalink: jupyter_kaggle.html
folder: mydoc
---


[Link to Kaggle Notebook](https://www.kaggle.com/code/bharatb964/run-jupyter/edit/run/91012694)

## Clone from Github Repository

```python
!pip install -q seedir
!sudo apt-get -qq install git-lfs
!pip install -q colabcode

import os
import seedir as sd
from datetime import date

%cd /kaggle/working

repository = 'Colab_Notebooks'
branch = 'main'

!git config --global user.email "mebharat123@gmail.com"
!git config --global user.name "Bharat"
username='bharatb964'
git_token='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' # make sure Github.ipynb is in .gitignore
github = 'github.com'

import shutil
if repository in os.listdir('..'):
    os.chdir('..')
    shutil.rmtree(repository)

!git clone https://{git_token}@{github}/{username}/{repository}
%cd {repository}
!git checkout $branch
#!git checkout -b $branch

sd.seedir('.',style='emoji',exclude_folders=[f for f in os.listdir() if f[0] in ['.','_','~']],depthlimit=3)
```

## Run Jupyter Notebook
```python
!pip install -q pyngrok
import os
import subprocess
import uuid
from pyngrok import ngrok
import re

ngrok.set_auth_token("get_token_from_website")

token = str(uuid.uuid1())

active_tunnels = ngrok.get_tunnels()
for tunnel in active_tunnels:
    public_url = tunnel.public_url
    ngrok.disconnect(public_url)

url = ngrok.connect(addr=10001, bind_tls=True)
print("Click on the link: "+re.findall("https://.*io",str(url))[0]+"/tree?token="+token)

lab_cmd=f"jupyter-lab --ip='localhost' --allow-root --ServerApp.allow_remote_access=True --no-browser --ServerApp.token='{token}' --port 10001"
os.system(f"fuser -n tcp -k 10001")
with subprocess.Popen([lab_cmd],shell=True,stdout=subprocess.PIPE,bufsize=1,universal_newlines=True,) as proc:
    for line in proc.stdout:
        print(line, end="")
        
```

## Pring the logs
```python

print('\033[1m' +"Git Remote Location --->" +'\033[0m')
!git remote -v  
print('\033[1m' +"Status --->" +'\033[0m')
!git status
print('\033[1m' +"Branches --->" +'\033[0m')
!git branch -a
print('\033[1m' +"Git Log --->" +'\033[0m')
!git log -n 5 --oneline
print('\033[1m' +"Gitignore file --->" +'\033[0m')
!cat .gitignore
print('\033[1m' +"Git lfs tracked files --->" +'\033[0m')
!cat .gitattributes
```
## Write gitignore and git lfs
```python

%%writefile .gitignore
.*
~*
*.log
.ipynb*
output/aclImdb/*

!git rm -r --cached . 
!git lfs track input/*
!git lfs track output/*
!git add -f .gitattributes
```
## Push the changes made
```python

import subprocess
lines = subprocess.check_output(
    ['git', 'log'], stderr=subprocess.STDOUT)

for i in str(lines).split('\\n'):
    if i.startswith(' '):
        d=i.split(' ')[-1]
        break

k=list(map(int,d.split('-')))
if date.today()==date(k[0],k[1],k[2]):
    message = "\""+"Made changes on branch \"{}\" on date {}".format(branch, str(date.today()))+"\""
    !git add .
    !git commit --amend -m $message
    !git push -f origin $branch
else:
    message = "\""+"Made changes on branch \"{}\" on date {}".format(branch, str(date.today()))+"\""
    !git add .
    !git commit -m $message
    !git push origin $branch
```
