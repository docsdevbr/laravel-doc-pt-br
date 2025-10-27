---
# Copyright (c) Taylor Otwell.
# Laravel is a trademark of Laravel Holdings Inc.

# Documentation licensed under the MIT License.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/laravel/docs/blob/-/license.md

source_url: https://github.com/laravel/docs/blob/12.x/structure.md
revision: 3c0bfd38b414133db9065f2415cf264d79c13f7f
status: ready
---

# Estrutura de diretório

- [Introdução](#introdução)
- [O diretório raiz](#o-diretório-raiz)
  - [O diretório `app` raiz](#o-diretório-app-raiz)
  - [O diretório `bootstrap`](#o-diretório-bootstrap)
  - [O diretório `config`](#o-diretório-config)
  - [O diretório `database`](#o-diretório-database)
  - [O diretório `public`](#o-diretório-public)
  - [O diretório `resources`](#o-diretório-resources)
  - [O diretório `routes`](#o-diretório-routes)
  - [O diretório `storage`](#o-diretório-storage)
  - [O diretório `tests`](#o-diretório-tests)
  - [O diretório `vendor`](#o-diretório-vendor)
- [O diretório `app`](#o-diretório-app)
  - [O diretório `Broadcasting`](#o-diretório-broadcasting)
  - [O diretório `Console`](#o-diretório-console)
  - [O diretório `Events`](#o-diretório-events)
  - [O diretório `Exceptions`](#o-diretório-exceptions)
  - [O diretório `Http`](#o-diretório-http)
  - [O diretório `Jobs`](#o-diretório-jobs)
  - [O diretório `Listeners`](#o-diretório-listeners)
  - [O diretório `Mail`](#o-diretório-mail)
  - [O diretório `Models`](#o-diretório-models)
  - [O diretório `Notifications`](#o-diretório-notifications)
  - [O diretório `Policies`](#o-diretório-policies)
  - [O diretório `Providers`](#o-diretório-providers)
  - [O diretório `Rules`](#o-diretório-rules)

<a name="introdução"></a>

## Introdução

A estrutura padrão da aplicação Laravel foi projetada para fornecer um excelente
ponto de partida tanto para aplicações grandes quanto pequenas.
No entanto, você tem total liberdade para organizar sua aplicação da maneira que
preferir.
O Laravel praticamente não impõe restrições quanto à localização de qualquer
classe, desde que o Composer consiga carregá-la automaticamente.

<a name="o-diretório-raiz"></a>

## O diretório raiz

<a name="o-diretório-app-raiz"></a>

### O diretório `app` raiz

O diretório `app` contém o código principal da sua aplicação.
Exploraremos esse diretório com mais detalhes em breve; no entanto, quase todas
as classes da sua aplicação estarão neste diretório.

<a name="o-diretório-bootstrap"></a>

### O diretório `bootstrap`

O diretório `bootstrap` contém o arquivo `app.php`, que inicializa o framework.
Este diretório também abriga um diretório `cache`, que contém arquivos gerados
pelo framework para otimização de desempenho, como arquivos de cache de rotas e
serviços.

<a name="o-diretório-config"></a>

### O diretório `config`

O diretório `config`, como o nome indica, contém todos os arquivos de
configuração da sua aplicação.
É uma ótima ideia ler todos esses arquivos e se familiarizar com todas as opções
disponíveis.

<a name="o-diretório-database"></a>

### O diretório `database`

O diretório `database` contém suas migrações de banco de dados, fábricas de
modelos e sementes.
Se desejar, você também pode usar este diretório para armazenar um banco de
dados SQLite.

<a name="o-diretório-public"></a>

### O diretório `public`

O diretório `public` contém o arquivo `index.php`, que é o ponto de entrada para
todas as requisições que chegam à sua aplicação e configura o carregamento
automático.
Este diretório também abriga seus assets como imagens, JavaScript e CSS.

<a name="o-diretório-resources"></a>

### O diretório `resources`

O diretório `resources` contém suas [visualizações](/docs/{{version}}/views),
bem como seus assets brutos e não compilados, como CSS ou JavaScript.

<a name="o-diretório-routes"></a>

### O diretório `routes`

O diretório `routes` contém todas as definições de rotas da sua aplicação.
Por padrão, dois arquivos de rotas são incluídos no Laravel: `web.php` e
`console.php`.

O arquivo `web.php` contém as rotas que o Laravel coloca no grupo de middlewares
`web`, que fornecem estado de sessão, proteção contra CSRF e encriptação de
cookies.
Se a sua aplicação não oferece uma API RESTful sem estado, todas as suas rotas
provavelmente serão definidas no arquivo `web.php`.

O arquivo `console.php` é onde você pode definir todos os comandos de console
baseados em closures.
Cada closure está vinculada a uma instância de comando, permitindo uma abordagem
simples para interagir com os métodos de entrada e saída de cada comando.
Embora este arquivo não defina rotas HTTP, ele define pontos de entrada (rotas)
baseados em console para sua aplicação.
Você também pode [agendar](/docs/{{version}}/scheduling) tarefas no arquivo
`console.php`.

Opcionalmente, você pode instalar arquivos de rota adicionais para rotas de API
(`api.php`) e canais de transmissão (`channels.php`), através dos comandos
`install:api` e `install:broadcasting` do Artisan.

O arquivo `api.php` contém rotas que não devem possuir estado, portanto, as
requisições que chegam à aplicação por meio dessas rotas devem ser autenticadas
por meio de [tokens](/docs/{{version}}/sanctum) e não terão acesso ao estado da
sessão.

O arquivo `channels.php` é onde você pode registrar todos os canais de
[transmissão de eventos](/docs/{{version}}/broadcasting) que sua aplicação
suporta.

<a name="o-diretório-storage"></a>

### O diretório `storage`

O diretório `storage` contém seus logs, templates Blade compilados, sessões
baseadas em arquivos, caches de arquivos e outros arquivos gerados pelo
framework.
Este diretório é dividido nos diretórios `app`, `framework` e `logs`.
O diretório `app` pode ser usado para armazenar qualquer arquivo gerado pela sua
aplicação.
O diretório `framework` é usado para armazenar arquivos e caches gerados pelo
framework.
Finalmente, o diretório `logs` contém os arquivos de log da sua aplicação.

O diretório `storage/app/public` pode ser usado para armazenar arquivos gerados
pela pessoa usuária, como avatares de perfil, que devem ser acessíveis
publicamente.
Você deve criar um link simbólico em `public/storage` que aponte para este
diretório.
Você pode criar o link usando o comando `php artisan storage:link` do Artisan.

<a name="o-diretório-tests"></a>

### O diretório `tests`

O diretório `tests` contém seus testes automatizados.
Exemplos de testes unitários e testes de funcionalidades do
[Pest](https://pestphp.com) ou [PHPUnit](https://phpunit.de/) são fornecidos por
padrão.
Cada classe de teste deve ter o sufixo `Test`.
Você pode executar seus testes usando os comandos `/vendor/bin/pest` ou
`/vendor/bin/phpunit`.
Ou, se preferir uma representação mais detalhada e bonita dos resultados dos
seus testes, você pode executá-los usando o comando `php artisan test` do
Artisan.

<a name="o-diretório-vendor"></a>

### O diretório `vendor`

O diretório `vendor` contém suas dependências do
[Composer](https://getcomposer.org).

<a name="o-diretório-app"></a>

## O diretório `app`

A maior parte da sua aplicação está localizada no diretório `app`.
Por padrão, este diretório tem o namespace `App` e é carregado automaticamente
pelo Composer usando o
[padrão de carregamento automático PSR-4](https://www.php-fig.org/psr/psr-4/).

Por padrão, o diretório `app` contém os diretórios `Http`, `Models` e
`Providers`.
No entanto, com o tempo, vários outros diretórios serão gerados dentro do
diretório `app` à medida que você usa os comandos `make` do Artisan para gerar
classes.
Por exemplo, o diretório `app/Console` não existirá até que você execute o
comando `make:command` do Artisan para gerar uma classe de comando.

Os diretórios `Console` e `Http` são explicados em detalhes nas respectivas
seções abaixo, mas considere-os como uma API que fornece acesso ao núcleo da sua
aplicação.
O protocolo HTTP e a CLI são mecanismos para interagir com a sua aplicação, mas
não contêm a lógica da aplicação em si.
Em outras palavras, são duas maneiras de enviar comandos para a sua aplicação.
O diretório `Console` contém todos os seus comandos Artisan, enquanto o
diretório `Http` contém seus controladores, middlewares e requisições.

> [!NOTE]
> Muitas das classes no diretório `app` podem ser geradas pelo Artisan por meio
> de comandos.
> Para revisar os comandos disponíveis, execute o comando
> `php artisan list make` no seu terminal.

<a name="o-diretório-broadcasting"></a>

### O diretório `Broadcasting`

O diretório `Broadcasting` contém todas as classes de canais de transmissão da
sua aplicação.
Essas classes são geradas usando o comando `make:channel`.
Este diretório não existe por padrão, mas será criado automaticamente quando
você criar seu primeiro canal.
Para saber mais sobre canais, confira a documentação sobre
[transmissão de eventos](/docs/{{version}}/broadcasting).

<a name="o-diretório-console"></a>

### O diretório `Console`

O diretório `Console` contém todos os comandos Artisan personalizados da sua
aplicação.
Esses comandos podem ser gerados usando o comando `make:command`.

<a name="o-diretório-events"></a>

### O diretório `Events`

Este diretório não existe por padrão, mas será criado pelos comandos Artisan
`event:generate` e `make:event`.
O diretório `Events` abriga as [classes de eventos](/docs/{{version}}/events)).
Os eventos podem ser usados para alertar outras partes da sua aplicação sobre a
ocorrência de uma determinada ação, proporcionando grande flexibilidade e
desacoplamento.

<a name="o-diretório-exceptions"></a>

### O diretório `Exceptions`

O diretório `Exceptions` contém todas as exceções personalizadas da sua
aplicação.
Essas exceções podem ser geradas usando o comando `make:exception`.

<a name="o-diretório-http"></a>

### O diretório `Http`

O diretório `Http` contém seus controladores, middlewares e requisições de
formulário.
Quase toda a lógica para lidar com as requisições que chegam à sua aplicação
será colocada neste diretório.

<a name="o-diretório-jobs"></a>

### O diretório `Jobs`

Este diretório não existe por padrão, mas será criado automaticamente se você
executar o comando Artisan `make:job`.
O diretório `Jobs` abriga os [trabalhos enfileirados](/docs/{{version}}/queues)
da sua aplicação.
Os trabalhos podem ser enfileirados pela sua aplicação ou executados de forma
síncrona durante do ciclo de vida da requisição atual.
Os trabalhos executados de forma síncrona durante a requisição atual são às
vezes chamados de "comandos", pois são uma implementação do
[padrão Command](https://pt.wikipedia.org/wiki/Command).

<a name="o-diretório-listeners"></a>

### O diretório `Listeners`

Este diretório não existe por padrão, mas será criado para você se executar os
comandos Artisan `event:generate` ou `make:listener`.
O diretório `Listeners` contém as classes que lidam com seus
[eventos](/docs/{{version}}/events).
Os ouvintes de eventos recebem uma instância de evento e executam a lógica em
resposta ao evento que está sendo disparado.
Por exemplo, um evento `UserRegistered` pode ser tratado por um ouvinte
`SendWelcomeEmail`.

<a name="o-diretório-mail"></a>

### O diretório `Mail`

Este diretório não existe por padrão, mas será criado para você se executar o
comando Artisan `make:mail`.
O diretório `Mail` contém todas as suas
[classes que representam e-mails](/docs/{{version}}/mail) enviados pela sua
aplicação.
Os objetos de e-mail permitem encapsular toda a lógica de construção de um
e-mail em uma única classe simples que pode ser enviada usando o método
`Mail::send`.

<a name="o-diretório-models"></a>

### O diretório `Models`

O diretório `Models` contém todas as suas
[classes de modelo do Eloquent](/docs/{{version}}/eloquent).
O ORM Eloquent incluído no Laravel fornece uma implementação simples e elegante
do ActiveRecord para trabalhar com seu banco de dados.
Cada tabela do banco de dados possui um modelo correspondente usado para
interagir com essa tabela.
Os modelos permitem consultar dados em suas tabelas, bem como inserir novos
registros nelas.

<a name="o-diretório-notifications"></a>

### O diretório `Notifications`

Este diretório não existe por padrão, mas será criado para você se executar o
comando Artisan `make:notification`.
O diretório `Notifications` contém todas as
[notificações](/docs/{{version}}/notifications) "transacionais" enviadas pela
sua aplicação, como notificações simples sobre eventos que ocorrem dentro dela.
O recurso de notificações do Laravel abstrai o envio de notificações por meio de
diversos drivers, como e-mail, Slack, SMS ou armazenamento em um banco de dados.

<a name="o-diretório-policies"></a>

### O diretório `Policies`

Este diretório não existe por padrão, mas será criado para você se executar o
comando Artisan `make:policy`.
O diretório `Policies` contém as
[classes de política de autorização](/docs/{{version}}/authorization) da sua
aplicação.
As políticas são usadas para determinar se uma pessoa usuária pode executar uma
determinada ação em um recurso.

<a name="o-diretório-providers"></a>

### O diretório `Providers`

O diretório `Providers` contém todos os
[provedores de serviço](/docs/{{version}}/providers) da sua aplicação.
Os provedores de serviço inicializam sua aplicação vinculando serviços no
contêiner de serviço, registrando eventos ou executando outras tarefas para
preparar sua aplicação para as requisições recebidas.

Em uma aplicação Laravel nova, este diretório já conterá o `AppServiceProvider`.
Você pode adicionar seus próprios provedores a este diretório conforme
necessário.

<a name="o-diretório-rules"></a>

### O diretório `Rules`

Este diretório não existe por padrão, mas será criado para você se executar o
comando Artisan `make:rule`.
O diretório `Rules` contém os objetos de regras de validação personalizadas da
sua aplicação.
As regras são usadas para encapsular lógica de validação complexa em um objeto
simples.
Para obter mais informações, consulte a
[documentação de validação](/docs/{{version}}/validation).
