---
title: Recuperando informações armazenadas em atributos
description: Aprenda a recuperar informações armazenadas em atributos, como para uma instância de atributo, muitas instâncias para o mesmo escopo, & muitas instâncias para escopos diferentes.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- retrieving attributes
- multiple attribute instances
- attributes [.NET Framework], retrieving
ms.assetid: 37dfe4e3-7da0-48b6-a3d9-398981524e1c
ms.openlocfilehash: cf147a0ae6833039247c4c0878996973cc3db545
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84661855"
---
# <a name="retrieving-information-stored-in-attributes"></a>Recuperando informações armazenadas em atributos
Recuperar um atributo personalizado é um processo simples. Primeiro, declare uma instância do atributo que você deseja recuperar. Em seguida, use o método <xref:System.Attribute.GetCustomAttribute%2A?displayProperty=nameWithType> para inicializar o novo atributo para o valor do atributo que você deseja recuperar. Após a inicialização do novo atributo, basta usar suas propriedades para obter os valores.  
  
> [!IMPORTANT]
> Este tópico descreve como recuperar atributos para o código carregado no contexto da execução. Para recuperar atributos do código carregado no contexto somente reflexão, use a classe <xref:System.Reflection.CustomAttributeData>, conforme mostrado em [Como carregar assemblies no contexto somente reflexão](../../framework/reflection-and-codedom/how-to-load-assemblies-into-the-reflection-only-context.md).  
  
 Esta seção descreve as seguintes maneiras de recuperar os atributos:  
  
