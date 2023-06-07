---
title: Instrução Declare
ms.date: 07/20/2015
f1_keywords:
- vb.Declare
- vb.Lib
- vb.Any
helpviewer_keywords:
- Lib keyword [Visual Basic]
- declaring procedures [Visual Basic], Declare statement
- functions [Visual Basic], function procedures
- declarations [Visual Basic], procedures
- procedures [Visual Basic], declaration
- procedures [Visual Basic], external
- Alias keyword [Visual Basic]
- external references [Visual Basic], Visual Basic
- DLLs, declaring procedures
- Declare statement [Visual Basic]
- declarations [Visual Basic], external
- Visual Basic code, Function procedures
- As keyword [Visual Basic], in Declare statement
- resources [Visual Basic], declaring
- Public keyword [Visual Basic], Declare statement
- platform invoke, Visual Basic external references
- Sub procedures [Visual Basic], declarations
- APIs, declaring API functions
- Visual Basic code, Sub procedures
- Function procedures [Visual Basic], declaring
ms.assetid: d3f21fb0-b804-4c99-97ed-583b23894cf1
ms.openlocfilehash: 021805508a8a053ccc8fab6f1013109bece4b6f2
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404765"
---
# <a name="declare-statement"></a>Instrução Declare

Declara uma referência a um procedimento implementado em um arquivo externo.

## <a name="syntax"></a>Sintaxe

```vb
[ <attributelist> ] [ accessmodifier ] [ Shadows ] [ Overloads ] _
Declare [ charsetmodifier ] [ Sub ] name Lib "libname" _
[ Alias "aliasname" ] [ ([ parameterlist ]) ]
' -or-
[ <attributelist> ] [ accessmodifier ] [ Shadows ] [ Overloads ] _
Declare [ charsetmodifier ] [ Function ] name Lib "libname" _
[ Alias "aliasname" ] [ ([ parameterlist ]) ] [ As returntype ]
```

## <a name="parts"></a>Partes

|Termo|Definição|
|---|---|
|`attributelist`|Opcional. Consulte a [lista de atributos](attribute-list.md).|
|`accessmodifier`|Opcional. Pode ser um dos seguintes:<br /><br /> -   [Publicada](../modifiers/public.md)<br />-   [Protected](../modifiers/protected.md)<br />-   [Público](../modifiers/friend.md)<br />-   [Pessoal](../modifiers/private.md)<br />- [Amigo protegido](../modifiers/protected-friend.md)<br />- [Particular protegido](../modifiers/private-protected.md)<br /><br /> Consulte [níveis de acesso em Visual Basic](../../programming-guide/language-features/declared-elements/access-levels.md).|
|`Shadows`|Opcional. Consulte [Shadows](../modifiers/shadows.md).|
|`charsetmodifier`|Opcional. Especifica o conjunto de caracteres e as informações de pesquisa de arquivo. Pode ser um dos seguintes:<br /><br /> -   [ANSI](../modifiers/ansi.md) (padrão)<br />-   [Unicode](../modifiers/unicode.md)<br />-   [Automático](../modifiers/auto.md)|
|`Sub`|Opcional, mas `Sub` ou `Function` deve aparecer. Indica que o procedimento externo não retorna um valor.|
|`Function`|Opcional, mas `Sub` ou `Function` deve aparecer. Indica que o procedimento externo retorna um valor.|
|`name`|Obrigatórios. Nome desta referência externa. Para obter mais informações, consulte [nomes de elementos declarados](../../programming-guide/language-features/declared-elements/declared-element-names.md).|
|`Lib`|Obrigatórios. Apresenta uma `Lib` cláusula, que identifica o arquivo externo (DLL ou recurso de código) que contém um procedimento externo.|
|`libname`|Obrigatórios. Nome do arquivo que contém o procedimento declarado.|
|`Alias`|Opcional. Indica que o procedimento que está sendo declarado não pode ser identificado em seu arquivo pelo nome especificado em `name` . Você especifica sua identificação no `aliasname` .|
|`aliasname`|Necessário se você usar a `Alias` palavra-chave. Cadeia de caracteres que identifica o procedimento de uma das duas maneiras:<br /><br /> O nome do ponto de entrada do procedimento dentro de seu arquivo, entre aspas ( `""` )<br /><br /> -ou-<br /><br /> Um sinal numérico ( `#` ) seguido por um inteiro especificando o número ordinal do ponto de entrada do procedimento dentro de seu arquivo|
|`parameterlist`|Obrigatório se o procedimento usa parâmetros. Consulte a [lista de parâmetros](parameter-list.md).|
|`returntype`|Obrigatório se `Function` for especificado e `Option Strict` for `On` . Tipo de dados do valor retornado pelo procedimento.|

## <a name="remarks"></a>Comentários

