---
title: 5분 만에 알아보는 AWS Lambda
date: "2020-04-26T23:50:00+09"
description: 예제와 함께하는 AWS Lambda with Python
draft: false
---

> 클라우드 서비스 덕에 서버가 없어도, 서버 관리자가 없어도 소수의 인원이 웹서비스를 만들고 운용할 수 있는 시대가 도래하였습니다. 그것이 가능한 이유 중 하나인 Serverless를 AWS Lambda 예제를 이용하여 알아보겠습니다.

## 서버리스란

서버리스는 운용하는 입장에서 서버에 대한 관리가 필요하지 않은 서비스입니다. 서버는 클라우드 서비스에서 관리해 주고, 사용자는 실행에 필요한 함수에 대해서만 관리를 하면 됩니다. 사용하지 않더라도 서버를 켜 놓은 시간에 대해서 비용이 발생하는 가상서버와는 달리, 서버리스는 함수를 실행시키는 데에 필요한 리소스에 대한 비용만을 지불하면 됩니다. 대표적인 서버리스 서비스는 AWS Lambda, GCP Functions, Azure functions가 있습니다.

서버리스를 이용하면 API를 손쉽게 만들 수 있을 뿐만 아니라, 다른 클라우드 서비스와 트리거를 설정해 놓으면, 실시간 파일 처리 등의 서비스를 구현 할 수 있습니다.

이번 포스트에서는 AWS Lambda와 파이썬을 이용하여 간단한 API를 만들어보겠습니다.

## 서버리스 프레임워크

[서버리스 프레임워크](https://serverless.com/)는 서버리스 서비스를 더욱 간편하게 이용 할 수 있게 해주는 오픈소스 프레임워크입니다. 서버리스 프레임워크를 이용하면, 서버리스에 사용될 함수의 개발, 테스트, 배포 단계를 편리하게 하실 수 있습니다.

### Node.js 설치

서버리스 프레임워크는 [Node.js](https://nodejs.org/ko/)가 설치되어있어야 하는데, 맥에서 [nvm](https://github.com/nvm-sh/nvm)을 통해 Node.js를 설치하기 위해서는 아래 명령어를 이용하면 됩니다.

```shell
# nvm 설치
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash

# Node.js LTS 버전 설치
nvm install --lts
```

### 서버리스 프레임워크 설치

서버리스 프레임워크는 아래의 명령어로 간편하게 설치하실 수 있습니다.

```shell
npm install -g serverless
```

### AWS 계정설정

서버리스 프레임워크에서 AWS 계정에 접근하기 위해서는 환경변수에 AWS credentials를 등록해야 합니다. 아래 명령어로 환경변수에 AWS credentials를 일회성으로 등록 할 수 있고, 쉘 실행 시에 자동으로 등록해주기 위해서는 아래 명령어를 ~/.bashrc 나 ~/.zshrc 파일에 넣어주면 됩니다.

```shell
export AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
export AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
export AWS_DEFAULT_REGION=us-west-2
```

## AWS Lambda와 파이썬으로 API 만들기


### 서버리스 프레임워크 템플릿
서버리스 프레임워크의 큰 장점은 템플릿이 많이 존재한다는 것입니다. 어떤 템플릿들이 존재하는지 궁금하시면 아래 명령어를 입력하시면 됩니다.

```shell
serverless create -t
```

![서버리스 템플릿](serverless_templates.png)

서버리스 프레임워크는 여러 가지 클라우드 서비스와 여러 가지 언어의 조합으로 템플릿을 제공하는데, 우리는 AWS Lambda를 python3로 이용하는 aws-python3 템플릿을 사용하여 프로젝트를 만들겠습니다.

```shell
serverless create -t aws-python3
```

![서버리스 aws-python3](serverless_aws-python3.png)

만들어진 프로젝트에는 handler.py와 serverless.yml, 그리고 .gitignore, 모두 세 개의 파일이 존재합니다. \
handler.py는 우리가 클라우드 서비스에서 실행시킬 함수가 들어있고, serverless.yml에는 서버리스 프레임워크의 설정값이 들어있습니다.

### serverless.yml 수정

serverless.yml의 기본값에는 API 관련 내용과 AWS region 설정이 빠져있습니다. 따라서 해당 내용을 추가하여 아래와 같이 수정해줍니다.

```yaml
service: serverless

provider:
  name: aws
  runtime: python3.8
  # region 추가
  region: ap-northeast-2

functions:
  hello:
    handler: handler.hello
    # API 설정 추가
    events:
      - http:
          method: get
          path: test
          cors: true
```

### hello.py 수정

hello.py를 조금 수정해서 API get method를 이용해 파라미터를 받아오는 예제를 만들어 보겠습니다.

hello.py를 아래와 같이 수정합니다.
```python
import json

def hello(event, context):
    body = {
        "message": "Go Serverless v1.0! Your function executed successfully!",
        # event 내용 중 API에서 넘겨주는 queryStringParameters 출력
        "input": event['queryStringParameters']
    }

    response = {
        "statusCode": 200,
        "body": json.dumps(body)
    }

    return response
```

### API 배포

서버리스 프레임워크의 아래 명령어로 API를 배포 할 수 있습니다.

```shell
serverless deploy
```

배포 완료 후, endpoint에 API 주소가 나타납니다. 그 주소 제일 뒤에 ?age=100을 추가하여 API를 테스트해 보겠습니다. 아래와 같은 출력이 나온다면 성공입니다. 웹브라우저나 curl을 이용하여 테스트해 보시면 됩니다.
![서버리스 결과](serverless_result.png)


축하드립니다! 서버리스를 이용하여 API를 완성하셨습니다!

## 마지막으로

서버에 대한 고민 없이, 서버리스 프레임워크의 설정값과 함수만을 이용하여 API를 만들어보았습니다. 서버리스 프레임워크를 활용하면, CPU를 이용하는 간단한 딥러닝 모델도 API 형태로 서빙을 할 수 있습니다. 다음번에는 서버리스를 이용하여 딥러닝 모델을 배포하는 방법에 대해 살펴보겠습니다.
