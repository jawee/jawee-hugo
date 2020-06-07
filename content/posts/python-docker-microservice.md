---
title: "Python Docker Microservice"
date: 2020-06-07T10:43:41+02:00
draft: false
---

When twitch changed their API to require an Oauth2 token for every call, I had to change the way I interacted with the API on our team website to show which drivers were live on twitch. 

So I wrote a small microservice in python, using flask and run through docker. 

The service takes a list from javascript that contains the usernames to check, and returns the data exactly as it's received from the twitch api. 

###### Docker-compose file
```yml
version: '3'

services:
  service:
    build: ./service
    volumes:
      - ./service:/usr/src/app
    ports:
      - 5111:5000
    environment:
      #FLASK_DEBUG: 1
      FLASK_ENV: development
      #FLASK_APP: app.py
      WORK_ENV: DEV
    command: bash -c "python microservice.py"
    restart: unless-stopped
```
###### Dockerfile
```Dockerfile
FROM python:3-slim
WORKDIR /usr/src/app

COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
```

###### Python code
```python
from flask import Flask, request, jsonify
import requests, json
from flask_cors import CORS
from dotenv import load_dotenv
import os

load_dotenv()

app = Flask(__name__)
CORS(app)

client_secret = os.getenv("CLIENT_SECRET")
client_id = os.getenv("CLIENT_ID")

def get_token():
    url = 'https://id.twitch.tv/oauth2/token?client_id=' + client_id + '&client_secret='+client_secret+'&grant_type=client_credentials'
    r = requests.post(url)
    access_token = r.json()['access_token']

    return r.json()['access_token']

@app.route('/', methods=['POST'])
def get_tasks():
    args = request.json

    query_parameters = '';
    for i in request.json['channels']:
        query_parameters += 'user_login=' + i + '&'
    token = get_token()
    url = 'https://api.twitch.tv/helix/streams?'

    full_url = url + query_parameters

    full_url = full_url[:-1]

    headers = {'Client-ID': client_id, 'Authorization': 'Bearer ' + token}
    r = requests.get(full_url, headers=headers)

    return r.json()

if __name__ == '__main__':
    app.run(host="0.0.0.0", port = 5000,debug=True)
```