- [Recuperando uma única instância de um atributo](#cpconretrievingsingleinstanceofattribute)  
  
- [Recuperar várias instâncias de um atributo aplicado ao mesmo escopo](#cpconretrievingmultipleinstancesofattributeappliedtosamescope)  
  
- [Recuperando várias instâncias de um atributo aplicado a escopos diferentes](#cpconretrievingmultipleinstancesofattributeappliedtodifferentscopes)  
  
<a name="cpconretrievingsingleinstanceofattribute"></a>
## <a name="retrieving-a-single-instance-of-an-attribute"></a>Recuperar uma única instância de um atributo  
 No exemplo a seguir, o `DeveloperAttribute` (descrito na seção anterior) é aplicado à classe `MainApp` no nível da classe. O método `GetAttribute` usa **GetCustomAttribute** para recuperar os valores armazenados em `DeveloperAttribute` no nível da classe antes de exibi-los no console.  
  
 [!code-cpp[Conceptual.Attributes.Usage#18](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source3.cpp#18)]
 [!code-csharp[Conceptual.Attributes.Usage#18](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source3.cs#18)]
 [!code-vb[Conceptual.Attributes.Usage#18](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source3.vb#18)]  
  
 Este programa exibe o texto a seguir quando executado.  
  
```console  
The Name Attribute is: Joan Smith.  
The Level Attribute is: 42.  
The Reviewed Attribute is: True.  
```  
  
 Se o atributo não for encontrado, o método **GetCustomAttribute** inicializará `MyAttribute` com um valor nulo. Este exemplo verifica em `MyAttribute` a existência dessa instância e notifica o usuário caso nenhum atributo seja encontrado. Se o `DeveloperAttribute` não for encontrado no escopo da classe, a mensagem a seguir será exibida no console.  
  
```console  
The attribute was not found.
```  
  
 Este exemplo supõe que a definição de atributo está no namespace atual. Lembre-se de importar o namespace no qual a definição do atributo reside, se não estiver no namespace atual.  
  
<a name="cpconretrievingmultipleinstancesofattributeappliedtosamescope"></a>
## <a name="retrieving-multiple-instances-of-an-attribute-applied-to-the-same-scope"></a>Recuperar várias instâncias de um atributo aplicado ao mesmo escopo  
 No exemplo anterior, a classe a ser inspecionada e o atributo específico para localizar são passados para <xref:System.Attribute.GetCustomAttribute%2A>. Esse código funcionará bem se apenas uma instância de um atributo for aplicada no nível de classe. No entanto, se várias instâncias de um atributo forem aplicadas no mesmo nível de classe, o método **GetCustomAttribute** não recuperará todas as informações. Em casos em que várias instâncias do mesmo atributo são aplicadas ao mesmo escopo, você pode usar <xref:System.Attribute.GetCustomAttributes%2A?displayProperty=nameWithType> para colocar todas as instâncias de um atributo em uma matriz. Por exemplo, se duas instâncias de `DeveloperAttribute` forem aplicadas no nível de classe da mesma classe, o método `GetAttribute` poderá ser modificado para exibir as informações encontradas nos dois atributos. Lembre-se, para aplicar vários atributos no mesmo nível, o atributo deverá ser definido com a propriedade **AllowMultiple** definida como **true** no <xref:System.AttributeUsageAttribute>.  
  
 O exemplo de código a seguir mostra como usar o método **GetCustomAttributes** para criar uma matriz que faz referência a todas as instâncias de `DeveloperAttribute` em qualquer classe específica. Os valores de todos os atributos são exibidos no console.  
  
 [!code-cpp[Conceptual.Attributes.Usage#19](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source3.cpp#19)]
 [!code-csharp[Conceptual.Attributes.Usage#19](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source3.cs#19)]
 [!code-vb[Conceptual.Attributes.Usage#19](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source3.vb#19)]  
  
 Se nenhum atributo for encontrado, esse código alertará o usuário. Caso contrário, as informações contidas nas duas instâncias de `DeveloperAttribute` serão exibidas.  
  
<a name="cpconretrievingmultipleinstancesofattributeappliedtodifferentscopes"></a>
## <a name="retrieving-multiple-instances-of-an-attribute-applied-to-different-scopes"></a>Recuperar várias instâncias de um atributo aplicado a escopos diferentes  
 Os métodos <xref:System.Attribute.GetCustomAttributes%2A> e <xref:System.Attribute.GetCustomAttribute%2A> não pesquisam uma classe inteira e retornam todas as instâncias de um atributo nessa classe. Em vez disso, eles pesquisam somente um método ou membro especificado por vez. Se você tiver uma classe com o mesmo atributo aplicado a cada membro e quiser recuperar os valores em todos os atributos aplicados a esses membros, forneça cada método ou membro individualmente para **GetCustomAttributes** e ** GetCustomAttribute**.  
  
 O exemplo de código a seguir usa uma classe como um parâmetro e procura pelo `DeveloperAttribute` (definido anteriormente) no nível de classe e em cada método individual dessa classe.  
  
 [!code-cpp[Conceptual.Attributes.Usage#20](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source3.cpp#20)]
 [!code-csharp[Conceptual.Attributes.Usage#20](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source3.cs#20)]
 [!code-vb[Conceptual.Attributes.Usage#20](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source3.vb#20)]  
  
 Se nenhuma instância do `DeveloperAttribute` for encontrada no nível de método ou nível de classe, o método `GetAttribute` notificará o usuário de que nenhum atributo foi encontrado e exibirá o nome do método ou classe que contém o atributo. Se um atributo for encontrado, os campos `Name`, `Level` e `Reviewed` serão exibidos no console.  
  
 Você pode usar os membros da classe <xref:System.Type> para obter os métodos e membros individuais na classe passada. Primeiro, esse exemplo consulta o objeto **Type** para obter informações de atributo para o nível de classe. Em seguida, ele usa <xref:System.Type.GetMethods%2A?displayProperty=nameWithType> para colocar instâncias de todos os métodos em uma matriz de objetos <xref:System.Reflection.MemberInfo?displayProperty=nameWithType> para recuperar informações de atributo para o nível de método. Você também pode usar o método <xref:System.Type.GetProperties%2A?displayProperty=nameWithType> para verificar os atributos no nível de propriedade, ou <xref:System.Type.GetConstructors%2A?displayProperty=nameWithType> para verificar os atributos no nível do construtor.  
  
## <a name="see-also"></a>Confira também

- <xref:System.Type?displayProperty=nameWithType>
- <xref:System.Attribute.GetCustomAttribute%2A?displayProperty=nameWithType>
- <xref:System.Attribute.GetCustomAttributes%2A?displayProperty=nameWithType>
- [Atributos](index.md)
