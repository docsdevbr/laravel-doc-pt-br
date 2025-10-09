---
# Copyright (c) Taylor Otwell.
# Laravel is a trademark of Laravel Holdings Inc.

# Documentation licensed under the MIT License.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/laravel/docs/blob/-/license.md

source_url: https://github.com/laravel/docs/blob/12.x/contributions.md
revision: 24b61ecbe541e300c0922e053cdc2d3598ea011d
status: ready
---

# Guia de contribuição

- [Relatórios de falhas](#relatórios-de-falhas)
- [Perguntas de suporte](#perguntas-de-suporte)
- [Discussão sobre o desenvolvimento principal](#discussão-sobre-o-desenvolvimento-principal)
- [Qual Branch?](#qual-branch)
- [Assets compilados](#assets-compilados)
- [Vulnerabilidades de segurança](#vulnerabilidades-de-segurança)
- [Coding Style](#estilo-de-codificação)
    - [PHPDoc](#phpdoc)
    - [StyleCI](#styleci)
- [Código de conduta](#código-de-conduta)

## Relatórios de falhas

Para incentivar a colaboração ativa, o Laravel incentiva fortemente pull
requests, não apenas relatórios de falhas.
Pull requests só serão revisados quando marcados como "prontos para revisão"
(não no estado "rascunho") e todos os testes para novos recursos forem
aprovados.
Pull requests inativos e persistentes, deixados no estado "rascunho", serão
fechados após alguns dias.

No entanto, se você registrar um relatório de falha, sua issue deve conter um
título e uma descrição clara do problema.
Você também deve incluir o máximo de informações relevantes possível e um
exemplo de código que demonstre o problema.
O objetivo de um relatório de falha é facilitar para você — e para outras
pessoas — a replicação da falha e o desenvolvimento de uma correção.

Lembre-se de que os relatórios de falha são criados na esperança de que outras
pessoas com o mesmo problema possam colaborar com você na solução.
Não espere que o relatório de falha detecte automaticamente qualquer atividade
ou que outras pessoas a corrijam.
Criar um relatório de falha serve para ajudar você e outras pessoas a começarem
o caminho para a correção do problema.
Se quiser contribuir, você pode ajudar corrigindo
[quaisquer falhas listados em nossos rastreadores de issues](https://github.com/issues?q=is%3Aopen+is%3Aissue+label%3Abug+user%3Alaravel).
Você precisa estar autenticada no GitHub para visualizar todos os problemas do
Laravel.

Se você notar avisos incorretos de DocBlock, PHPStan ou IDE ao usar o Laravel,
não crie uma issue no GitHub.
Em vez disso, envie um pull request para corrigir o problema.

O código-fonte do Laravel é gerenciado no GitHub e há repositórios para cada um
dos projetos do Laravel:

<div class="content-list" markdown="1">

- [Aplicação Laravel](https://github.com/laravel/laravel)
- [Laravel Art](https://github.com/laravel/art)
- [Documentação do Laravel](https://github.com/laravel/docs)
- [Laravel Dusk](https://github.com/laravel/dusk)
- [Laravel Cashier Stripe](https://github.com/laravel/cashier)
- [Laravel Cashier Paddle](https://github.com/laravel/cashier-paddle)
- [Laravel Echo](https://github.com/laravel/echo)
- [Laravel Envoy](https://github.com/laravel/envoy)
- [Laravel Folio](https://github.com/laravel/folio)
- [Laravel Framework](https://github.com/laravel/framework)
- [Laravel Homestead](https://github.com/laravel/homestead) ([Scripts de construção](https://github.com/laravel/settler))
- [Laravel Horizon](https://github.com/laravel/horizon)
- [Laravel Passport](https://github.com/laravel/passport)
- [Laravel Pennant](https://github.com/laravel/pennant)
- [Laravel Pint](https://github.com/laravel/pint)
- [Laravel Prompts](https://github.com/laravel/prompts)
- [Laravel Reverb](https://github.com/laravel/reverb)
- [Laravel Sail](https://github.com/laravel/sail)
- [Laravel Sanctum](https://github.com/laravel/sanctum)
- [Laravel Scout](https://github.com/laravel/scout)
- [Laravel Socialite](https://github.com/laravel/socialite)
- [Laravel Telescope](https://github.com/laravel/telescope)
- [Kit para iniciantes Laravel Livewire](https://github.com/laravel/livewire-starter-kit)
- [Kit para iniciantes Laravel React](https://github.com/laravel/react-starter-kit)
- [Kit para iniciantes Laravel Vue](https://github.com/laravel/vue-starter-kit)

</div>

## Perguntas de suporte

Os rastreadores de issues do GitHub do Laravel não têm como objetivo fornecer
ajuda ou suporte ao Laravel.
Em vez disso, use um dos seguintes canais:

<div class="content-list" markdown="1">

- [Discussões do GitHub](https://github.com/laravel/framework/discussions)
- [Fóruns do Laracasts](https://laracasts.com/discuss)
- [Fóruns do Laravel.io](https://laravel.io/forum)
- [StackOverflow](https://stackoverflow.com/questions/tagged/laravel)
- [Discord](https://discord.gg/laravel)
- [Larachat](https://larachat.co)
- [IRC](https://web.libera.chat/?nick=artisan&channels=#laravel)

</div>

## Discussão sobre o desenvolvimento principal

Você pode propor novos recursos ou melhorias para comportamento existente do
Laravel no
[fórum de discussão do GitHub](https://github.com/laravel/framework/discussions)
do repositório do framework Laravel.
Se você propor um novo recurso, esteja disposta a implementar pelo menos parte
do código necessário para completá-lo.

Discussões informais sobre falhas, novos recursos e implementação de recursos
existentes ocorrem no canal `#internals` do
[servidor Discord do Laravel](https://discord.gg/laravel).
Taylor Otwell, a pessoa mantenedora do Laravel, normalmente está presente no
canal durante a semana, das 8h às 17h (UTC-06:00 ou América/Chicago), e
esporadicamente presente no canal em outros horários.

## Qual Branch?

**Todas** as correções de falhas devem ser enviadas para a versão mais recente
que suporta correções de falhas (atualmente `12.x`).
Correções de falhas **nunca** devem ser enviadas para o branch `master`, a menos
que corrijam recursos que existem apenas na próxima versão.

Recursos **menores** que são **totalmente compatíveis com versões anteriores**
da versão atual podem ser enviados para o branch estável mais recente
(atualmente `12.x`).

Novos recursos **importantes** ou recursos com alterações significativas devem
sempre ser enviados para o branch `master`, que contém a próxima versão.

## Assets compilados

Se você estiver enviando uma alteração que afetará um arquivo compilado, como a
maioria dos arquivos em `resources/css` ou `resources/js` do repositório
`laravel/laravel`, não faça o commit dos arquivos compilados.
Devido ao seu grande tamanho, eles não podem ser revisados por uma pessoa
mantenedora.
Isso pode ser explorado como uma forma de injetar código malicioso no Laravel.
Para evitar isso defensivamente, todos os arquivos compilados serão gerados e
enviados pelos mantenedores do Laravel.

## Vulnerabilidades de segurança

Se você descobrir uma vulnerabilidade de segurança no Laravel, envie um e-mail
para Taylor Otwell em
<a href="mailto:taylor@laravel.com">taylor@laravel.com</a>.
Todas as vulnerabilidades de segurança serão prontamente corrigidas.

## Estilo de codificação

O Laravel segue o padrão de codificação
[PSR-2](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md)
e o padrão de carregamento automático
[PSR-4](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader.md).

### PHPDoc

Abaixo está um exemplo de um bloco de documentação válido do Laravel.
Observe que o atributo `@param` é seguido por dois espaços, o tipo do argumento,
mais dois espaços e, por fim, o nome da variável:

```php
/**
 * Registra uma ligação com o contêiner.
 *
 * @param  string|array  $abstract
 * @param  \Closure|string|null  $concrete
 * @param  bool  $shared
 * @return void
 *
 * @throws \Exception
 */
public function bind($abstract, $concrete = null, $shared = false)
{
    // ...
}
```

Quando os atributos `@param` ou `@return` são redundantes devido ao uso de tipos
nativos, eles podem ser removidos:

```php
/**
 * Executa o trabalho.
 */
public function handle(AudioProcessor $processor): void
{
    //
}
```

No entanto, quando o tipo nativo for genérico, especifique-o por meio dos
atributos `@param` ou `@return`:

```php
/**
 * Obtém os anexos da mensagem.
 *
 * @return array<int, \Illuminate\Mail\Mailables\Attachment>
 */
public function attachments(): array
{
    return [
        Attachment::fromStorage('/caminho/para/o/arquivo'),
    ];
}
```

### StyleCI

Não se preocupe se o estilo do seu código não estiver perfeito!
O [StyleCI](https://styleci.io/) fará o merge automaticamente de quaisquer
correções de estilo no repositório Laravel após fazer o merge das pull requests.
Isso nos permite focar no conteúdo da contribuição e não no estilo do código.

## Código de conduta

O código de conduta do Laravel é derivado do código de conduta do Ruby.
Quaisquer violações do código de conduta podem ser denunciadas a Taylor Otwell
(taylor@laravel.com):

<div class="content-list" markdown="1">

- As pessoas participantes serão tolerantes com pontos de vista opostos.
- As pessoas participantes devem garantir que a sua linguagem e ações estão
  livres de ataques pessoais e comentários pessoais depreciativos.
- Ao interpretar as palavras e ações das outras pessoas, as pessoas
  participantes devem sempre assumir boas intenções.
- Comportamentos que possam ser razoavelmente considerados assédio não serão
  tolerados.

</div>
