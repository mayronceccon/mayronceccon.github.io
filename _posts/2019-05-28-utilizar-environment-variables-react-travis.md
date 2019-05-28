---
layout: post
title:  Utilizar "environment variables" no React fazendo build com Travis
date:   2019-05-28 10:10:00
categories: [react]
tags: [build, travis, react, environment]
---

Quando estiver utilizando environment variables (.env) em seu projeto React e o build estiver sendo feito através do [Travis](https://travis-ci.org), o arquivo .env não funcionará diretamente no servidor.

Para resolvermos esse problema devemos configurar em nosso Travis as "environment variables".

Primeiro devemos adicionar os dados que vamos usar no arquivo **.travis.yml**, como exemplo iremos adicionar REACT_APP_URL_API, onde, esta será a URL de consultas de nossa API.

OBS: É obrigatório que a constante tenha o prefixo **REACT_APP_**

```
env:
    REACT_APP_URL_API=${url_api}
```
Agora precisamos referenciar a variável **${url_api}** no Travis.

Devemos entre no Travis, selecionar o repositório do projeto, encontrar o menu **More options** e clicar em **Settings**.

![Settings](/assets/Selecao_738.png)

Vá até a seção **Environment Variables** e insira a mesma descrição que prencheu no parâmetro acima, no nosso caso "url_api".

![Environment Variables](/assets/Selecao_739.png)

Após isso é somente fazer o build e direcionar para o servidor.

Para utilizar em nosso projeto utilizamos **process.env.REACT_APP_URL_API**
```
import axios from 'axios';

const api = axios.create({
  baseURL: process.env.REACT_APP_URL_API
});

export default api;
```