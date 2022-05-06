# Tsun-dockアプリ

## 環境構築
### 前提
1. Docker Desktopがインストールされていること

### Vueプロジェクトの作成
1. docker-compose.ymlに以下を定義

``` yml
version: "3.4"

services:
  client:
    build: ./tsundocku-client
    container_name: tsundocku-client
    ports:
      - "8080:8080"
    expose:
      - "8080"
    volumes:
      - ./tsundocku-client:/app
    tty: true
    stdin_open: true

```

2. Vueプロジェクトの作成先のディレクトリを作成

mkdir tundocku-client

3. Dockerfileを作成

``` Dockerfile
FROM node:18.1-alpine

WORKDIR /app

RUN apk update && \
    npm install -g npm && \
    npm install -g vue-cli

EXPOSE 8080

```

4. コンテナを起動

```
docker-compose up -d --buid
```

5. Vueプロジェクト作成
Vue CLIを使用して、
Vueプロジェクトを作成する。

```
docker-compose exec client sh
/app/tsun-docku # vue create .
```

以下の警告が表示されたら、記載に従ってvue/cliの最新番を取得する。

```
Vue CLI v5.0.4
? Generate project in current directory? (Y/n) Y
```

以下の警告が表示されたら、記載に従ってvue/cliの最新番を取得する。

```
  vue create is a Vue CLI 3 only command and you are using Vue CLI 2.9.6.
  You may want to run the following to upgrade to Vue CLI 3:

  npm uninstall -g vue-cli
  npm install -g @vue/cli
```

プリセットの要求が表示されたら、「Default」を選択して、Enter
```
Vue CLI v5.0.4
? Please pick a preset: (Use arrow keys)
❯ Default ([Vue 3] babel, eslint)
  Default ([Vue 2] babel, eslint)
  Manually select features


```

依存関係の要求が表示されたら、「NPM」を選択して、Enter

```
Vue CLI v5.0.4
? Please pick a preset: Default ([Vue 3] babel, eslint)
? Pick the package manager to use when installing dependencies:
  Use Yarn
❯ Use NPM
```

Vue Projectの作成が完了するのを待ちます。

6. 疎通確認
作成されたVueプロジェクトを起動します。

```
 $ npm run serve

/app # npm run serve

> app@0.1.0 serve
> vue-cli-service serve

 INFO  Starting development server...


 DONE  Compiled successfully in 111561ms                                                                                                                                                                                     10:36:52 AM


  App running at:
  - Local:   http://localhost:8080/

  It seems you are running Vue CLI inside a container.
  Access the dev server via http://localhost:<your container's external mapped port>/

  Note that the development build is not optimized.
  To create a production build, run npm run build.
```

http://localhost:8080/にアクセスし、デフォルトの画面が表示されたら、初期環境構築の完了です。

## Vue Routerの導入
今回は、SPA(Sinle Page Archtecture)による画面-コンポーネントの表現をするため、Vue Routerの導入をします。