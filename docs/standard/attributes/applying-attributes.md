---
title: Aplicando atributos
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- assemblies [.NET Framework], attributes
- attributes [.NET Framework], applying
ms.assetid: dd7604eb-9fa3-4b60-b2dd-b47739fa3148
ms.openlocfilehash: 5557da1531eb55c13d1c7540a50b044d1a7b8a1d
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84276329"
---
# <a name="applying-attributes"></a>Aplicando atributos
Use o processo a seguir para aplicar um atributo a um elemento do código.  
  
1. Defina um novo atributo ou use um atributo existente importando seu namespace do .NET Framework.  
  
2. Aplique o atributo ao elemento de código, colocando-o imediatamente antes do elemento.  
  
     Cada linguagem tem sua própria sintaxe de atributo. Em C++ e C#, o atributo é delimitado por colchetes e separado do elemento por um espaço em branco, que pode incluir uma quebra de linha. No Visual Basic, o atributo é delimitado por colchetes angulares e deve estar na mesma linha lógica. O caractere de continuação de linha pode ser usado se você quiser uma quebra de linha.
  
3. Especifique parâmetros posicionais e parâmetros nomeados para o atributo.  
  
     Os parâmetros posicionais são necessários e devem vir antes de quaisquer parâmetros nomeados. Eles correspondem aos parâmetros de um dos constructos do atributo. Os parâmetros nomeados são opcionais e correspondem às propriedades do atributo de leitura/gravação. Em C++ e C#, especifique `name`=`value` para cada parâmetro opcional, em que `name` é o nome da propriedade. No Visual Basic, especifique `name`:=`value`.  
  
 O atributo é emitido em metadados quando você compila o código e está disponível para o Common Language Runtime e todas as ferramentas ou aplicativos personalizados por meio dos serviços de reflexão do tempo de execução.  
  
 Por convenção, todos os nomes de atributos terminam com o sufixo Attribute. No entanto, várias linguagens que têm como destino o runtime, como Visual Basic e C#, não exigem que você especifique o nome completo de um atributo. Por exemplo, se quiser inicializar <xref:System.ObsoleteAttribute?displayProperty=nameWithType>, você só precisa fazer referência a ele como **Obsoleto**.  
  
## <a name="applying-an-attribute-to-a-method"></a>Aplicar um atributo a um método  
 O exemplo de código a seguir mostra como declarar **System.ObsoleteAttribute**, que marca o código como obsoleto. A cadeia de caracteres `"Will be removed in next version"` é passada para o atributo. Esse atributo faz com que o compilador que exibe a cadeia de caracteres transmitida emita um aviso quando o código que descreve o atributo for chamado.  
  
 [!code-cpp[Conceptual.Attributes.Usage#3](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source1.cpp#3)]
 [!code-csharp[Conceptual.Attributes.Usage#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source1.cs#3)]
 [!code-vb[Conceptual.Attributes.Usage#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source1.vb#3)]  
  
## <a name="applying-attributes-at-the-assembly-level"></a>Aplicar atributos no nível de assembly  
 Se você quiser aplicar um atributo no nível do assembly, use a **assembly** `Assembly` palavra-chave assembly (na Visual Basic). O código a seguir mostra o **AssemblyTitleAttribute** aplicado no nível de assembly.  
  
 [!code-cpp[Conceptual.Attributes.Usage#2](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source1.cpp#2)]
 [!code-csharp[Conceptual.Attributes.Usage#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source1.cs#2)]
 [!code-vb[Conceptual.Attributes.Usage#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source1.vb#2)]  
  
 Quando esse atributo é aplicado, a cadeia de caracteres `"My Assembly"` é colocada no manifesto do assembly na parte de metadados do arquivo. Você pode exibir o atributo usando o [Desmontador de MSIL (Ildasm.exe)](../../framework/tools/ildasm-exe-il-disassembler.md) ou criando um programa personalizado para recuperar o atributo.  
  
## <a name="see-also"></a>Veja também

- [Atributos](index.md)
- [Recuperando informações armazenadas em atributos](retrieving-information-stored-in-attributes.md)
- [Conceitos](/cpp/windows/attributed-programming-concepts)
- [Atributos (C#)](../../csharp/programming-guide/concepts/attributes/index.md)
- [Visão geral de atributos (Visual Basic)](../../visual-basic/programming-guide/concepts/attributes/index.md)
