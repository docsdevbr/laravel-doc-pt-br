---
# Copyright (c) Taylor Otwell.
# Laravel is a trademark of Laravel Holdings Inc.

# Documentation licensed under the MIT License.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/laravel/docs/blob/-/license.md

source_url: https://github.com/laravel/docs/blob/12.x/configuration.md
revision: a8f665a70d31e43380ead60242e1df5af2a02d47
status: ready
---

# Configuração

- [Introdução](#introdução)
- [Configuração do ambiente](#configuração-do-ambiente)
  - [Tipos de variáveis de ambiente](#tipos-de-variáveis-de-ambiente)
  - [Recuperando a configuração de ambiente](#recuperando-a-configuração-de-ambiente)
  - [Determinando o ambiente atual](#determinando-o-ambiente-atual)
  - [Criptografando arquivos de ambiente](#criptografando-arquivos-de-ambiente)
- [Acessando valores de configuração](#acessando-valores-de-configuração)
- [Cache de configuração](#cache-de-configuração)
- [Publicação de configuração](#publicação-de-configuração)
- [Modo de depuração](#modo-de-depuração)
- [Modo de manutenção](#modo-de-manutenção)

## Introdução

Todos os arquivos de configuração do framework Laravel são armazenados no
diretório `config`.
Cada opção é documentada, portanto, sinta-se à vontade para consultar os
arquivos e se familiarizar com as opções disponíveis.

Esses arquivos de configuração permitem que você configure coisas como as
informações de conexão com o banco de dados, as informações do servidor de
e-mail, bem como vários outros valores de configuração essenciais, como a URL e
a chave de criptografia da aplicação.

### O comando `about`

O Laravel pode exibir uma visão geral da configuração, dos drivers e do ambiente
da sua aplicação por meio do comando `about` do Artisan.

```shell
php artisan about
```

Se você tiver interesse em apenas uma seção específica da saída da visão geral
da aplicação, você pode filtrar essa seção usando a opção `--only`:

```shell
php artisan about --only=environment
```

Ou, para explorar os valores de um arquivo de configuração específico em
detalhes, você pode usar o comando Artisan `config:show`:

```shell
php artisan config:show database
```

## Configuração do ambiente

Geralmente, é útil ter valores de configuração diferentes com base no ambiente
em que a aplicação está sendo executada.
Por exemplo, você pode querer usar um driver de cache localmente diferente do
que usa no seu servidor de produção.

Para facilitar isso, o Laravel utiliza a biblioteca PHP
[DotEnv](https://github.com/vlucas/phpdotenv).
Em uma nova instalação do Laravel, o diretório raiz da sua aplicação conterá um
arquivo `.env.example` que define muitas variáveis de ambiente comuns.
Durante o processo de instalação do Laravel, esse arquivo será copiado
automaticamente para `.env`.

O arquivo `.env` padrão do Laravel contém alguns valores de configuração comuns
que podem diferir dependendo se a sua aplicação está sendo executada localmente
ou em um servidor web de produção.
Esses valores são então lidos pelos arquivos de configuração dentro do diretório
`config` usando a função `env` do Laravel.

Se você estiver desenvolvendo com um time, pode querer continuar incluindo e
atualizando o arquivo `.env.example` com a sua aplicação.
Ao colocar valores de espaço reservado no arquivo de configuração de exemplo,
outras pessoas desenvolvedoras do seu time podem ver claramente quais variáveis
de ambiente são necessárias para executar sua aplicação.

> [!NOTE]
> Qualquer variável no seu arquivo `.env` pode ser substituída por variáveis de
> ambiente externas, como variáveis de ambiente de nível de servidor ou de
> sistema.

#### Segurança do arquivo de ambiente

Seu arquivo `.env` não deve ser enviado para o controle de versão da sua
aplicação, pois cada pessoa desenvolvedora ou servidor que o utiliza pode exigir
uma configuração de ambiente diferente.
Além disso, isso representaria um risco à segurança caso uma pessoa invasora
obtivesse acesso ao seu repositório de controle de versão, já que quaisquer
credenciais confidenciais seriam expostas.

No entanto, é possível criptografar seu arquivo de ambiente usando a
[criptografia de ambiente](#criptografando-arquivos-de-ambiente) integrada do Laravel.
Arquivos de ambiente criptografados podem ser armazenados com segurança no
controle de origem.

#### Arquivos de ambiente adicionais

Antes de carregar as variáveis de ambiente da sua aplicação, o Laravel determina
se uma variável de ambiente `APP_ENV` foi fornecida externamente ou se o
argumento CLI `--env` foi especificado.
Nesse caso, o Laravel tentará carregar um arquivo `.env.[APP_ENV]`, se existir.
Caso contrário, o arquivo `.env` padrão será carregado.

### Tipos de variáveis de ambiente

Todas as variáveis em seus arquivos `.env` são normalmente analisadas como
strings, portanto, alguns valores reservados foram criados para permitir que
você retorne uma gama maior de tipos da função `env()`:

<div class="overflow-auto">

| Valor `.env` | Valor `env()` |
| ------------ | ------------- |
| true         | (bool) true   |
| (true)       | (bool) true   |
| false        | (bool) false  |
| (false)      | (bool) false  |
| empty        | (string) ''   |
| (empty)      | (string) ''   |
| null         | (null) null   |
| (null)       | (null) null   |

</div>

Se precisar definir uma variável de ambiente com um valor que contenha espaços,
você pode fazê-lo colocando o valor entre aspas duplas:

```ini
APP_NAME="Minha Aplicação"
```

### Recuperando a configuração de ambiente

Todas as variáveis listadas no arquivo `.env` serão carregadas na superglobal
`$_ENV` do PHP quando sua aplicação receber uma requisição.
No entanto, você pode usar a função `env` para recuperar valores dessas
variáveis em seus arquivos de configuração.
Aliás, se você revisar os arquivos de configuração do Laravel, notará que muitas
das opções já estão usando esta função:

```php
'debug' => env('APP_DEBUG', false),
```

O segundo valor passado para a função `env` é o "valor padrão".
Este valor será retornado se não existir nenhuma variável de ambiente para a
chave fornecida.

### Determinando o ambiente atual

O ambiente atual da aplicação é determinado pela variável `APP_ENV` do seu
arquivo `.env`.
Você pode acessar este valor através do método `environment` na
[fachada](facades.md) `App`:

```php
use Illuminate\Support\Facades\App;

$environment = App::environment();
```

Você também pode passar argumentos para o método `environment` para determinar
se o ambiente corresponde a um determinado valor.
O método retornará `true` se o ambiente corresponder a qualquer um dos valores
fornecidos:

```php
if (App::environment('local')) {
    // O ambiente é local
}

if (App::environment(['local', 'staging'])) {
    // O ambiente é local OU staging...
}
```

> [!NOTE]
> A detecção do ambiente atual da aplicação pode ser substituída definindo uma
> variável de ambiente `APP_ENV` em nível de servidor.

### Criptografando arquivos de ambiente

Arquivos de ambiente não criptografados nunca devem ser armazenados no controle
de versão.
No entanto, o Laravel permite criptografar seus arquivos de ambiente para que
eles possam ser adicionados com segurança ao controle de versão com o restante
da sua aplicação.

#### Criptografia

Para criptografar um arquivo de ambiente, você pode usar o comando
`env:encrypt`:

```shell
php artisan env:encrypt
```

Executar o comando `env:encrypt` criptografará seu arquivo `.env` e colocará o
conteúdo criptografado em um arquivo `.env.encrypted`.
A chave de descriptografia é apresentada na saída do comando e deve ser
armazenada em um gerenciador de senhas seguro.
Se desejar fornecer sua própria chave de criptografia, você pode usar a opção
`--key` ao invocar o comando:

```shell
php artisan env:encrypt --key=3UVsEgGVK36XN82KKeyLFMhvosbZN1aF
```

> [!NOTE]
> O comprimento da chave fornecida deve corresponder ao comprimento da chave
> exigido pela cifra de criptografia utilizada.
> Por padrão, o Laravel usará a cifra `AES-256-CBC`, que requer uma chave de 32
> caracteres.
> Você pode usar qualquer cifra suportada pelo [encriptador](encryption.md) do
> Laravel, passando a opção `--cipher` ao invocar o comando.

Se sua aplicação tiver vários arquivos de ambiente, como `.env` e
`.env.staging`, você pode especificar o arquivo de ambiente que deve ser
criptografado fornecendo o nome do ambiente por meio da opção `--env`:

```shell
php artisan env:encrypt --env=staging
```

#### Descriptografia

Para descriptografar um arquivo de ambiente, você pode usar o comando
`env:decrypt`.
Este comando requer uma chave de descriptografia, que o Laravel recuperará da
variável de ambiente `LARAVEL_ENV_ENCRYPTION_KEY`:

```shell
php artisan env:decrypt
```

Ou a chave pode ser fornecida diretamente ao comando por meio da opção `--key`:

```shell
php artisan env:decrypt --key=3UVsEgGVK36XN82KKeyLFMhvosbZN1aF
```

Quando o comando `env:decrypt` é invocado, o Laravel descriptografa o conteúdo
do arquivo `.env.encrypted` e coloca o conteúdo descriptografado no arquivo
`.env`.

A opção `--cipher` pode ser fornecida ao comando `env:decrypt` para usar uma
cifra de criptografia personalizada:

```shell
php artisan env:decrypt --key=qUWuNRdfuImXcKxZ --cipher=AES-128-CBC
```

Se sua aplicação tiver vários arquivos de ambiente, como `.env` e
`.env.staging`, você pode especificar o arquivo de ambiente que deve ser
descriptografado fornecendo o nome do ambiente por meio da opção `--env`:

```shell
php artisan env:decrypt --env=staging
```

Para substituir um arquivo de ambiente existente, você pode fornecer a opção
`--force` ao comando `env:decrypt`:

```shell
php artisan env:decrypt --force
```

## Acessando valores de configuração

Você pode acessar facilmente seus valores de configuração usando a interface
`Config` ou a função global `config` de qualquer lugar da sua aplicação.
Os valores de configuração podem ser acessados usando a sintaxe "ponto", que
inclui o nome do arquivo e a opção que você deseja acessar.
Um valor padrão também pode ser especificado e será retornado se a opção de
configuração não existir:

```php
use Illuminate\Support\Facades\Config;

$value = Config::get('app.timezone');

$value = config('app.timezone');

// Recupera um valor padrão se o valor de configuração não existir...
$value = config('app.timezone', 'Asia/Seoul');
```

Para definir valores de configuração em tempo de execução, você pode invocar o
método `set` da fachada `Config` ou passar um array para a função `config`:

```php
Config::set('app.timezone', 'America/Chicago');

config(['app.timezone' => 'America/Chicago']);
```

Para auxiliar na análise estática, a fachada `Config` também fornece métodos de
recuperação de configuração tipados.
Se o valor de configuração recuperado não corresponder ao tipo esperado, uma
exceção será lançada:

```php
Config::string('chave-da-configuracao');
Config::integer('chave-da-configuracao');
Config::float('chave-da-configuracao');
Config::boolean('chave-da-configuracao');
Config::array('chave-da-configuracao');
```

## Cache de configuração

Para aumentar a velocidade da sua aplicação, você deve armazenar em cache todos
os seus arquivos de configuração em um único arquivo usando o comando
`config:cache` do Artisan.
Isso combinará todas as opções de configuração da sua aplicação em um único
arquivo, que pode ser carregado rapidamente pelo framework.

Normalmente, você deve executar o comando `php artisan config:cache` como parte
do seu processo de implantação em produção.
O comando não deve ser executado durante o desenvolvimento local, pois as opções
de configuração precisarão ser alteradas com frequência durante o
desenvolvimento da sua aplicação.

Após o armazenamento em cache da configuração, o arquivo `.env` da sua aplicação
não será carregado pelo framework durante requisições ou comandos do Artisan;
portanto, a função `env` retornará apenas variáveis de ambiente externas, em
nível de sistema.

Por esse motivo, você deve garantir que está chamando a função `env` apenas de
dentro dos arquivos de configuração da sua aplicação (`config`).
Você pode ver muitos exemplos disso examinando os arquivos de configuração
padrão do Laravel.
Os valores de configuração podem ser acessados de qualquer lugar da sua
aplicação usando a função `config`
[descrita acima](#acessando-valores-de-configuração).

O comando `config:clear` pode ser usado para limpar a configuração armazenada em
cache:

```shell
php artisan config:clear
```

> [!WARNING]
> Se você executar o comando `config:cache` durante o processo de implantação,
> certifique-se de chamar a função `env` apenas de dentro dos seus arquivos de
> configuração.
> Após o armazenamento em cache da configuração, o arquivo `.env` não será
> carregado; portanto, a função `env` retornará apenas variáveis de ambiente
> externas, de nível de sistema.

## Publicação de configuração

A maioria dos arquivos de configuração do Laravel já está publicada no diretório
`config` da sua aplicação; no entanto, certos arquivos de configuração, como
`cors.php` e `view.php`, não são publicados por padrão, pois a maioria das
aplicações nunca precisará modificá-los.


No entanto, você pode usar o comando `config:publish` do Artisan para publicar
quaisquer arquivos de configuração que não sejam publicados por padrão:

```shell
php artisan config:publish

php artisan config:publish --all
```

## Modo de depuração

A opção `debug` no seu arquivo de configuração `config/app.php` determina quanta
informação sobre um erro é realmente exibida à pessoa usuária.
Por padrão, esta opção é definida para respeitar o valor da variável de ambiente
`APP_DEBUG`, que é armazenada no seu arquivo `.env`.

> [!WARNING]
> Para desenvolvimento local, você deve definir a variável de ambiente
> `APP_DEBUG` como `true`.
> **Em seu ambiente de produção, este valor deve ser sempre `false`.
> Se a variável for definida como `true` em produção, você corre o risco de
> expor valores de configuração sensíveis às pessoas usuárias finais da sua
> aplicação.**

## Modo de manutenção

Quando sua aplicação estiver em modo de manutenção, uma visualização
personalizada será exibida para todas as requisições dentro da sua aplicação.
Isso facilita a "desativação" da sua aplicação durante a atualização ou quando
você estiver realizando manutenção.
Uma verificação do modo de manutenção está incluída na pilha de middleware
padrão da sua aplicação.
Se a aplicação estiver em modo de manutenção, uma instância
`Symfony\Component\HttpKernel\Exception\HttpException` será lançada com o código
de status 503.

Para habilitar o modo de manutenção, execute o comando `down` do Artisan:

```shell
php artisan down
```

Se desejar que o cabeçalho HTTP `Refresh` seja enviado com todas as respostas do
modo de manutenção, você pode fornecer a opção `refresh` ao invocar o comando
`down`.
O cabeçalho `Refresh` instruirá o navegador a atualizar a página automaticamente
após o número especificado de segundos:

```shell
php artisan down --refresh=15
```

Você também pode fornecer uma opção `retry` para o comando `down`, que será
definida como o valor do cabeçalho HTTP `Retry-After`, embora os navegadores
geralmente ignorem esse cabeçalho:

```shell
php artisan down --retry=60
```

#### Ignorando o modo de manutenção

Para permitir que o modo de manutenção seja ignorado usando um token secreto,
você pode usar a opção `secret` para especificar um token de desvio do modo de
manutenção:

```shell
php artisan down --secret="1630542a-246b-4b66-afa1-dd72a4c43515"
```

Após colocar a aplicação em modo de manutenção, você pode navegar até a URL da
aplicação correspondente a esse token e o Laravel emitirá um cookie de desvio do
modo de manutenção para o seu navegador:

```shell
https://example.com/1630542a-246b-4b66-afa1-dd72a4c43515
```

Se desejar que o Laravel gere o token secreto para você, use a opção
`with-secret`.
O segredo será exibido quando a aplicação estiver em modo de manutenção:

```shell
php artisan down --with-secret
```

Ao acessar essa rota oculta, você será redirecionado para a rota `/` da
aplicação.
Assim que o cookie for emitido para o seu navegador, você poderá navegar pela
aplicação normalmente, como se ela não estivesse em modo de manutenção.

> [!NOTE]
> O segredo do seu modo de manutenção normalmente deve consistir em caracteres
> alfanuméricos e, opcionalmente, traços.
> Evite usar caracteres com significado especial em URLs, como `?` ou `&`.

#### Modo de manutenção em vários servidores

Por padrão, o Laravel determina se sua aplicação está em modo de manutenção
usando um sistema baseado em arquivos.
Isso significa que, para ativar o modo de manutenção, o comando
`php artisan down` precisa ser executado em cada servidor que hospeda sua
aplicação.

Como alternativa, o Laravel oferece um método baseado em cache para lidar com o
modo de manutenção.
Esse método requer a execução do comando `php artisan down` em apenas um
servidor.
Para usar essa abordagem, modifique as variáveis do modo de manutenção no
arquivo `.env` da sua aplicação.
Você deve selecionar um `store` de cache que seja acessível a todos os seus
servidores.
Isso garante que o status do modo de manutenção seja mantido de forma
consistente em todos os servidores:

```ini
APP_MAINTENANCE_DRIVER=cache
APP_MAINTENANCE_STORE=database
```

#### Pré-renderizando a visualização do modo de manutenção

Se você utilizar o comando `php artisan down` durante a implantação, suas
pessoas usuárias ainda poderão encontrar erros ocasionalmente ao acessar a
aplicação enquanto suas dependências do Composer ou outros componentes de
infraestrutura estiverem sendo atualizados.
Isso ocorre porque uma parte significativa do framework Laravel precisa ser
inicializada para determinar se sua aplicação está em modo de manutenção e
renderizar a visualização do modo de manutenção usando o motor de templates.

Por esse motivo, o Laravel permite que você pré-renderize uma visualização do
modo de manutenção que será retornada logo no início do ciclo de requisição.
Essa visualização é renderizada antes que qualquer dependência da sua aplicação
seja carregada.
Você pode pré-renderizar um template de sua escolha usando a opção `render` do
comando `down`:

```shell
php artisan down --render="errors::503"
```

#### Redirecionando requisições em modo de manutenção

Enquanto estiver em modo de manutenção, o Laravel exibirá a visualização do modo
de manutenção para todas as URLs da aplicação que a pessoa usuária tentar
acessar.
Se desejar, você pode instruir o Laravel a redirecionar todas as requisições
para uma URL específica.
Isso pode ser feito usando a opção `redirect`.
Por exemplo, você pode redirecionar todas as requisições para a URI `/`:

```shell
php artisan down --redirect=/
```

#### Desabilitando o modo de manutenção

Para desabilitar o modo de manutenção, use o comando `up`:

```shell
php artisan up
```

> [!NOTE]
> Você pode personalizar o template padrão do modo de manutenção definindo seu
> próprio template em `resources/views/errors/503.blade.php`.

#### Modo de manutenção e filas

Enquanto sua aplicação estiver em modo de manutenção, nenhuma
[tarefa enfileirada](queues.md) será processada.
As tarefas continuarão sendo processadas normalmente quando a aplicação sair do
modo de manutenção.

#### Alternativas ao modo de manutenção

Como o modo de manutenção exige que sua aplicação tenha vários segundos de
inatividade, considere executar suas aplicações em uma plataforma totalmente
gerenciada como a [Laravel Cloud](https://cloud.laravel.com) para realizar uma
implantação sem inatividade com o Laravel.
