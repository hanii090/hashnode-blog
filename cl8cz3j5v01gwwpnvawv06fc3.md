---
title: "Facebook ChatBot Using Flask"
seoTitle: "Facebook ChatBot Using Flask"
datePublished: Thu Sep 22 2022 11:30:50 GMT+0000 (Coordinated Universal Time)
cuid: cl8cz3j5v01gwwpnvawv06fc3
slug: facebook-chatbot-using-flask
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1663846001730/5TjTAnweV.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1663846121292/TICTLMLZz.png
tags: python, facebook, flask, python-beginner, python-projects

---

## Set up 

first of all go here and  login with your fb account
Also Install ngrok   zip file

```
https://developers.facebook.com/
```

```
https://ngrok.com/download
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/yoa4iwuo5qhu1wkv8ee5.png)
Fill the instructions According to you
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/dmwujwvzodm5c0phh2ad.png)

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/78e5kzh0oo4pgc2vaxm3.png)
That's it Now We Do Some code
create a folder and using pip install  these  Modules

```
pip install pymessenger

pip install requests

pip install flask
```
Open your Folder And make  a file name 
fbBot.py

```
from flask import Flask,request
import requests
from pymessenger import Bot

app =  Flask(__name__)

VERIFY_TOKEN = ''
PAGE_ACCESS_TOKEN =''
bot = Bot(PAGE_ACCESS_TOKEN)


def handling_message(text):
    adjusted_msg = text
    if adjusted_msg == 'hi' or adjusted_msg == 'Hi':
        response = 'hey'
    elif adjusted_msg == "what's up" or adjusted_msg == "what's up":
        response = "i'm Great"
    else:
        response = "it's pleasure to talk with you,Thank you."
    return response

@app.route('/', methods = ["POST","GET"])

def web_hook():
    if request.method == "GET":
        if request.args.get('hub.verify.token') == VERIFY_TOKEN:
            return request.args.get('hub.challenge')

        else:
                return 'Unable To Connect Meta'
    elif request.method == 'POST':
        data = request.json
        process = data['entry'][0]['messaging']
        for msg in process:
            text = msg['message']['text']
            sender_id = msg['sender']['id']
            response = handling_message(text)
            bot.send_text_message(sender_id,response)
        return 'Message Delivered'

    else:
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8q5t43y3zd7kvdzdpfdp.png)


if  __name__ == '__main__':
    app.run()

```
name anything about verify token
Generate page access token and paste above
 
Now Extract ngrok and paste it in your Workspace 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/aa5osciwhtq8gjh1n6w0.png)

run fbBot.py 
and ngrok As An Administrator
put this cmd
and Hit Enter

```
ngrok.exe http 5000
```

now you are connected with it 
from here copy https that end with .io
and paste it here with token name and save

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ad0o2ecnn279vfvqaufj.png)
Restart your app Build again And You Are Good To Go
