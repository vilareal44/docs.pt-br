---
ms.openlocfilehash: ca369c4e3f78648c6e8e0bcb5d54711d3009a769
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174251"
---
### <a name="blazor-insignificant-whitespace-trimmed-from-components-at-compile-time"></a>Mais grande: espaço em branco insignificado cortado de componentes em tempo de compilação

A partir do ASP.NET Core 5,0 Preview 7, o compilador do Razor omite espaço em branco insignificante nos componentes mais importantes (arquivos *. Razor* ) no momento da compilação. Para obter uma discussão, veja Issue [dotnet/aspnetcore # 23568](https://github.com/dotnet/aspnetcore/issues/23568).

#### <a name="version-introduced"></a>Versão introduzida

5,0 Preview 7

#### <a name="old-behavior"></a>Comportamento antigo

Nas versões 3. x do mais claro servidor e Webassembly do mais claro, o espaço em branco é respeitado no código-fonte de um componente. Nós de texto somente em espaços em branco são renderizados no Modelo de Objeto do Documento do navegador (DOM) mesmo quando não há nenhum efeito visual.

Considere o seguinte código de componente mais interessante:

```razor
<ul>
    @foreach (var item in Items)
    {
        <li>
            @item.Text
        </li>
    }
</ul>
```

O exemplo anterior processa dois nós de espaço em branco:

* Fora do `@foreach` bloco de código.
* Em volta do `<li>` elemento.
* Em volta da `@item.Text` saída.

Uma lista contendo 100 itens resulta em 402 nós de espaço em branco. Isso é mais de metade de todos os nós renderizados, mesmo que nenhum dos nós de espaço em branco afete visualmente a saída renderizada.

Ao renderizar HTML estático para componentes, o espaço em branco dentro de uma marca não foi preservado. Por exemplo, exiba a origem do seguinte componente:

```razor
<foo        bar="baz"     />
```

O espaço em branco não é preservado. A saída previamente renderizada é:

```razor
<foo bar="baz" />
```

#### <a name="new-behavior"></a>Novo comportamento

A menos que a `@preservewhitespace` diretiva seja usada com o valor `true` , os nós de espaço em branco serão removidos se:

* Estão à esquerda ou à direita dentro de um elemento.
* Estão à esquerda ou à direita dentro de um `RenderFragment` parâmetro. Por exemplo, o conteúdo filho que está sendo passado para outro componente.
* Preceda ou siga um bloco de código C#, como `@if` e `@foreach` .

#### <a name="reason-for-change"></a>Motivo da alteração

Uma meta para o mais claro no ASP.NET Core 5,0 é melhorar o desempenho da renderização e da diferenciação. Nós de árvore de espaço em branco insignificantes consumidos até 40% do tempo de renderização em parâmetros de comparação.

#### <a name="recommended-action"></a>Ação recomendada

Na maioria dos casos, o layout visual do componente renderizado não é afetado. No entanto, a remoção de espaço em branco pode afetar a saída renderizada ao usar uma regra CSS como `white-space: pre` . Para desabilitar essa otimização de desempenho e preservar o espaço em branco, execute uma das seguintes ações:

* Adicione a `@preservewhitespace true` diretiva na parte superior do arquivo *. Razor* para aplicar a preferência a um componente específico.
* Adicione a `@preservewhitespace true` diretiva dentro de um arquivo *_Imports. Razor* para aplicar a preferência a um subdiretório inteiro ou todo o projeto.

Na maioria dos casos, nenhuma ação é necessária, pois os aplicativos normalmente continuarão a se comportar normalmente (mas com mais rapidez). Se a remoção de espaço em branco causar problemas para um componente específico, use esse `@preservewhitespace true` componente para desabilitar essa otimização.

#### <a name="category"></a>Categoria

ASP.NET Core

#### <a name="affected-apis"></a>APIs afetadas

Nenhum

<!--

#### Affected APIs

Not detectable via API analysis

-->
