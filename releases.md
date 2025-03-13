---
source_url: https://github.com/laravel/docs/blob/12.x/releases.md
revision: 7589d0d3515172587ff625a48969afe6cb8056ae
status: ready
---

# Notas de Versão

## Esquema de Versionamento

O Laravel e seus outros pacotes originais seguem o
[Versionamento Semântico](https://semver.org/lang/pt-BR/).
As versões maiores do _framework_ são lançadas todos os anos (por volta do 1º
trimestre), enquanto as versões menores e de correções podem ser lançadas até
semanalmente.
Versões menores e de correções **nunca** devem conter alterações significativas.

Ao referenciar o _framework_ Laravel ou seus componentes a partir da sua
aplicação ou pacote, você deve sempre usar uma restrição de versão como `^12.0`,
uma vez que as versões maiores do Laravel incluem alterações significativas.
No entanto, nos esforçamos para sempre garantir que você possa atualizar para
uma nova versão maior em um dia ou menos.

#### Argumentos Nomeados

[Argumentos nomeados](https://www.php.net/manual/en/functions.arguments.php#functions.named-arguments)
não são cobertos pelas diretrizes de compatibilidade com versões anteriores do
Laravel.
Podemos optar por renomear os argumentos de funções quando necessário para
melhorar a base de código do Laravel.
Portanto, o uso de argumentos nomeados ao chamar métodos do Laravel deve ser
feito com cautela e com o entendimento de que os nomes dos parâmetros podem
mudar no futuro.

## Política de Suporte

Para todas as versões do Laravel, as correções de falhas são fornecidas por 18
meses e as correções de segurança são fornecidas por 2 anos.
Para todas as bibliotecas adicionais, incluindo Lumen, apenas a versão maior
mais recente recebe correções de falhas.
Além disso, revise as versões do banco de dados
[suportadas pelo Laravel](database.md#introduction).

<div class="overflow-auto">

|       Versão        |  PHP (*)  |       Lançamento        | Correções de falhas até | Correções de segurança até |
|:-------------------:|:---------:|:-----------------------:|:-----------------------:|:--------------------------:|
| 9 {: .bg-red-500 }  | 8.0 - 8.2 | 8 de fevereiro de 2022  |   8 de agosto de 2023   |   6 de fevereiro de 2024   |
| 10 {: .bg-red-500 } | 8.1 - 8.3 | 14 de fevereiro de 2023 |   6 de agosto de 2024   |   4 de fevereiro de 2025   |
|         11          | 8.2 - 8.4 |   12 de março de 2024   |  3 de setembro de 2025  |    12 de março de 2026     |
|         12          | 8.2 - 8.4 | 24 de fevereiro de 2025 |   13 de agosto de 2026  |  24 de fevereiro de 2027   |

</div>

<div class="version-colors">
    <div class="end-of-life">
        <div class="bg-red-500 color-box"></div>
        <div>Fim de vida</div>
    </div>
    <div class="security-fixes">
        <div class="color-box"></div>
        <div>Apenas correções de segurança</div>
    </div>
</div>

(*) Versões suportadas do PHP

## Laravel 12

O Laravel 12 continua as melhorias feitas no Laravel 11.x atualizando
dependências _upstream_ e introduzindo novos _kits_ para iniciantes React, Vue e
Livewire, incluindo a opção de usar o [WorkOS AuthKit](https://authkit.com) para
autenticação da pessoa usuária.
A variante WorkOS dos nossos _kits_ para iniciantes oferece autenticação social,
chaves de acesso e suporte a SSO.

### Alterações Significativas Mínimas

Grande parte do nosso foco durante este ciclo de lançamento tem sido minimizar
alterações significativas.
Em vez disso, nos dedicamos a distribuir melhorias contínuas de qualidade de
vida ao longo do ano que não quebram as aplicações existentes.

Portanto, o lançamento do Laravel 12 é um "lançamento de manutenção"
relativamente menor para atualizar as dependências existentes.
Diante disso, a maioria das aplicações Laravel pode ser atualizada para o
Laravel 12 sem alterar nenhum código da aplicação.

### Novos _Kits_ de Aplicações para Iniciantes

O Laravel 12 apresenta novos
[_kits_ de aplicações para iniciantes](starter-kits.md) React, Vue e Livewire.
Os _kits_ para iniciantes React e Vue utilizam Inertia 2, TypeScript,
[shadcn/ui](https://ui.shadcn.com) e Tailwind, enquanto os _kits_ para
iniciantes Livewire utilizam a biblioteca de componentes
[Flux UI](https://fluxui.dev) baseada no Tailwind e no Laravel Volt.

Os _kits_ para iniciantes React, Vue e Livewire utilizam o sistema de
autenticação integrado do Laravel para oferecer _login_, registro, redefinição
de senha, verificação de e-mail e muito mais.
Além disso, estamos introduzindo uma variante do
[WorkOS AuthKit](https://authkit.com) de cada _kit_ para iniciantes, oferecendo
autenticação social, chaves de acesso e suporte a SSO.
O WorkOS oferece autenticação gratuita para aplicações de até 1 milhão de
pessoas usuárias ativas mensais.

Com a introdução de nossos novos _kits_ de aplicações para iniciantes, o Laravel
Breeze e o Laravel Jetstream não receberão mais atualizações adicionais.

Para começar a usar nossos novos _kits_ para iniciantes, confira a
[documentação dos _kits_ para iniciantes](starter-kits.md).
