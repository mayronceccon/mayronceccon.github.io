---
layout: post
comments: true
title:  "Postgresql com Backup Incremental — parte 2 — Fazendo backup do sistema de arquivos (pg_basebackup)"
date:   2016-11-15 10:10:00
categories: [database]
tags: [postgresql, backup]
---

Continuando os procedimentos para realizar o Backup Incremental no Postgresql, neste post faremos o backup dos dados do servidor principal, onde este backup será usado posteriormente para o restore.

No servidor principal, vamos utilizar o comando [pg_basebackup](https://www.postgresql.org/docs/9.2/app-pgbasebackup.html), onde este comando faz o backup do cluster total do servidor principal, não sendo possível fazer o backup de somente um banco específico.

Executamos o comando abaixo:

```
pg_basebackup -h127.0.0.1 -U user_replication -D backup -Ft -z -P
```

os parâmetros utilizados pode sem verificados no site do [Postgresql](https://www.postgresql.org/docs/9.2/app-pgbasebackup.html).

Após a execução e conclusão, você pode verificar que foi criada uma pasta chamada backup a partir do diretório onde você executou o comando e dentro desta o arquivo base.tar.gz.