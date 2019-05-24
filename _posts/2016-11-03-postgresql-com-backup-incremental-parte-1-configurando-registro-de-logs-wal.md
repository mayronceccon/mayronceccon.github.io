---
layout: post
title:  "Postgresql com Backup Incremental — parte 1 — Configurando o registro de Logs (Wal)"
date:   2016-11-03 10:10:00
categories: [database]
tags: [postgresql, backup]
---

{{ tags }}

Para realizar o backup incremental do Postgresql, utilizaremos o "Continuous Archiving", está técnica permite utilizar o PITR [(Point-in-Time Recovery)](https://en.wikipedia.org/wiki/Point-in-time_recovery), onde assim é possível realizar a cópia do WAL [(Write Ahead Log)](https://en.wikipedia.org/wiki/Write-ahead_logging) do PostgreSQL e utilizar estes logs para atualização de uma base de dados.

Para este tutorial estou utilizando o SO Ubuntu 14.04.

Primeiramente devemos alterar o arquivo **postgresql.conf**, adicionando os comandos abaixo:

```
wal_level = hot_standby
archive_mode = on
archive_command = 'test ! -f /var/lib/postgresql/pg_log_archive/%f && cp %p /var/lib/postgresql/pg_log_archive/%f'
max_wal_senders = 2
```

Após as modificações acima, devemos criar a pasta onde os logs serão salvos e dar permissão ao postgres para acesso da pasta.

```
mkdir /var/lib/postgresql/pg_log_archive 
chown -R postgres.postgres /var/lib/postgresql/pg_log_archive
chmod 700 /var/lib/postgresql/pg_log_archive/
```

Agora criaremos um usuário com privilégio de replicação:

```
su - postgres
psql -c "CREATE ROLE user_replication REPLICATION LOGIN PASSWORD '123';"
```

Após a criação do usuário "user_replication" devemos alterar o **pg_hba.conf**, adicionando a regra abaixo, assim, habilitando e permitindo a conexão para a replicação.

```
host replication user_replication 127.0.0.1/32 trust
```

Após as configurações, devemos fazer o restart do Postgresql.

```
/etc/init.d/postgresql restart
```

Agora seu servidor já esta configurado com o Continuous Archiving.