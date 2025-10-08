---
source_url: https://github.com/laravel/docs/blob/12.x/upgrade.md
revision: 6bc6cd05d1b1754de15eb73e6160384c7aaa094f
status: ready
---

# Guia de atualização

- [Atualizando da versão 11.x para a versão 12.0](#atualizando-da-versão-11.x-para-a-versão-12.0)

## Mudanças de alto impacto

<div class="content-list" markdown="1">

- [Atualizando dependências](#atualizando-dependências)
- [Atualizando o instalador do Laravel](#atualizando-o-instalador-do-laravel)

</div>

## Mudanças de médio impacto

<div class="content-list" markdown="1">

- [Modelos e UUIDv7](#modelos-e-uuidv7)

</div>

## Mudanças de baixo impacto

<div class="content-list" markdown="1">

- [Carbon 3](#carbon-3)
- [Mapeamento do índice de resultados de concorrência](#mapeamento-do-índice-de-resultados-de-concorrência)
- [Resolução de dependência de classe do contêiner](#resolução-de-dependência-de-classe-do-contêiner)
- [A validação de imagem agora exclui SVGs](#a-validação-de-imagem-agora-exclui-svgs)
- [Caminho raiz padrão do disco do sistema de arquivos local](#caminho-raiz-padrão-do-disco-do-sistema-de-arquivos-local)
- [Inspeção de banco de dados multi-esquema](#inspeção-de-banco-de-dados-multi-esquema)
- [Mesclagem de arrays aninhados da requisição](#mesclagem-de-arrays-aninhados-da-requisição)

</div>

## Atualizando da versão 11.x para a versão 12.0

#### Tempo estimado de atualização: 5 minutos

> [!NOTE]
> Tentamos documentar todas as possíveis alterações significativas.
> Como algumas dessas alterações significativas estão em partes obscuras do
> framework, apenas uma parte delas pode realmente afetar sua aplicação.
> Quer economizar tempo?
> Você pode usar o [Laravel Shift](https://laravelshift.com/) para ajudar a
> automatizar as atualizações da sua aplicação.

### Atualizando dependências

**Probabilidade de impacto: alta**

Você deve atualizar as seguintes dependências no arquivo `composer.json` da sua
aplicação:

<div class="content-list" markdown="1">

- `laravel/framework` para `^12.0`
- `phpunit/phpunit` para `^11.0`
- `pestphp/pest` para `^3.0`

</div>

#### Carbon 3

**Probabilidade de impacto: baixa**

O suporte para [Carbon 2.x](https://carbon.nesbot.com/docs/) foi removido.
Todas as aplicações Laravel 12 agora requerem
[Carbon 3.x](https://carbon.nesbot.com/docs/#api-carbon-3).

### Atualizando o instalador do Laravel

Se você estiver usando a ferramenta CLI do instalador do Laravel para criar
novas aplicações Laravel, atualize a instalação do seu instalador para que seja
compatível com o Laravel 12.x e os
[novos kits para iniciantes do Laravel](https://laravel.com/starter-kits).
Se você instalou o instalador do Laravel via `composer global require`, pode
atualizar o instalador usando `composer global update`:

```shell
composer global update laravel/installer
```

Se você instalou o PHP e o Laravel originalmente via `php.new`, basta executar
novamente os comandos de instalação do `php.new` no seu sistema operacional para
instalar a versão mais recente do PHP e do instalador do Laravel:

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

Ou, se você estiver usando a cópia do instalador que vem com o
[Laravel Herd](https://herd.laravel.com), atualize sua instalação do Herd para a
versão mais recente.

### Autenticação

#### Assinatura do construtor de `DatabaseTokenRepository` atualizada

**Probabilidade de impacto: muito baixa**

O construtor da classe `Illuminate\Auth\Passwords\DatabaseTokenRepository` agora
espera que o parâmetro `$expires` seja informado em segundos, em vez de minutos.

### Concorrência

#### Mapeamento do índice de resultados de concorrência

**Probabilidade de impacto: baixa**

Ao invocar o método `Concurrency::run` com um array associativo, os resultados
das operações concorrentes agora são retornados com suas chaves associadas:

```php
$result = Concurrency::run([
    'tarefa-1' => fn () => 1 + 1,
    'tarefa-2' => fn () => 2 + 2,
]);

// ['tarefa-1' => 2, 'tarefa-2' => 4]
```

### Contêiner

#### Resolução de dependência de classe do contêiner

**Probabilidade de impacto: baixa**

O contêiner de injeção de dependência agora respeita o valor padrão das
propriedades da classe ao resolver uma instância de classe.
Se você dependia anteriormente do contêiner para resolver uma instância de
classe sem o valor padrão, talvez seja necessário ajustar sua aplicação para
levar em conta este novo comportamento:

```php
class Example
{
    public function __construct(public ?Carbon $date = null) {}
}

$example = resolve(Example::class);

// <= 11.x
$example->date instanceof Carbon;

// >= 12.x
$example->date === null;
```

### Banco de dados

#### Inspeção de banco de dados multi-esquema

**Probabilidade de impacto: baixa**

Os métodos `Schema::getTables()`, `Schema::getViews()` e `Schema::getTypes()`
agora incluem os resultados de todos os esquemas por padrão.
Você pode passar o argumento `schema` para recuperar o resultado apenas para o
esquema fornecido:

```php
// Todas as tabelas em todos os esquemas...
$tables = Schema::getTables();

// Todas as tabelas no esquema "main"...
$tables = Schema::getTables(schema: 'main');

// Todas as tabelas nos esquemas "main" e "blog"...
$tables = Schema::getTables(schema: ['main', 'blog']);
```

O método `Schema::getTableListing()` agora retorna nomes de tabela qualificados
pelo esquema por padrão.
Você pode passar o argumento `schemaQualified` para alterar o comportamento
conforme desejado:

```php
$tables = Schema::getTableListing();
// ['main.migrations', 'main.users', 'blog.posts']

$tables = Schema::getTableListing(schema: 'main');
// ['main.migrations', 'main.users']

$tables = Schema::getTableListing(schema: 'main', schemaQualified: false);
// ['migrations', 'users']
```

Os comandos `db:table` e `db:show` agora exibem os resultados de todos os
esquemas no MySQL, MariaDB e SQLite, assim como no PostgreSQL e no SQL Server.

#### Assinatura do construtor de `Blueprint` atualizada

**Probabilidade de impacto: muito baixa**

O construtor da classe `Illuminate\Database\Schema\Blueprint` agora espera uma
instância de `Illuminate\Database\Connection` como seu primeiro argumento.

### Eloquent

#### Modelos e UUIDv7

**Probabilidade de impacto: média**

A trait `HasUuids` agora retorna UUIDs compatíveis com a versão 7 da
especificação de UUIDs (UUIDs ordenados).
Se você deseja continuar usando strings UUIDv4 ordenadas para os IDs do seu
modelo, use a trait `HasVersion4Uuids`:

```php
use Illuminate\Database\Eloquent\Concerns\HasUuids; // [tl! remove]
use Illuminate\Database\Eloquent\Concerns\HasVersion4Uuids as HasUuids; // [tl! add]
```

A trait `HasVersion7Uuids` foi removida.
Se você usava essa trait anteriormente, use a trait `HasUuids`, que agora
oferece o mesmo comportamento.

### Requisições

#### Mesclagem de arrays aninhados da requisição

**Probabilidade de impacto: baixa**

O método `$request->mergeIfMissing()` agora permite mesclar dados de arrays
aninhados usando a notação "ponto".
Se você anteriormente utilizava esse método para criar uma chave de array de
nível superior contendo a versão da chave em notação "ponto", talvez seja
necessário ajustar sua aplicação para levar em conta esse novo comportamento:

```php
$request->mergeIfMissing([
    'user.last_name' => 'Otwell',
]);
```

### Armazenamento

#### Caminho raiz padrão do disco do sistema de arquivos local

**Probabilidade de impacto: baixa**

Se sua aplicação não definir explicitamente um disco `local` na configuração do
seu sistema de arquivos, o Laravel agora definirá como padrão a raiz do disco
local como `storage/app/private`.
Em versões anteriores, o padrão era `storage/app`.
Como resultado, chamadas para `Storage::disk('local')` lerão e gravarão em
`storage/app/private`, a menos que configurado de outra forma.
Para restaurar o comportamento anterior, você pode definir o disco `local`
manualmente e definir o caminho raiz desejado.

### Validação

#### A validação de imagem agora exclui SVGs

**Probabilidade de impacto: baixa**

A regra de validação `image` não permite mais imagens SVG por padrão.
Se desejar permitir SVGs ao usar a regra `image`, você deve permiti-las
explicitamente:

```php
use Illuminate\Validation\Rules\File;

'photo' => 'required|image:allow_svg'

// Ou...
'photo' => ['required', File::image(allowSvg: true)],
```

### Diversos

Também recomendamos que você visualize as alterações no repositório
[`laravel/laravel` do GitHub](https://github.com/laravel/laravel).
Embora muitas dessas alterações não sejam obrigatórias, você pode querer manter
esses arquivos sincronizados com sua aplicação.
Algumas dessas alterações serão abordadas neste guia de atualização, mas outras,
como alterações em arquivos de configuração ou comentários, não serão.
Você pode visualizar facilmente as alterações com a
[ferramenta de comparação do GitHub](https://github.com/laravel/laravel/compare/11.x...12.x)
e escolher quais atualizações são importantes para você.
