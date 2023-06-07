---
title: Melhorando a depuração com os atributos de exibição do depurador
description: Introdução aos atributos de exibição do depurador no .NET, que permitem que o desenvolvedor do tipo também especifique qual será a aparência do tipo quando ele é mostrado em um depurador.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- debugger, display attributes
- DebuggerTypeProxyAttribute attribute
- debugging [.NET Framework], debugger display attributes
- DebuggerDisplayAttribute attribute
- display attributes for debugger
- DebuggerBrowsableAttribute attribute
ms.assetid: 72bb7aa9-459b-42c4-9163-9312fab4c410
ms.openlocfilehash: f266bf7278f472c51dd355df5ba04a123cbd7df0
ms.sourcegitcommit: a2c8b19e813a52b91facbb5d7e3c062c7188b457
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85415960"
---
# <a name="enhancing-debugging-with-the-debugger-display-attributes"></a>Melhorando a depuração com os atributos de exibição do depurador

Os atributos de exibição do depurador permitem ao desenvolvedor do tipo, que especifica e entende melhor o comportamento de runtime desse tipo, especificar também a aparência desse tipo quando ele é exibido em um depurador. Além disso, os atributos de exibição do depurador que fornecem uma propriedade `Target` podem ser aplicados no nível do assembly pelos usuários sem conhecimento do código-fonte. O atributo <xref:System.Diagnostics.DebuggerDisplayAttribute> controla como um tipo ou um membro é exibido nas janelas de variáveis do depurador. O atributo <xref:System.Diagnostics.DebuggerBrowsableAttribute> determina se e como um campo ou uma propriedade é exibida nas janelas de variáveis do depurador. O atributo <xref:System.Diagnostics.DebuggerTypeProxyAttribute> especifica um tipo substituto ou um proxy, para um tipo e altera o modo como o tipo é exibido nas janelas do depurador. Quando você exibe uma variável que tem um proxy ou um tipo substituto, o proxy substitui o tipo original na janela de exibição do depurador. A janela de variáveis do depurador exibe apenas os membros públicos do tipo de proxy. Os membros particulares não são exibidos.  
  
## <a name="using-the-debuggerdisplayattribute"></a>Usando o DebuggerDisplayAttribute  

O construtor <xref:System.Diagnostics.DebuggerDisplayAttribute.%23ctor%2A> tem um único argumento: uma cadeia de caracteres a ser exibida na coluna de valor das instâncias do tipo. Essa cadeia de caracteres pode conter chaves ({ e }). O texto dentro de um par de chaves é avaliado como uma expressão. Por exemplo, o código C# a seguir faz com que “Contagem = 4” seja exibida quando o sinal de adição (+) é selecionado para expandir a exibição do depurador para uma instância de `MyHashtable`.  

```csharp
[DebuggerDisplay("Count = {count}")]
class MyHashtable
{
    public int count = 4;
}
```

Os atributos aplicados às propriedades referenciadas na expressão não são processados. Para o compilador C#, uma expressão geral é permitida que tem somente acesso implícito a essa referência na instância atual do tipo de destino. A expressão é limitada; não há nenhum acesso a aliases, locais ou ponteiros. No código C#, você pode usar uma expressão geral entre as chaves que tem acesso implícito ao ponteiro `this` na instância atual somente do tipo de destino.

Por exemplo, se um objeto do C# tiver um `ToString()` substituído, o depurador chamará a substituição e mostrará seu resultado, em vez do `{<typeName>}.` padrão. Portanto, se você tiver substituído `ToString()`, não precisará usar <xref:System.Diagnostics.DebuggerDisplayAttribute>. Se você usar ambas, o atributo <xref:System.Diagnostics.DebuggerDisplayAttribute> terá precedência sobre a substituição de `ToString()`.

## <a name="using-the-debuggerbrowsableattribute"></a>Usando o DebuggerBrowsableAttribute
 Aplique o <xref:System.Diagnostics.DebuggerBrowsableAttribute> a um campo ou uma propriedade para especificar como o campo ou a propriedade deve ser exibida na janela do depurador. O construtor desse atributo usa um dos valores de enumeração <xref:System.Diagnostics.DebuggerBrowsableState>, que especifica um dos seguintes estados:

- <xref:System.Diagnostics.DebuggerBrowsableState.Never> indica que o membro não é exibido na janela de dados.  Por exemplo, o uso desse valor para o <xref:System.Diagnostics.DebuggerBrowsableAttribute> em um campo remove o campo da hierarquia; o campo não é exibido quando você expande o tipo delimitador clicando no sinal de adição (+) da instância de tipo.

- <xref:System.Diagnostics.DebuggerBrowsableState.Collapsed> indica que o membro é exibido, mas não expandido por padrão.  Esse é o comportamento padrão.