Às vezes, você precisa chamar um procedimento definido em um arquivo (como uma DLL ou recurso de código) fora do seu projeto. Quando você faz isso, o compilador Visual Basic não tem acesso às informações necessárias para chamar o procedimento corretamente, como onde o procedimento está localizado, como ele é identificado, sua sequência de chamada e tipo de retorno e o conjunto de caracteres da cadeia de caracteres que ele usa. A `Declare` instrução cria uma referência a um procedimento externo e fornece essas informações necessárias.

Você pode usar `Declare` somente no nível do módulo. Isso significa que o *contexto de declaração* para uma referência externa deve ser uma classe, estrutura ou módulo e não pode ser um arquivo de origem, namespace, interface, procedimento ou bloco. Para obter mais informações, consulte [Contextos de declaração e níveis de acesso padrão](declaration-contexts-and-default-access-levels.md).

Referências externas padrão para acesso [público](../modifiers/public.md) . Você pode ajustar seus níveis de acesso com os modificadores de acesso.

## <a name="rules"></a>Regras

- **Atributos.** Você pode aplicar atributos a uma referência externa. Qualquer atributo que você aplicar tem efeito somente em seu projeto, não no arquivo externo.

- **Modificadores.** Os procedimentos externos são [compartilhados](../modifiers/shared.md)implicitamente. Você não pode usar a `Shared` palavra-chave ao declarar uma referência externa e não pode alterar seu status compartilhado.

  Um procedimento externo não pode participar da substituição, implementar membros de interface ou manipular eventos. Da mesma forma, você não pode usar a `Overrides` `Overridable` `NotOverridable` palavra-chave,,, `MustOverride` `Implements` , ou `Handles` em uma `Declare` instrução.

- **Nome do procedimento externo.** Você não precisa dar a essa referência externa o mesmo nome (in `name` ) que o nome do ponto de entrada do procedimento em seu arquivo externo ( `aliasname` ). Você pode usar uma `Alias` cláusula para especificar o nome do ponto de entrada. Isso pode ser útil se o procedimento externo tiver o mesmo nome que um modificador Visual Basic reservado ou uma variável, um procedimento ou qualquer outro elemento de programação no mesmo escopo.

  > [!NOTE]
  > Os nomes de ponto de entrada na maioria das DLLs diferenciam maiúsculas de minúsculas.

- **Número do procedimento externo.** Como alternativa, você pode usar uma `Alias` cláusula para especificar o número ordinal do ponto de entrada dentro da tabela de exportação do arquivo externo. Para fazer isso, comece `aliasname` com um sinal numérico ( `#` ). Isso pode ser útil se qualquer caractere no nome do procedimento externo não for permitido em Visual Basic, ou se o arquivo externo exportar o procedimento sem um nome.

## <a name="data-type-rules"></a>Regras de tipo de dados

- **Tipos de dados de parâmetro.** Se `Option Strict` for `On` , você deverá especificar o tipo de dados de cada parâmetro no `parameterlist` . Pode ser qualquer tipo de dados ou o nome de uma enumeração, estrutura, classe ou interface. No `parameterlist` , você usa uma `As` cláusula para especificar o tipo de dados do argumento a ser passado para cada parâmetro.

  > [!NOTE]
  > Se o procedimento externo não foi gravado para o .NET Framework, você deve ter cuidado com os tipos de dados correspondentes. Por exemplo, se você declarar uma referência externa a um procedimento Visual Basic 6,0 com um `Integer` parâmetro (16 bits em Visual Basic 6,0), você deverá identificar o argumento correspondente como `Short` na `Declare` instrução, pois esse é o tipo de inteiro de 16 bits em Visual Basic. Da mesma forma, `Long` tem uma largura de dados diferente no Visual Basic 6,0 e `Date` é implementada de forma diferente.

- **Tipo de dados de retorno.** Se o procedimento externo for a `Function` e `Option Strict` for `On` , você deverá especificar o tipo de dados do valor retornado para o código de chamada. Pode ser qualquer tipo de dados ou o nome de uma enumeração, estrutura, classe ou interface.

  > [!NOTE]
  > O compilador Visual Basic não verifica se os tipos de dados são compatíveis com os do procedimento externo. Se houver uma incompatibilidade, o Common Language Runtime gerará uma <xref:System.Runtime.InteropServices.MarshalDirectiveException> exceção em tempo de execução.

- **Tipos de dados padrão.** Se `Option Strict` for `Off` e você não especificar o tipo de dados de um parâmetro no `parameterlist` , o compilador Visual Basic converterá o argumento correspondente para o [tipo de dados Object](../data-types/object-data-type.md). Da mesma forma, se você não especificar `returntype` , o compilador usará o tipo de dados de retorno como `Object` .

  > [!NOTE]
  > Como você está lidando com um procedimento externo que pode ter sido escrito em uma plataforma diferente, é perigoso fazer qualquer suposição sobre tipos de dados ou permitir que eles sejam padrão. É muito mais seguro especificar o tipo de dados de cada parâmetro e do valor de retorno, se houver. Isso também melhora a legibilidade do seu código.

