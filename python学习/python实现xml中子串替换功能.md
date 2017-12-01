---
title: python实现xml中子串替换功能
date: 2017-03-13 16:44:16
tags: python
categories:
    - 脚本
    - 
password: 
---

今天遇到一个问题，说的是要把一个android res目录下，所有name=xx的字符串的值，自己参照网上的方法，写了一个脚本。记录如下，方便以后使用


```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-
import os
import re

#定义一个函数，筛选所有文件
#list files
def listFiles(dirPath):
#walk方法，root-文件夹路径；dirs-含有的子文件夹；files-含有的文件
    for root,dirs,files in os.walk(dirPath):
        for fileObj in files:
            if "-es" in root:
                #获得某个文件的绝对路径并添加到列表中
                fileList_es.append(os.path.join(root,fileObj))
            elif "-en" in root:
                fileList_en.append(os.path.join(root,fileObj))
            elif "-fr" in root:
                fileList_fr.append(os.path.join(root,fileObj))
            else:
                fileList_nor.append(os.path.join(root,fileObj))
#定义一个函数，更改字符串
def changeString(style):
    changeText = ""
    if style == "en":
        fileList = fileList_en
        changeText = '"You are going to use the roaming data service ,any price information please contact your service provider"'
    elif style == "es":
        fileList = fileList_es
        changeText = '"Usted va a utilizar el servicio de datos en itinerancia, cualquier información de precios por favor póngase en contacto con su proveedor de servicios"'
    elif style == "fr":
        fileList = fileList_fr
        changeText = '"Vous allez utiliser le service de données en itinérance, les informations de prix s\'il vous plaît contacter votre fournisseur de services"'
    else:
        fileList = fileList_nor
        changeText = '"You are going to use the roaming data service ,any price information please contact your service provider"'

    for fileObj in fileList:
        #以读写模式打开文件
        f = open(fileObj,'r+')
        #读取所有内容,每一行放到一个列表里面
        all_the_lines=f.readlines()
        #光标移动到文章开头
        f.seek(0)
        #这个是截取文章，它是把文章清空了，感觉不合理，明显影响了效率
        f.truncate()

        for line in all_the_lines:
            print(line)
            #print (pattern)
            #用来获得匹配的字符
            res = pattern.search(line)
            if res != None:
                res = res.groups()
                #把每一行的内容替换掉了以后重新写入
                f.write(line.replace(res[0],changeText))
            else:
                f.write(line)

        f.close()
def main():
    #考虑到脚本的通用性，这个尽量不要写死
    #fileDir = "/mnt/zwx318792/hq6735/packages/services/Telephony/res"
    fileDir = '.'

    listFiles(fileDir)
    changeString("en")
    changeString("es")
    changeString("fr")
    changeString("nor")
#这个是用来生成一个正则表达式条件
pattern = re.compile('<string name="roaming_warning".*?>(.*)</string>')
fileList_en=[]
fileList_es=[]
fileList_fr=[]
fileList_nor=[]
#这个好像是python脚本的固定写法
if __name__=='__main__':

    main()
    exit()
```

