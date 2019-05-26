---
layout: post
title:  Utilizar "environment variables" no React fazendo build com Travis
date:   2019-05-25 10:10:00
categories: [react]
tags: [build, travis, react, environment]
---

Quando estiver utilizando environment variables (.env) em seu projeto React e o build estiver sendo feito através do Travis, o arquivo .env não funcionará.

Para evitar esse problema, primeiro adicione em seu arquivo .travis.yml

```
env:
    REACT_APP_URL_API=${url_api}
```

Após isso entre em seu Travis e entre em seu repositório