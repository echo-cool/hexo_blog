---
title: 百度AI API测试
url: 149.html
id: 149
categories:
  - Python
date: 2018-04-14 12:09:04
tags:
---

源于一个想做OCR的想法 所以就写了这个程序 一开始只有OCR的功能 没有调用其他API 后来看到还有一大堆的AI技能 就加了一些其他的

* * *

语言 `python`

* * *

下面是识别图片文字的函数

def wenzishibie():
    
    ws = ''
    client = AipOcr(APP\_ID, API\_KEY, SECRET_KEY)
    print("Working.......Please Wait..........")
    result = client.basicAccurate(get\_file\_content(x))
    long = result\['words\_result\_num'\]
    long = int(long)
    for i in range(long):
        print(result\['words_result'\]\[i\]\['words'\])

        ws += result\['words_result'\]\[i\]\['words'\]
    print("Do you need i  read it ?(Y or N):")
    ppp = input()
    if ppp == 'Y':
        voice(ws)
    else:
        print("Ok")

 

    def wenzishibie():
        # 调用通用文字识别接口
        ws = ''
        client = AipOcr(APP_ID, API_KEY, SECRET_KEY)
        print("Working.......Please Wait..........")
        result = client.basicAccurate(get_file_content(x))
        long = result['words_result_num']
        long = int(long)
        for i in range(long):
            print(result['words_result'][i]['words'])
    
            ws += result['words_result'][i]['words']
        print("Do you need i  read it ?(Y or N):")
        ppp = input()
        if ppp == 'Y':
            voice(ws)
        else:
            print("Ok")
    

总体代码比较简洁 毕竟python特性 最重要的只有 `result = client.basicAccurate(get_file_content(x))` 这一句 这一句调用了百度的API 将照片发送至百度服务器 接收翻译结果

* * *

下面是一开始读取图片的函数

    def get_file_content(filePath):
        with open(filePath, 'rb') as fp:
            return fp.read()
    

    st=>start: 读取图片 
    op1=>operation: 将图片发送至百度服务器
    op2=>operation: 接收结果并打印在屏幕上
    e=>end: End
    st->op1->op2->e
    

之后再次基础上添加了其他的功能 原理都大同小异

* * *

\# 贴全部代码：

    from aip import AipImageClassify
    from aip import AipFace
    from aip import AipOcr
    from aip import AipImageCensor
    from aip import AipImageClassify
    from aip import AipSpeech
    import json
    import os
    import requests
    import time
    APP_ID = '10541195'
    API_KEY = 'API_KEY'
    SECRET_KEY = 'SECRET_KEY'
    
    client = AipFace(APP_ID, API_KEY, SECRET_KEY)
    # 读取图片
    
    def get_file_content(filePath):
        with open(filePath, 'rb') as fp:
            return fp.read()
    
    def biaogeshibie():
        client = AipOcr(APP_ID, API_KEY, SECRET_KEY)
        ss = client.tableRecognitionAsync(get_file_content(x))
    
        #result = json.loads(ss)
        ss = ss['result'][0]['request_id']
    
        #id = ss['request_id']
       # print (ss )
        dd = client.getTableRecognitionResult(ss, {
            'result_type': 'excel',
        })
       # print (dd)
        i = 0 
        while i<100 :
            print("Please Wait..........")
            i += 1
            time.sleep(5)
            dd = client.getTableRecognitionResult(ss, {
            'result_type': 'excel',
        })
    
            print (dd['result']['result_data'])
            print (dd['result']['ret_msg'])
            if dd['result']['ret_code'] == 2:
                print (dd['result']['percent'])
            if dd['result']['ret_code'] == 3:
                i = 300
                print("Just wait a monment i will open it for you !")
                download(dd['result']['result_data'])
                os.system('defult.xls')
    def wenzishibie():
        # 调用通用文字识别接口
        ws = ''
        client = AipOcr(APP_ID, API_KEY, SECRET_KEY)
        print("Working.......Please Wait..........")
        result = client.basicAccurate(get_file_content(x))
        long = result['words_result_num']
        long = int(long)
        for i in range(long):
            print(result['words_result'][i]['words'])
    
            ws += result['words_result'][i]['words']
        print("Do you need i  read it ?(Y or N):")
        ppp = input()
        if ppp == 'Y':
            voice(ws)
        else:
            print("Ok")
    def face():
         options = {
            'max_face_num': 1,
            'face_fields': "age,beauty,expression,faceshape",
        }
         print("Working.......Please Wait..........")
         result = client.detect(get_file_content(x), options)
         print(result)
         print("Do you need?")
         ds = input()
         if ds == 'Y':
            uid = input("uid:")
            userinfo = input("user info:")
            group = input("group:")
            facer(uid,userinfo,group)
    def yellow():
        print("Working.......Please Wait..........")
        client = AipImageCensor(APP_ID, API_KEY, SECRET_KEY)
        result = client.antiPorn(get_file_content(x))
        print(result['confidence_coefficient'])
        for i in range(3):
          print(result['result'][i]['class_name'])
          print(result['result'][i]['probability'])
        print(result['conclusion'])
    def car():
        client = AipImageClassify(APP_ID, API_KEY, SECRET_KEY)
        image = get_file_content(x)
        result = client.carDetect(image)
        print('Maximum possibility :')
        print(result['result'][0]['name'])
        print("Others :")
        print(result['result'][1]['name'])
        print(result['result'][2]['name'])
        print(result['result'][3]['name'])
    def auto():
        clients = AipImageClassify(APP_ID, API_KEY, SECRET_KEY)
        image = get_file_content(x)
        result = clients.generalDetect(image)
        print(result)
    def voice(ws):
        client = AipSpeech(APP_ID, API_KEY, SECRET_KEY)
        result  = client.synthesis(ws, 'zh', 1, {
        'vol': 5,
        })
        if not isinstance(result, dict):
            with open('auido.mp3', 'wb') as f:
                f.write(result)
            os.system('auido.mp3')
    def download(image_url):
        r = requests.get(image_url) # create HTTP response object
        with open("defult.xls",'wb') as f:
            f.write(r.content)
    def facer(uid,userinfo,group):
            result = client.addUser(  
                    uid, 
                    userinfo, 
                    group,
                    get_file_content(x)
                    )
            print(result)
    def compair(group):
    
        options = {
          'user_top_num': 1,
            'face_top_num': 1,
        }
        result = client.identifyUser(
                      group,
                      get_file_content(x),  
                      options
                    )
        print(result)
    x = input("Please input the path:")
    d = input("Please input the type(1 Text , 2 Table ,3 Face, 5 Car, 6 Voice):")
    if d == '1' :
        wenzishibie()
    elif d == '2' :
        biaogeshibie()
    elif d == '3' :
        face()
    elif d == '4' :
        yellow()
    elif d == '5' :
        car()
    elif d == '0' :
        auto()
    elif d == '6' :
        ws = x
        voice()
    elif d == 'c' :
        group = input('group:')
        compair(group)