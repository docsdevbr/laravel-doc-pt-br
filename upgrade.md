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
- [Image Validation Now Excludes SVGs](#image-validation)
- [Local Filesystem Disk Default Root Path](#local-filesystem-disk-default-root-path)
- [Inspeção de banco de dados multi-esquema](#inspeção-de-banco-de-dados-multi-esquema)
- [Nested Array Request Merging](#nested-array-request-merging)

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

**Likelihood Of Impact: Medium**

The `HasUuids` trait now returns UUIDs that are compatible with version 7 of the UUID spec (ordered UUIDs). If you would like to continue using ordered UUIDv4 strings for your model's IDs, you should now use the `HasVersion4Uuids` trait:

```php
use Illuminate\Database\Eloquent\Concerns\HasUuids; // [tl! remove]
use Illuminate\Database\Eloquent\Concerns\HasVersion4Uuids as HasUuids; // [tl! add]
```

The `HasVersion7Uuids` trait has been removed. If you were previously using this trait, you should use the `HasUuids` trait instead, which now provides the same behavior.

<a name="requests"></a>
### Requests

<a name="nested-array-request-merging"></a>
#### Nested Array Request Merging

**Likelihood Of Impact: Low**

The `$request->mergeIfMissing()` method now allows merging nested array data using "dot" notation. If you were previously relying on this method to create a top-level array key containing the "dot" notation version of the key, you may need to adjust your application to account for this new behavior:

```php
$request->mergeIfMissing([
    'user.last_name' => 'Otwell',
]);
```

<a name="storage"></a>
### Storage

<a name="local-filesystem-disk-default-root-path"></a>
#### Local Filesystem Disk Default Root Path

**Likelihood Of Impact: Low**

If your application does not explicitly define a `local` disk in your filesystems configuration, Laravel will now default the local disk's root to `storage/app/private`. In previous releases, this defaulted to `storage/app`. As a result, calls to `Storage::disk('local')` will read from and write to `storage/app/private` unless otherwise configured. To restore the previous behavior, you may define the `local` disk manually and set the desired root path.

<a name="validation"></a>
### Validation

<a name="image-validation"></a>
#### Image Validation Now Excludes SVGs

**Likelihood Of Impact: Low**

The `image` validation rule no longer allows SVG images by default. If you would like to allow SVGs when using the `image` rule, you must explicitly allow them:

```php
use Illuminate\Validation\Rules\File;

'photo' => 'required|image:allow_svg'

// Or...
'photo' => ['required', File::image(allowSvg: true)],
```

<a name="miscellaneous"></a>
### Miscellaneous

We also encourage you to view the changes in the `laravel/laravel` [GitHub repository](https://github.com/laravel/laravel). While many of these changes are not required, you may wish to keep these files in sync with your application. Some of these changes will be covered in this upgrade guide, but others, such as changes to configuration files or comments, will not be. You can easily view the changes with the [GitHub comparison tool](https://github.com/laravel/laravel/compare/11.x...12.x) and choose which updates are important to you.