- <xref:System.Diagnostics.DebuggerBrowsableState.RootHidden> indica que o próprio membro não é mostrado, mas seus objetos constituintes são exibidos, se ele é uma matriz ou uma coleção.

> [!NOTE]
> Não há suporte para o <xref:System.Diagnostics.DebuggerBrowsableAttribute> no Visual Basic no .NET Framework versão 2.0.

O exemplo de código a seguir mostra o uso do <xref:System.Diagnostics.DebuggerBrowsableAttribute> para impedir que a propriedade após ele seja exibida na janela de depuração da classe.

```csharp
[DebuggerBrowsable(DebuggerBrowsableState.Never)]
public static string y = "Test String";
```

## <a name="using-the-debuggertypeproxy"></a>Usando o DebuggerTypeProxy
 Use o atributo <xref:System.Diagnostics.DebuggerTypeProxyAttribute> quando você precisar alterar a exibição de depuração de um tipo de forma significativa e básica, mas não o próprio tipo. O atributo <xref:System.Diagnostics.DebuggerTypeProxyAttribute> é usado para especificar um proxy de exibição para um tipo, permitindo que um desenvolvedor adapte a exibição para o tipo.  Este atributo, como o <xref:System.Diagnostics.DebuggerDisplayAttribute>, pode ser usado no nível do assembly, caso em que a propriedade <xref:System.Diagnostics.DebuggerTypeProxyAttribute.Target%2A> especifica o tipo para o qual o proxy será usado. O uso recomendado é que este atributo especifique um tipo aninhado particular que ocorre dentro do tipo ao qual o atributo é aplicado.  Um avaliador de expressão que dá suporte a visualizadores de tipo verifica se esse atributo existe quando um tipo é exibido. Se o atributo for encontrado, o avaliador de expressão substituirá o tipo de proxy de exibição do tipo ao qual o atributo é aplicado.

 Quando o <xref:System.Diagnostics.DebuggerTypeProxyAttribute> está presente, a janela de variáveis do depurador exibe apenas os membros públicos do tipo de proxy. Os membros particulares não são exibidos. O comportamento da janela de dados não é alterado por exibições aprimoradas por atributo.

 Para evitar penalidades de desempenho desnecessárias, os atributos do proxy de exibição não são processados até que o objeto seja expandido, por meio do clique pelo usuário no sinal de adição (+) ao lado do tipo em uma janela de dados ou por meio da aplicação do atributo <xref:System.Diagnostics.DebuggerBrowsableAttribute>. Portanto, é recomendável que nenhum atributo seja aplicado ao tipo de exibição. Os atributos podem e devem ser aplicados no corpo do tipo de exibição.

 O exemplo de código a seguir mostra o uso do <xref:System.Diagnostics.DebuggerTypeProxyAttribute> para especificar um tipo a ser usado como um proxy de exibição do depurador.

```csharp
[DebuggerTypeProxy(typeof(HashtableDebugView))]
class MyHashtable : Hashtable
{
    private const string TestString =
        "This should not appear in the debug window.";

    internal class HashtableDebugView
    {
        private Hashtable hashtable;
        public const string TestStringProxy =
            "This should appear in the debug window.";

        // The constructor for the type proxy class must have a
        // constructor that takes the target type as a parameter.
        public HashtableDebugView(Hashtable hashtable)
        {
            this.hashtable = hashtable;
        }
    }
}
```

## <a name="example"></a>Exemplo

### <a name="description"></a>Descrição

O exemplo de código a seguir pode ser exibido no Visual Studio para ver os resultados da aplicação dos <xref:System.Diagnostics.DebuggerDisplayAttribute> <xref:System.Diagnostics.DebuggerBrowsableAttribute> atributos, e <xref:System.Diagnostics.DebuggerTypeProxyAttribute> .

### <a name="code"></a>Código

[!code-cpp[System.Diagnostics.DebuggerBrowsableAttribute#1](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.Diagnostics.DebuggerBrowsableAttribute/cpp/program.cpp#1)]
[!code-csharp[System.Diagnostics.DebuggerBrowsableAttribute#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.Diagnostics.DebuggerBrowsableAttribute/CS/program.cs#1)]
[!code-vb[System.Diagnostics.DebuggerBrowsableAttribute#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.Diagnostics.DebuggerBrowsableAttribute/VB/module1.vb#1)]

## <a name="see-also"></a>Confira também

- <xref:System.Diagnostics.DebuggerDisplayAttribute>
- <xref:System.Diagnostics.DebuggerBrowsableAttribute>
- <xref:System.Diagnostics.DebuggerTypeProxyAttribute>
