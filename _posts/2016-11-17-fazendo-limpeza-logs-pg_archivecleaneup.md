---
layout: post
title:  "Postgresql com Backup Incremental — parte 3 — Fazendo a limpeza de logs (pg_archivecleanup)"
date:   2016-11-17 10:10:00
categories: [database]
tags: [postgresql, backup]
---

Para a limpeza dos logs antigos usaremos o comando pg_archivecleanup.

O pg_archivecleanup foi projetado para ser utilizado no archive_cleanup_command, após o recovery de um banco secundário. Mas como não estamos trabalhando com replicação, utilizamos de outra forma este comando, para facilitar criei um arquivo bash, onde verifico o último arquivo ".backup" da pasta de logs, e faço a exclusão a partir dele.

Os arquivos .backup contém os dados referentes aos processos do pg_basebackup, informando quais são os logs anteriores a execução.

O arquivo bash ficou com as seguintes linhas de comando:

```
DIR_LOGS='/var/lib/postgresql/pg_log_archive'
LAST_LOG=$(cd ${DIR_LOGS} ; ls -rt *.backup | tail -1)
/usr/lib/postgresql/9.3/bin/pg_archivecleanup "${DIR_LOGS}" "${LAST_LOG}"
```

onde,

**DIR_LOGS** é o local onde os logs (Wal) estão sendo gravados, e o **LAST_LOG** é o último arquivo .backup salvo, após o processo de pg_basebackup.

Ao executar o pg_archivecleanup, somente os logs (Wal), posteriores ao último basebackup continuam na pasta, com isso evitamos o acumulo de logs.

Em um banco onde se tem muitas transações, sem o devido cuidado com a exclusão dos logs, podemos ficar sem espaço em disco em pouco tempo, por isso é de grande importância esta rotina.