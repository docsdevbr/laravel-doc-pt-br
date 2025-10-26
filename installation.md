---
# Copyright (c) Taylor Otwell.
# Laravel is a trademark of Laravel Holdings Inc.

# Documentation licensed under the MIT License.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/laravel/docs/blob/-/license.md

source_url: https://github.com/laravel/docs/blob/12.x/installation.md
revision: 8721ef259eaf131d5859498d64a6868ee9899b7e
status: ready
---

# Instalação

- [Conheça o Laravel](#conheça-o-laravel)
  - [Por que Laravel?](#por-que-laravel)
- [Criando uma aplicação Laravel](#criando-uma-aplicação-laravel)
  - [Instalando o PHP e o instalador do Laravel](#instalando-o-php-e-o-instalador-do-laravel)
  - [Criando uma aplicação](#criando-uma-aplicação)
- [Configuração inicial](#configuração-inicial)
  - [Configuração baseada em ambiente](#configuração-baseada-em-ambiente)
  - [Bancos de dados e migrações](#bancos-de-dados-e-migrações)
  - [Configuração de diretório](#configuração-de-diretório)
- [Instalação usando o Herd](#instalação-usando-o-herd)
  - [Herd no macOS](#herd-no-macos)
  - [Herd no Windows](#herd-no-windows)
- [Suporte para IDE](#suporte-para-ide)
- [Laravel e AI](#laravel-e-ai)
  - [Instalando Laravel Boost](#instalando-laravel-boost)
- [Próximos Passos](#próximos-passos)
  - [Laravel, o framework full-stack](#laravel-o-framework-full-stack)
  - [Laravel, o Backend de API](#laravel-o-backend-de-api)

## Conheça o Laravel

Laravel é um framework para aplicações web com sintaxe expressiva e elegante.
Um framework web fornece uma estrutura e um ponto de partida para a criação da
sua aplicação, permitindo que você se concentre em criar algo incrível enquanto
nós cuidamos dos detalhes.

O Laravel se esforça para proporcionar uma experiência incrível à pessoa
desenvolvedora, ao mesmo tempo em que oferece recursos poderosos, como injeção
completa de dependência, uma camada expressiva de abstração de banco de dados,
filas e trabalhos agendados, testes unitários e de integração e muito mais.

Quer você seja iniciante em frameworks web PHP ou tenha anos de experiência, o
Laravel é um framework que pode crescer com você.
Ajudaremos você a dar seus primeiros passos como pessoa desenvolvedora web ou
lhe daremos um impulso para que você leve sua experiência para o próximo nível.
Mal podemos esperar para ver o que você construirá.

### Por que Laravel?

Há uma variedade de ferramentas e frameworks disponíveis para você criar uma
aplicação web.
No entanto, acreditamos que o Laravel é a melhor escolha para criar aplicações
web modernas e completas.

#### Um framework progressivo

Gostamos de chamar o Laravel de framework "progressivo".
Com isso, queremos dizer que o Laravel cresce com você.
Se você está apenas dando os primeiros passos no desenvolvimento web, a vasta
biblioteca de documentação, guias e [tutoriais em vídeo](https://laracasts.com)
do Laravel ajudará você a aprender o básico sem se sobrecarregar.

Se você é uma pessoa desenvolvedora sênior, o Laravel oferece ferramentas
robustas para [injeção de dependência](container.md),
[testes unitários](testing.md), [filas](queues.md),
[eventos em tempo real](broadcasting.md) e muito mais.
O Laravel é otimizado para a construção de aplicações web profissionais e está
pronto para lidar com cargas de trabalho corporativas.

#### Um framework escalável

O Laravel é incrivelmente escalável.
Graças à natureza amigável ao escalonamento do PHP e ao suporte integrado do
Laravel para sistemas de cache rápidos e distribuídos, como o Redis, o
escalonamento horizontal com o Laravel é muito fácil.
De fato, aplicações Laravel foram facilmente escaladas para lidar com centenas
de milhões de requisições por mês.

Precisa de escalonamento extremo?
Plataformas como a [Laravel Cloud](https://cloud.laravel.com) permitem que você
execute sua aplicação Laravel em escala quase ilimitada.

#### Um framework da comunidade

O Laravel combina os melhores pacotes do ecossistema PHP para oferecer o
framework mais robusto e amigável disponível para pessoas desenvolvedoras.
Além disso, milhares de pessoas desenvolvedoras talentosas de todo o mundo
[contribuíram para o framework](https://github.com/laravel/framework).
Quem sabe você até se torna uma pessoa contribuidora do Laravel.

## Criando uma aplicação Laravel

### Instalando o PHP e o instalador do Laravel

Antes de criar sua primeira aplicação Laravel, certifique-se de que sua máquina
local tenha o [PHP](https://php.net), o [Composer](https://getcomposer.org) e o
[instalador do Laravel](https://github.com/laravel/installer) instalados.
Além disso, você deve instalar o [Node e o NPM](https://nodejs.org) ou o
[Bun](https://bun.sh/) para poder compilar os assets de front-end da sua
aplicação.

Se você não tiver o PHP e o Composer instalados na sua máquina local, os
seguintes comandos instalarão o PHP, o Composer e o instalador do Laravel no
macOS, Windows ou Linux:

```shell tab=macOS
/bin/bash -c "$(curl -fsSL https://php.new/install/mac/8.4)"
```

```shell tab=Windows PowerShell
# Executar como administrador...
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://php.new/install/windows/8.4'))
```

```shell tab=Linux
/bin/bash -c "$(curl -fsSL https://php.new/install/linux/8.4)"
```

Após executar um dos comandos acima, reinicie sua sessão de terminal.
Para atualizar o PHP, o Composer e o instalador do Laravel após instalá-los via
`php.new`, você pode executar o comando novamente no seu terminal.

Se você já tem o PHP e o Composer instalados, pode instalar o instalador do
Laravel via Composer:

```shell
composer global require laravel/installer
```

> [!NOTE]
> Para uma experiência completa e gráfica de instalação e gerenciamento do PHP,
> confira [Laravel Herd](#instalação-usando-o-herd).

### Criando uma aplicação

Após instalar o PHP, o Composer e o instalador do Laravel, você estará pronta
para criar uma nova aplicação Laravel.
O instalador do Laravel solicitará que você selecione seu framework de testes,
banco de dados e kit para iniciantes preferido:

```shell
laravel new app-exemplo
```

Depois que a aplicação for criada, você pode iniciar o servidor de
desenvolvimento local do Laravel, o worker de filas e o servidor de
desenvolvimento Vite usando o script `dev` do Composer:

```shell
cd app-exemplo
npm install && npm run build
composer run dev
```

Após iniciar o servidor de desenvolvimento, sua aplicação estará acessível no
seu navegador em [http://localhost:8000](http://localhost:8000).
Em seguida, você estará pronta para
[começar a dar os próximos passos no ecossistema Laravel](#próximos-passos).
Claro, você também pode querer
[configurar um banco de dados](#bancos-de-dados-e-migrações).

> [!NOTE]
> Se você quiser uma vantagem inicial no desenvolvimento da sua aplicação
> Laravel, considere usar um dos nossos [kits para iniciantes](starter-kits.md).
> Os kits para iniciantes do Laravel fornecem uma estrutura de autenticação de
> back-end e front-end para sua nova aplicação Laravel.

## Configuração inicial

Todos os arquivos de configuração do framework Laravel são armazenados no
diretório `config`.
Cada opção está documentada, então sinta-se à vontade para examinar os arquivos
e se familiarizar com as opções disponíveis.

O Laravel praticamente não precisa de configuração adicional.
Você está livre para começar a desenvolver!
No entanto, você pode revisar o arquivo `config/app.php` e sua documentação.
Ele contém diversas opções, como `url` e `locale`, que você pode alterar de
acordo com sua aplicação.

### Configuração baseada em ambiente

Como muitos dos valores das opções de configuração do Laravel podem variar
dependendo se sua aplicação está sendo executada na sua máquina local ou em um
servidor web de produção, muitos valores de configuração importantes são
definidos usando o arquivo `.env` que existe na raiz da sua aplicação.

Seu arquivo `.env` não deve ser enviado para o controle de versão da sua
aplicação, pois cada pessoa desenvolvedora ou servidor que utiliza sua aplicação
pode exigir uma configuração de ambiente diferente.
Além disso, isso representaria um risco à segurança caso uma pessoa invasora
obtivesse acesso ao seu repositório de controle de origem, já que quaisquer
credenciais confidenciais seriam expostas.

> [!NOTE]
> Para obter mais informações sobre o arquivo `.env` e a configuração baseada em
> ambiente, consulte a
> [documentação de configuração completa](configuration.md#configuração-do-ambiente).

### Bancos de dados e migrações

Agora que você criou sua aplicação Laravel, provavelmente deseja armazenar
alguns dados em um banco de dados.
Por padrão, o arquivo de configuração `.env` da sua aplicação especifica que o
Laravel irá interagir com um banco de dados SQLite.

Durante a criação da aplicação, o Laravel criou um arquivo
`database/database.sqlite` para você e executou as migrações necessárias para
criar as tabelas de banco de dados da aplicação.

Se preferir usar outro driver de banco de dados, como MySQL ou PostgreSQL, você
pode atualizar seu arquivo de configuração `.env` para usar o banco de dados
apropriado.
Por exemplo, se desejar usar MySQL, atualize as variáveis `DB_*` do seu arquivo
de configuração `.env` da seguinte forma:

```ini
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=
```

Se você optar por usar um banco de dados diferente do SQLite, será necessário
criar o banco de dados e executar as
[migrações de banco de dados](migrations.md) da sua aplicação:

```shell
php artisan migrate
```

> [!NOTE]
> Se você estiver desenvolvendo no macOS ou Windows e precisar instalar MySQL,
> PostgreSQL ou Redis localmente, considere usar o
> [Herd Pro](https://herd.laravel.com/#plans) ou o
> [DBngin](https://dbngin.com/).

### Configuração de diretório

O Laravel deve sempre ser servido a partir da raiz do "diretório web"
configurado para o seu servidor web.
Você não deve tentar servir uma aplicação Laravel a partir de um subdiretório do
"diretório web".
Tentar fazer isso pode expor arquivos confidenciais presentes na sua aplicação.

## Instalação usando o Herd

[Laravel Herd](https://herd.laravel.com) é um ambiente de desenvolvimento nativo
e rápido em Laravel e PHP para macOS e Windows.
O Herd inclui tudo o que você precisa para começar a desenvolver com Laravel,
incluindo PHP e Nginx.

Após instalar o Herd, você estará pronta para começar a desenvolver com o
Laravel.
O Herd inclui ferramentas de linha de comando para `php`, `composer`, `laravel`,
`expose`, `node`, `npm` e `nvm`.

> [!NOTE]
> O [Herd Pro](https://herd.laravel.com/#plans) complementa o Herd com recursos
> adicionais poderosos, como a capacidade de criar e gerenciar bancos de dados
> MySQL, Postgres e Redis locais, além de visualização de e-mails e
> monitoramento de logs locais.

### Herd no macOS

Se você desenvolve no macOS, pode baixar o instalador do Herd no
[site do Herd](https://herd.laravel.com).
O instalador baixa automaticamente a versão mais recente do PHP e configura seu
Mac para sempre executar o [Nginx](https://www.nginx.com/) em segundo plano.

O Herd para macOS usa o [dnsmasq](https://en.wikipedia.org/wiki/Dnsmasq) para
oferecer suporte a diretórios "estacionados".
Qualquer aplicação Laravel em um diretório estacionado será automaticamente
atendida pelo Herd.
Por padrão, o Herd cria um diretório estacionado em `~/Herd` e você pode acessar
qualquer aplicação Laravel neste diretório no domínio `.test` usando o nome do
diretório.

Após instalar o Herd, a maneira mais rápida de criar uma nova aplicação Laravel
é usar a CLI do Laravel, que está incluída no Herd:

```shell
cd ~/Herd
laravel new minha-aplicacao
cd minha-aplicacao
herd open
```

Claro, você sempre pode gerenciar seus diretórios estacionados e outras
configurações do PHP através da interface do Herd, que pode ser aberta no menu
do Herd na bandeja do sistema.

Você pode aprender mais sobre o Herd consultando a
[documentação do Herd](https://herd.laravel.com/docs).

### Herd no Windows

Você pode baixar o instalador do Herd para Windows no
[site do Herd](https://herd.laravel.com/windows).
Após a conclusão da instalação, você pode iniciar o Herd para concluir o
processo de integração e acessar a interface do Herd pela primeira vez.

A interface do Herd pode ser acessada clicando com o botão esquerdo do mouse no
ícone do Herd na bandeja do sistema.
Um clique com o botão direito do mouse abre o menu rápido com acesso a todas as
ferramentas que você precisa no dia a dia.

Durante a instalação, o Herd cria um diretório "estacionado" no seu diretório
inicial em `%USERPROFILE%\Herd`.
Qualquer aplicação Laravel em um diretório estacionado será automaticamente
atendida pelo Herd, e você poderá acessar qualquer aplicação Laravel neste
diretório no domínio `.test` usando seu nome de diretório.

Após instalar o Herd, a maneira mais rápida de criar uma nova aplicação Laravel
é usar a CLI do Laravel, que vem com o Herd.
Para começar, abra o PowerShell e execute os seguintes comandos:

```shell
cd ~\Herd
laravel new minha-aplicacao
cd minha-aplicacao
herd open
```

Você pode aprender mais sobre o Herd consultando a
[documentação do Herd para Windows](https://herd.laravel.com/docs/windows).

## Suporte para IDE

Você pode usar qualquer editor de código que desejar ao desenvolver aplicações
Laravel.
Se você procura editores leves e extensíveis, o
[VS Code](https://code.visualstudio.com) ou o [Cursor](https://cursor.com)
combinados com a
[Extensão VS Code do Laravel](https://marketplace.visualstudio.com/items?itemName=laravel.vscode-laravel)
oficial oferecem excelente suporte ao Laravel com recursos como destaque de
sintaxe, snippets, integração de comandos Artisan e autocompletar inteligente
para modelos do Eloquent, rotas, middleware, assets, configuração e Inertia.js.

Para suporte abrangente e robusto ao Laravel, confira o
[PhpStorm](https://www.jetbrains.com/phpstorm/laravel/?utm_source=laravel.com&utm_medium=link&utm_campaign=laravel-2025&utm_content=partner&ref=laravel-2025),
um IDE da JetBrains.
Com o [plugin Laravel Idea](https://laravel-idea.com/), ele oferece suporte
preciso ao Laravel e seu ecossistema, incluindo Laravel Pint, Pest, Larastan e
muito mais.
O suporte ao framework Laravel Idea inclui templates Blade, autocompletar
inteligente para modelos do Eloquent, rotas, visualizações, traduções e
componentes, além de geração de código e navegação avançadas em projetos
Laravel.

Para quem busca uma experiência de desenvolvimento baseada em nuvem, o
[Firebase Studio](https://firebase.studio/) oferece acesso instantâneo à criação
com Laravel diretamente no seu navegador.
Sem necessidade de configuração, o Firebase Studio facilita a criação de
aplicações Laravel a partir de qualquer dispositivo.

## Laravel e IA

[Laravel Boost](https://github.com/laravel/boost) é uma ferramenta poderosa que
preenche a lacuna entre agentes de codificação de IA e aplicações Laravel.
O Boost fornece aos agentes de IA contexto, ferramentas e diretrizes específicos
do Laravel para que eles possam gerar código mais preciso e específico para cada
versão, seguindo as convenções do Laravel.

Ao instalar o Boost em sua aplicação Laravel, os agentes de IA obtêm acesso a
mais de 15 ferramentas especializadas, incluindo a capacidade de saber quais
pacotes você está usando, consultar seu banco de dados, pesquisar a documentação
do Laravel, ler logs do navegador, gerar testes e executar código via Tinker.

Além disso, o Boost oferece aos agentes de IA acesso a mais de 17.000 documentos
vetorizados do ecossistema Laravel, específicos para as versões dos pacotes
instalados.
Isso significa que os agentes podem fornecer orientações direcionadas às versões
exatas que seu projeto utiliza.

O Boost também inclui diretrizes de IA mantidas pelo Laravel que ajudam os
agentes a seguir as convenções do framework, escrever testes apropriados e
evitar armadilhas comuns ao gerar código Laravel.

### Instalando o Laravel Boost

O Boost pode ser instalado em aplicações Laravel 10, 11 e 12 executando PHP 8.1
ou superior.
Para começar, instale o Boost como uma dependência de desenvolvimento:

```shell
composer require laravel/boost --dev
```

Após a instalação, execute o instalador interativo:

```shell
php artisan boost:install
```

O instalador detectará automaticamente seus agentes de IDE e IA, permitindo que
você opte pelos recursos que fazem sentido para o seu projeto.
O Boost respeita as convenções de projeto existentes e não impõe regras de
estilo opinativas por padrão.

> [!NOTE]
> Para saber mais sobre o Boost, confira o
> [repositório do Laravel Boost no GitHub](https://github.com/laravel/boost).

## Próximos Passos

Agora que você criou sua aplicação Laravel, pode estar se perguntando o que
aprender em seguida.
Primeiramente, recomendamos fortemente que você se familiarize com o
funcionamento do Laravel lendo a seguinte documentação:

<div class="content-list" markdown="1">

- [Ciclo de vida da requisição](lifecycle.md)
- [Configuração](configuration.md)
- [Estrutura de diretório](structure.md)
- [Front-end](frontend.md)
- [Contêiner de serviços](container.md)
- [Fachadas](facades.md)

</div>

A maneira como você deseja usar o Laravel também determinará os próximos passos
em sua jornada.
Há diversas maneiras de usar o Laravel, e exploraremos dois casos de uso
principais para o framework a seguir.

### Laravel, o framework full-stack

O Laravel pode servir como um framework full-stack.
Por framework "full-stack", queremos dizer que você usará o Laravel para rotear
requisições para sua aplicação e renderizar seu front-end por meio de
[templates Blade](blade.md) ou de uma tecnologia híbrida de aplicação de página
única como o [Inertia](https://inertiajs.com).
Esta é a maneira mais comum de usar o framework Laravel e, em nossa opinião, a
maneira mais produtiva de usar o Laravel.

Se você planeja usar o Laravel dessa forma, talvez queira conferir nossa
documentação sobre [desenvolvimento frontend](frontend.md),
[roteamento](routing.md), [visualizações](views.md) ou o
[ORM Eloquent](eloquent.md).
Além disso, você pode se interessar em aprender sobre pacotes da comunidade como
[Livewire](https://livewire.laravel.com) e [Inertia](https://inertiajs.com).
Esses pacotes permitem que você use o Laravel como um framework full-stack
enquanto aproveita muitos dos benefícios de interface da pessoa usuária
oferecidos por aplicações JavaScript de página única.

Se você estiver usando o Laravel como um framework full-stack, também
recomendamos fortemente que você aprenda a compilar o CSS e o JavaScript da sua
aplicação usando [Vite](vite.md).

> [!NOTE]
> Se você quiser começar a construir sua aplicação, confira um dos nossos
> [kits de aplicações para iniciantes](starter-kits.md) oficiais.

### Laravel, o Backend de API

O Laravel também pode servir como um backend de API para uma aplicação
JavaScript de página única ou aplicativo móvel.
Por exemplo, você pode usar o Laravel como backend de API para sua aplicação
[Next.js](https://nextjs.org).
Nesse contexto, você pode usar o Laravel para fornecer autenticação e
armazenamento/recuperação de dados para sua aplicação, além de aproveitar os
poderosos serviços do Laravel, como filas, e-mails, notificações e muito mais.

Se você planeja usar o Laravel dessa forma, pode consultar nossa documentação
sobre [roteamento](routing.md), [Laravel Sanctum](sanctum.md) e
[Eloquent ORM](eloquent.md).
