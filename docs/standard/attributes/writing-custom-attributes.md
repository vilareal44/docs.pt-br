---
title: Escrevendo atributos personalizados
description: Crie seus próprios atributos personalizados no .NET. Os atributos personalizados são essencialmente classes derivadas de forma direta ou indireta de System. Attribute.
ms.date: 07/17/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- multiple attribute instances
- AttributeTargets enumeration
- attributes [.NET Framework], custom
- AllowMultiple property
- custom attributes
- AttributeUsageAttribute class, custom attributes
- Inherited property
- attribute classes, declaring
ms.assetid: 97216f69-bde8-49fd-ac40-f18c500ef5dc
ms.openlocfilehash: 3cae8de9b76aa9953b21ad2e23ad003e97555aa9
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84768476"
---
# <a name="writing-custom-attributes"></a>Escrevendo atributos personalizados
Para criar seus próprios atributos personalizados, não é preciso dominar muitos novos conceitos. Se estiver familiarizado com a programação orientada a objeto e souber como criar classes, você já domina a maior parte do conhecimento necessário. Os atributos personalizados são classes essencialmente tradicionais que derivam direta ou indiretamente de <xref:System.Attribute?displayProperty=nameWithType>. Como acontece nas classes tradicionais, os atributos personalizados contêm métodos que armazenam e recuperam dados.  
  
 As principais etapas para criar corretamente classes de atributos personalizados são as seguintes:  
  
