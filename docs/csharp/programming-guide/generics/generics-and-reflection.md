---
title: Genéricos e reflexão – Guia de Programação em C#
description: Saiba como usar a reflexão para obter informações sobre tipos genéricos. Exibir listas de termos e condições para reflexão genérica.
ms.date: 07/20/2015
helpviewer_keywords:
- generics [C#], reflection
- reflection [C#], generic types
ms.assetid: 162fd9b4-dd5b-4abb-8c9b-e44e21e2f451
ms.openlocfilehash: 6e4387fe7e78cd0e970531ae42f323efa8f181db
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87299299"
---
# <a name="generics-and-reflection-c-programming-guide"></a>Genéricos e reflexão (Guia de Programação em C#)
Como o tempo de execução do CLR (Common Language Runtime) tem acesso às informações de tipo genérico em tempo de execução, você pode usar a reflexão para obter informações sobre tipos genéricos da mesma forma como para tipos não genéricos. Para obter mais informações, consulte [Genéricos em tempo de execução](./generics-in-the-run-time.md).  
  
 No .NET Framework 2,0, vários novos membros foram adicionados à <xref:System.Type> classe para habilitar informações em tempo de execução para tipos genéricos. Consulte a documentação dessas classes para obter mais informações sobre como usar esses métodos e propriedades. O namespace <xref:System.Reflection.Emit> também contém novos membros que dão suporte a genéricos. Consulte [Como definir um tipo genérico com a emissão de reflexão](../../../framework/reflection-and-codedom/how-to-define-a-generic-type-with-reflection-emit.md).  
  
 Para obter uma lista das condições invariáveis para termos usados na reflexão genérica, consulte os comentários da propriedade <xref:System.Type.IsGenericType%2A>.  
  
|Nome do membro System.Type|Descrição|  
|-----------------------------|-----------------|  
|<xref:System.Type.IsGenericType%2A>|Retorna verdadeiro se um tipo for genérico.|  
|<xref:System.Type.GetGenericArguments%2A>|Retorna uma matriz de objetos `Type` que representa os argumentos de tipo fornecidos para um tipo construído ou os parâmetros de tipo de uma definição de tipo genérico.|  
|<xref:System.Type.GetGenericTypeDefinition%2A>|Retorna a definição de tipo genérico subjacente para o tipo construído atual.|  
|<xref:System.Type.GetGenericParameterConstraints%2A>|Retorna uma matriz de objetos `Type` que representam as restrições no parâmetro de tipo genérico atual.|  
|<xref:System.Type.ContainsGenericParameters%2A>|Retorna verdadeiro se o tipo ou qualquer um dos seus tipos ou métodos de delimitação contêm parâmetros de tipo para os quais não foram fornecidos tipos específicos.|  
|<xref:System.Type.GenericParameterAttributes%2A>|Obtém uma combinação de sinalizadores `GenericParameterAttributes` que descrevem as restrições especiais do parâmetro de tipo genérico atual.|  
|<xref:System.Type.GenericParameterPosition%2A>|Para um objeto `Type` que representa um parâmetro de tipo, obtém a posição do parâmetro de tipo na lista de parâmetros de tipo da definição de tipo genérico ou da definição de método genérico que declarou o parâmetro de tipo.|  
|<xref:System.Type.IsGenericParameter%2A>|Obtém um valor que indica se o `Type` atual representa um parâmetro de tipo de um tipo genérico ou uma definição de método.|  
|<xref:System.Type.IsGenericTypeDefinition%2A>|Obtém um valor que indica se o <xref:System.Type> atual representa uma definição de tipo genérico, da qual outros tipos genéricos podem ser construídos. Retorna verdadeiro se o tipo representa a definição de um tipo genérico.|  
|<xref:System.Type.DeclaringMethod%2A>|Retorna o método genérico que definiu o parâmetro de tipo genérico atual ou nulo, se o parâmetro de tipo não foi definido por um método genérico.|  
|<xref:System.Type.MakeGenericType%2A>|Substitui os elementos de uma matriz de tipos pelos parâmetros de tipo da definição de tipo genérico atual e retorna um objeto <xref:System.Type> que representa o tipo construído resultante.|  
  
 Além disso, membros da classe <xref:System.Reflection.MethodInfo> habilitam informações em tempo de execução para métodos genéricos. Consulte os comentários sobre a propriedade <xref:System.Reflection.MethodBase.IsGenericMethod%2A> para obter uma lista das condições invariáveis para termos usados para refletir sobre os métodos genéricos.  
  
|Nome do membro System.Reflection.MemberInfo|Descrição|  
|----------------------------------------------|-----------------|  
|<xref:System.Reflection.MethodBase.IsGenericMethod%2A>|Retorna verdadeiro se um método for genérico.|  
|<xref:System.Reflection.MethodInfo.GetGenericArguments%2A>|Retorna uma matriz de objetos Type que representam os argumentos de tipo de um método genérico construído ou os parâmetros de tipo de uma definição de método genérico.|  
|<xref:System.Reflection.MethodInfo.GetGenericMethodDefinition%2A>|Retorna a definição de método genérico subjacente para o método construído atual.|  
|<xref:System.Reflection.MethodBase.ContainsGenericParameters%2A>|Retorna verdadeiro se o método ou qualquer um dos seus tipos de delimitação contêm parâmetros de tipo para os quais não foram fornecidos tipos específicos.|  
|<xref:System.Reflection.MethodBase.IsGenericMethodDefinition%2A>|Retorna verdadeiro se o <xref:System.Reflection.MethodInfo> atual representar a definição de um método genérico.|  
|<xref:System.Reflection.MethodInfo.MakeGenericMethod%2A>|Substitui os elementos de uma matriz de tipos pelos parâmetros de tipo da definição de método genérico atual e retorna um objeto <xref:System.Reflection.MethodInfo> que representa o método construído resultante.|  
  
## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Genéricos](./index.md)
- [Reflexão e tipos genéricos](../../../framework/reflection-and-codedom/reflection-and-generic-types.md)
- [Genéricos](../../../standard/generics/index.md)
