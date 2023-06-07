---
title: Reflexão
ms.date: 07/20/2015
ms.assetid: d991bc0f-d16a-4ac5-9351-70e5c5b9891b
ms.openlocfilehash: 43c05a0b3bbfc3dfc304b1aed3f689625a40229a
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84413174"
---
# <a name="reflection-visual-basic"></a>Reflexão (Visual Basic)
A reflexão fornece objetos (do tipo <xref:System.Type>) que descrevem assemblies, módulos e tipos. É possível usar a reflexão para criar dinamicamente uma instância de um tipo, associar o tipo a um objeto existente ou obter o tipo de um objeto existente e invocar seus métodos ou acessar suas propriedades e campos. Se você estiver usando atributos em seu código, a reflexão permite acessá-los. Para obter mais informações, consulte [Atributos](../../../standard/attributes/index.md).  
  
 Veja um exemplo simples de reflexão usando o método estático `GetType` – herdado por todos os tipos da classe base `Object` – para obter o tipo de uma variável:  
  
```vb  
' Using GetType to obtain type information:  
Dim i As Integer = 42  
Dim type As System.Type = i.GetType()  
System.Console.WriteLine(type)  
```  
  
 A saída é:  
  
 `System.Int32`  
  
 O exemplo a seguir usa a reflexão para obter o nome completo do assembly carregado.  
  
```vb  
' Using Reflection to get information from an Assembly:  
Dim info As System.Reflection.Assembly = GetType(System.Int32).Assembly  
System.Console.WriteLine(info)  
```  
  
 A saída é:  
  
 `mscorlib, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089`  
  
## <a name="reflection-overview"></a>Visão geral da reflexão  
 A reflexão é útil nas seguintes situações:  
  
- Quando você precisa acessar atributos nos metadados do seu programa. Para obter mais informações, consulte [Recuperando informações armazenadas em atributos](../../../standard/attributes/retrieving-information-stored-in-attributes.md).  
  
- Para examinar e instanciar tipos em um assembly.  
  
- Para criar novos tipos em runtime. Usar as classes em <xref:System.Reflection.Emit>.  
  
- Para executar a associação tardia, acessar métodos em tipos criados em tempo de execução. Consulte o tópico [Carregando e usando tipos dinamicamente](../../../framework/reflection-and-codedom/dynamically-loading-and-using-types.md).  
  
## <a name="related-sections"></a>Seções relacionadas  
 Para obter mais informações:  
  
- [Reflexão](../../../framework/reflection-and-codedom/reflection.md)  
  
- [Exibindo informações de tipo](../../../framework/reflection-and-codedom/viewing-type-information.md)  
  
- [Reflexão e tipos genéricos](../../../framework/reflection-and-codedom/reflection-and-generic-types.md)  
  
- <xref:System.Reflection.Emit>  
  
- [Recuperando informações armazenadas em atributos](../../../standard/attributes/retrieving-information-stored-in-attributes.md)  
  
## <a name="see-also"></a>Confira também

- [Guia de programação do Visual Basic](../index.md)
- [Assemblies no .NET](../../../standard/assembly/index.md)
