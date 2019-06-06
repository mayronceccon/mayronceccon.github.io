---
layout: post
comments: true
title:  "Exibindo erros do Apache no navegador com PHP"
date:   2017-03-06 10:10:00
categories: [php]
tags: [apache, errors, php]
---

Para quem não tem acesso diretamente ao servidor, algo que ajuda muito é a exibição do erros gerados pelo Apache diretamente no navegador.

Temos uma lista com 3 funções que nos ajudam nessa exibição, temos o [display_errors](https://php.net/manual/pt_BR/errorfunc.configuration.php#ini.display-errors), onde este faz a exibição dos erros de script diretamente na tela, o [display_startup_erros](https://php.net/manual/pt_BR/errorfunc.configuration.php#ini.display-startup-errors) que são os erros ocorridos na inicialização do PHP.

Além desses dois parâmetros, temos o [error_reporting](https://php.net/manual/pt_BR/errorfunc.configuration.php#ini.error-reporting) onde este define que tipo de erro será exibido.

Temos uma lista de tipos:

* **E_ALL** - Todos os erros e alertas
* **E_ERROR** - Erros fatais
* **E_WARNING** - Erros não fatais
* **E_PARSE** - Erros de compilação (antes da execução do código)
* **E_DEPRECATED** - Avisos de coisas obsoletas, que serão retiradas no futuro
* **E_NOTICE** - Avisos que podem ou não ser bugs
* **E_STRICT** - Dá recomendações de melhor interoperabilidade, desde o PHP 5.

Podemos usar 1 ou mais tipos, devemos utilizar o | (pipe) como separador.

```
E_ERROR | E_PARSE | E_NOTIVE
```

Segue exemplo da utilização dos comados de exibição dos erros, no arquivo principal de seu sistema/site adicione as linhas abaixo:

```
ini_set('display_errors', 1);
ini_set('display_startup_errors', 1);
error_reporting(E_ALL);
```