## <a name="behavior"></a>Comportamento

- **Com.** Uma referência externa está no escopo em toda a classe, estrutura ou módulo.

- **Existência.** Uma referência externa tem o mesmo tempo de vida da classe, estrutura ou módulo no qual ela é declarada.

- **Chamando um procedimento externo.** Você chama um procedimento externo da mesma maneira que chama um `Function` `Sub` procedimento ou — usando-o em uma expressão, caso ele retorne um valor, ou especificando-o em uma [instrução de chamada](call-statement.md) se ele não retornar um valor.

  Você passa argumentos para o procedimento externo exatamente como especificado por `parameterlist` na `Declare` instrução. Não leve em consideração como os parâmetros foram originalmente declarados no arquivo externo. Da mesma forma, se houver um valor de retorno, use-o exatamente como especificado por `returntype` na `Declare` instrução.

- **Conjuntos de caracteres.** Você pode especificar em `charsetmodifier` como Visual Basic deve realizar marshaling de cadeias de caracteres ao chamar o procedimento externo. O `Ansi` modificador direciona Visual Basic para empacotar todas as cadeias de caracteres para valores ANSI e o `Unicode` modificador o direciona para empacotar todas as cadeias de caracteres para valores Unicode. O `Auto` modificador direciona Visual Basic para empacotar cadeias de caracteres de acordo com as regras de .NET Framework com base na referência externa `name` ou, `aliasname` se especificado. O valor padrão é `Ansi`.

  `charsetmodifier`também especifica como Visual Basic deve procurar o procedimento externo dentro de seu arquivo externo. `Ansi`e `Unicode` os dois Visual Basic diretos para pesquisá-lo sem modificar seu nome durante a pesquisa. `Auto`direciona Visual Basic para determinar o conjunto de caracteres base da plataforma de tempo de execução e possivelmente modificar o nome do procedimento externo, da seguinte maneira:

  - Em uma plataforma ANSI, como o Windows 95, o Windows 98 ou o Windows Millennium Edition, primeiro procure o procedimento externo sem nenhuma modificação de nome. Se isso falhar, acrescente "A" ao final do nome do procedimento externo e procure novamente.

  - Em uma plataforma Unicode, como o Windows NT, o Windows 2000 ou o Windows XP, primeiro procure o procedimento externo sem nenhuma modificação de nome. Se isso falhar, acrescente "W" ao final do nome do procedimento externo e procure novamente.

- **Mecanismo.** Visual Basic usa o mecanismo de *invocação da plataforma* .NET Framework (PInvoke) para resolver e acessar procedimentos externos. A `Declare` instrução e a <xref:System.Runtime.InteropServices.DllImportAttribute> classe usam esse mecanismo automaticamente e você não precisa de nenhum conhecimento do PInvoke. Para obter mais informações, consulte [Walkthrough: chamando APIs do Windows](../../programming-guide/com-interop/walkthrough-calling-windows-apis.md).

> [!IMPORTANT]
> Se o procedimento externo for executado fora do Common Language Runtime (CLR), ele será um *código não gerenciado*. Quando você chama esse procedimento, por exemplo, uma função de API do Windows ou um método COM, você pode expor seu aplicativo a riscos de segurança. Para obter mais informações, consulte [proteger as diretrizes de codificação para código não gerenciado](https://docs.microsoft.com/previous-versions/dotnet/framework/security/secure-coding-guidelines-for-unmanaged-code).

## <a name="example"></a>Exemplo

O exemplo a seguir declara uma referência externa a um `Function` procedimento que retorna o nome de usuário atual. Em seguida, ele chama o procedimento externo `GetUserNameA` como parte do `getUser` procedimento.

[!code-vb[VbVbalrStatements#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#15)]

## <a name="example"></a>Exemplo

O <xref:System.Runtime.InteropServices.DllImportAttribute> fornece uma maneira alternativa de usar funções em código não gerenciado. O exemplo a seguir declara uma função importada sem usar uma `Declare` instrução.

[!code-vb[VbVbalrStatements#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#16)]

[!code-vb[VbVbalrStatements#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#1)]

## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.ErrObject.LastDllError%2A>
- [Instrução Imports (tipo e namespace .NET)](imports-statement-net-namespace-and-type.md)
- [Operador AddressOf](../operators/addressof-operator.md)
- [Instrução Function](function-statement.md)
- [Instrução Sub](sub-statement.md)
- [Lista de parâmetros](parameter-list.md)
- [Instrução Call](call-statement.md)
- [Passo a passo: Fazer chamadas de APIs do Windows](../../programming-guide/com-interop/walkthrough-calling-windows-apis.md)
