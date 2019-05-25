---
layout: post
title:  "Postgresql com Backup Incremental — parte 4— Restore da base de dados"
date:   2017-03-03 10:10:00
categories: [database]
tags: [postgresql, backup]
---

Para esta etapa, devemos ter o Postgresql instalado na máquina onde o restore será feito.

Devemos copiar o arquivo **base.tar.gz** criado a partir do comando pg_basebackup no servidor principal e os arquivos incrementais que estão sendo salvos na pasta **/var/lib/postgresql/pg_log_archive**.

O aconselhável é sempre fazer a transferência destes arquivos para outro lugar, se ocorrer um problema físico na máquina principal, não será possível acessar estes arquivos e o backup não terá utilidade. Podemos utilizar scp, rsync, ou qualquer outro programa para a transferência.

Devemos parar o serviço do Postgresql da máquina onde será feito o restore.

```
/etc/init.d/postgresql stop
```

Dentro da pasta **/var/lib/pgsql** temos a pasta **data**, esta pasta é onde encontramos os dados referentes ao Postgresql, como vamos fazer o restore do banco de outra máquina, devemos renomear a pasta data para tmp (ou qualquer outro nome)

```
mv /var/lib/pgsql/data /var/lib/pgsql/tmp
```

Agora vamos descompactar o arquivo de backup feito no servidor principal. Se as versões do Linux forem as mesmas, a pasta será criada diretamente dentro de /var/lib/pgsql, se atentar onde a pasta data será criada.

```
tar xvfP /var/lib/pgsql/base.tar.gz
```

Ao listar as pastas, você verá que foi criada novamente a pasta data, onde está é a cópia do servidor principal.

Depois desta etapa devemos remover os logs antigos da pasta pg_xlog do arquivo descompactado, para evitar problemas com dados antigos

```
rm -rf /var/lib/pgsql/data/pg_xlog/*.*
```

No servidor master "Principal" temos os logs que ainda não foram salvos no arquivo incremental, estes logs estão dentro da pasta pg_xlog. Se não for possível recuperar estes logs, o restore de dados será feito somente a partir do último arquivo incremental.

Se for possível acessar o servidor principal então devemos copiar os arquivos da pasta pg_xlog, para a pasta pg_xlog do servidor onde será feito o restore.

Agora devemos criar um arquivo chamado recovery.conf dentro da pasta /var/lib/pgsql/data/, neste arquivo colocaremos a linha abaixo:

```
restore_command = 'cp /var/lib/pgsql/pg_log_archive/%f %p'
```

Onde o caminho **/var/lib/pgsql/pg_log_archive/** são os logs incrementais que buscamos do servidor principal.

Devemos também dar permissão ao postgres executar o arquivo

```
chown postgres.postgres recovery.conf
```

Após estes passos devemos iniciar o Postgresql novamente, para a importação dos logs

```
/etc/init.d/postgresql start
```

Com isso a rotina de backup e restore incremental esta concluída.