- [Aplicar o AttributeUsageAttribute](#applying-the-attributeusageattribute)  
  
- [Declarar a classe de atributos](#declaring-the-attribute-class)  
  
- [Declarar constructos](#declaring-constructors)  
  
- [Declarar propriedades](#declaring-properties)  
  
 Esta seção descreve cada uma dessas etapas e oferece um [exemplo de atributo personalizado](#custom-attribute-example) no fim.  
  
## <a name="applying-the-attributeusageattribute"></a>Aplicar o AttributeUsageAttribute  
 A declaração de um atributo personalizado começa com o <xref:System.AttributeUsageAttribute?displayProperty=nameWithType>, que define algumas das principais características da classe de atributos. Por exemplo, é possível especificar se o atributo pode ser herdado por outras classes ou a quais elementos o atributo pode ser aplicado. O fragmento de código a seguir demonstra como usar o <xref:System.AttributeUsageAttribute>.  
  
 [!code-cpp[Conceptual.Attributes.Usage#5](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#5)]
 [!code-csharp[Conceptual.Attributes.Usage#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#5)]
 [!code-vb[Conceptual.Attributes.Usage#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#5)]  
  
 O <xref:System.AttributeUsageAttribute> possui três membros que são importantes para a criação de atributos personalizados: [AttributeTargets](#attributetargets-member), [Inherited](#inherited-property) e [AllowMultiple](#allowmultiple-property).  
  
### <a name="attributetargets-member"></a>Membro de AttributeTargets  
 No exemplo anterior, <xref:System.AttributeTargets.All?displayProperty=nameWithType> é especificado, indicando que esse atributo pode ser aplicado a todos os elementos do programa. Como alternativa, você pode especificar <xref:System.AttributeTargets.Class?displayProperty=nameWithType>, que indica que o atributo pode ser aplicado somente a uma classe, ou <xref:System.AttributeTargets.Method?displayProperty=nameWithType>, que indica que o atributo pode ser aplicado a um único método. Todos os elementos do programa podem ser marcados para serem descritos dessa maneira por um atributo personalizado.  
  
 Você também pode passar vários valores de <xref:System.AttributeTargets>. O fragmento de código a seguir especifica que um atributo personalizado pode ser aplicado a qualquer classe ou método.  
  
 [!code-cpp[Conceptual.Attributes.Usage#6](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#6)]
 [!code-csharp[Conceptual.Attributes.Usage#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#6)]
 [!code-vb[Conceptual.Attributes.Usage#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#6)]  
  
### <a name="inherited-property"></a>Propriedade Inherited  
 A propriedade <xref:System.AttributeUsageAttribute.Inherited%2A?displayProperty=nameWithType> indica se o atributo pode ser herdado pelas classes derivadas das classes às quais o atributo é aplicado. Essa propriedade aceita um sinalizador `true` (o padrão) ou `false`. No exemplo a seguir, `MyAttribute` tem um valor padrão <xref:System.AttributeUsageAttribute.Inherited%2A> de `true`, enquanto `YourAttribute` tem um valor <xref:System.AttributeUsageAttribute.Inherited%2A> de `false`.  
  
 [!code-cpp[Conceptual.Attributes.Usage#7](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#7)]
 [!code-csharp[Conceptual.Attributes.Usage#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#7)]
 [!code-vb[Conceptual.Attributes.Usage#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#7)]  
  
 Em seguida, os dois atributos são aplicados a um método na classe base `MyClass`.  
  
 [!code-cpp[Conceptual.Attributes.Usage#9](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#9)]
 [!code-csharp[Conceptual.Attributes.Usage#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#9)]
 [!code-vb[Conceptual.Attributes.Usage#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#9)]  
  
 Por fim, a classe `YourClass` é herdada da classe base `MyClass`. O método `MyMethod` mostra `MyAttribute`, mas não mostra `YourAttribute`.  
  
 [!code-cpp[Conceptual.Attributes.Usage#10](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#10)]
 [!code-csharp[Conceptual.Attributes.Usage#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#10)]
 [!code-vb[Conceptual.Attributes.Usage#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#10)]  
  
### <a name="allowmultiple-property"></a>Propriedade AllowMultiple  
 A propriedade <xref:System.AttributeUsageAttribute.AllowMultiple%2A?displayProperty=nameWithType> indica se várias instâncias do atributo podem existir em um elemento. Se for definida como `true`, várias instâncias serão permitidas; se for definida como `false` (padrão), apenas uma instância será permitida.  
  
 No exemplo a seguir, `MyAttribute` tem um valor padrão <xref:System.AttributeUsageAttribute.AllowMultiple%2A> de `false`, enquanto `YourAttribute` tem um valor de `true`.  
  
 [!code-cpp[Conceptual.Attributes.Usage#11](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#11)]
 [!code-csharp[Conceptual.Attributes.Usage#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#11)]
 [!code-vb[Conceptual.Attributes.Usage#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#11)]  
  
 Quando várias instâncias desses atributos são aplicadas, `MyAttribute` produz um erro do compilador. O exemplo de código a seguir mostra o uso válido de `YourAttribute` e o uso inválido de `MyAttribute`.  
  
 [!code-cpp[Conceptual.Attributes.Usage#13](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#13)]
 [!code-csharp[Conceptual.Attributes.Usage#13](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#13)]
 [!code-vb[Conceptual.Attributes.Usage#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#13)]  
  
 Se as propriedades <xref:System.AttributeUsageAttribute.AllowMultiple%2A> e <xref:System.AttributeUsageAttribute.Inherited%2A> estiverem definidas como `true`, uma classe herdada de outra classe poderá herdar um atributo e ter outra instância do mesmo atributo aplicada à mesma classe filho. Se <xref:System.AttributeUsageAttribute.AllowMultiple%2A> estiver definida como `false`, os valores de todos os atributos na classe pai serão substituídos pelas novas instâncias do mesmo atributo na classe filho.  
  
## <a name="declaring-the-attribute-class"></a>Declarar a classe de atributos  
 Depois de aplicar o <xref:System.AttributeUsageAttribute>, comece a definir as especificações do seu atributo. A declaração de uma classe de atributos é semelhante à declaração de uma classe tradicional, como demonstrado pelo código a seguir.  
  
 [!code-cpp[Conceptual.Attributes.Usage#14](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#14)]
 [!code-csharp[Conceptual.Attributes.Usage#14](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#14)]
 [!code-vb[Conceptual.Attributes.Usage#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#14)]  
  
 Esta definição de atributo demonstra os seguintes pontos:  
  
- As classes de atributos devem ser declaradas como classes públicas.  
  
- Por convenção, o nome da classe de atributos termina com a palavra **Attribute**. Embora não seja necessária, essa convenção é recomendada para facilitar a leitura. Quando o atributo é aplicado, a inclusão da palavra Attribute torna-se opcional.  
  
- Todas as classes de atributos devem ser herdadas diretamente ou indiretamente de <xref:System.Attribute?displayProperty=nameWithType>.  
  
- No Microsoft Visual Basic, todas as classes de atributos personalizados devem ter o atributo <xref:System.AttributeUsageAttribute?displayProperty=nameWithType>.  
  
## <a name="declaring-constructors"></a>Declarar constructos  
 Os atributos são inicializados com constructos, como as classes tradicionais. O fragmento de código a seguir ilustra um constructo de atributo típico. Esse construtor público aceita um parâmetro e define uma variável de membro igual ao seu valor.  
  
 [!code-cpp[Conceptual.Attributes.Usage#15](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#15)]
 [!code-csharp[Conceptual.Attributes.Usage#15](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#15)]
 [!code-vb[Conceptual.Attributes.Usage#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#15)]  
  
 Você pode sobrecarregar o constructo para acomodar diferentes combinações de valores. Se também definir uma [propriedade](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/65zdfbdt(v=vs.120)) para sua classe de atributos personalizados, você poderá usar uma combinação de parâmetros nomeados e posicionais ao inicializar o atributo. Normalmente, você define todos os parâmetros necessários como posicionais e todos os parâmetros opcionais como nomeados. Nesse caso, o atributo não pode ser inicializado sem o parâmetro necessário. Todos os outros parâmetros são opcionais. Observe que, no Visual Basic, o constructos de uma classe de atributos não devem usar um argumento ParamArray.  
  
 O exemplo de código a seguir mostra como um atributo que usa o constructo anterior pode ser aplicado usando parâmetros necessários e opcionais. Ele pressupõe que o atributo tem um valor booliano necessário e uma cadeia de caracteres opcional.  
  
 [!code-cpp[Conceptual.Attributes.Usage#17](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#17)]
 [!code-csharp[Conceptual.Attributes.Usage#17](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#17)]
 [!code-vb[Conceptual.Attributes.Usage#17](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#17)]  
  
## <a name="declaring-properties"></a>Declarar propriedades  
 Se quiser definir um parâmetro nomeado ou disponibilizar uma maneira fácil de retornar os valores armazenados pelo seu atributo, declare uma [propriedade](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/65zdfbdt(v=vs.120)). As propriedades de atributos devem ser declaradas como entidades públicas com uma descrição do tipo de dados que será retornado. Defina a variável que conterá o valor de sua propriedade e irá associá-la aos métodos **get** e **set**. O exemplo de código a seguir demonstra como implementar uma propriedade simples em seu atributo.  
  
 [!code-cpp[Conceptual.Attributes.Usage#16](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#16)]
 [!code-csharp[Conceptual.Attributes.Usage#16](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#16)]
 [!code-vb[Conceptual.Attributes.Usage#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#16)]  
  
## <a name="custom-attribute-example"></a>Exemplo de Atributo personalizado  
 Esta seção incorpora as informações anteriores e mostra como criar um atributo simples que documenta informações sobre o autor de uma seção de código. O atributo neste exemplo armazena o nome e o nível do programador, e se o código foi revisado. Ele usa três variáveis privadas para armazenar os valores reais que serão salvos. Cada variável é representada por uma propriedade pública que obtém e define os valores. Por fim, o constructo é definido com dois parâmetros necessários.  
  
 [!code-cpp[Conceptual.Attributes.Usage#4](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#4)]
 [!code-csharp[Conceptual.Attributes.Usage#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#4)]
 [!code-vb[Conceptual.Attributes.Usage#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#4)]  
  
 Você pode aplicar esse atributo usando o nome completo, `DeveloperAttribute`, ou usando o nome abreviado, `Developer`, em uma das seguintes maneiras.  
  
 [!code-cpp[Conceptual.Attributes.Usage#12](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#12)]
 [!code-csharp[Conceptual.Attributes.Usage#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#12)]
 [!code-vb[Conceptual.Attributes.Usage#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#12)]  
  
 O primeiro exemplo mostra o atributo aplicado com apenas os parâmetros nomeados necessários e o segundo mostra o atributo aplicado com os parâmetros necessários e opcionais.  
  
## <a name="see-also"></a>Veja também

- <xref:System.Attribute?displayProperty=nameWithType>
- <xref:System.AttributeUsageAttribute?displayProperty=nameWithType>
- [Atributos](index.md)
