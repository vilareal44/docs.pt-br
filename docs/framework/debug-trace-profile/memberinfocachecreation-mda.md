---
title: MDA memberInfoCacheCreation
description: Entenda o MDA (Assistente de depuração gerenciada) do memberInfoCacheCreation no .NET, que é ativado quando um cache MemberInfo é criado.
ms.date: 03/30/2017
helpviewer_keywords:
- member info cache creation
- MemberInfoCacheCreation MDA
- reflection, run-time errors
- MDAs (managed debugging assistants), cache
- cache [.NET Framework], reflection
- managed debugging assistants (MDAs), cache
- MemberInfo cache
ms.assetid: 5abdad23-1335-4744-8acb-934002c0b6fe
ms.openlocfilehash: c48be7ac8632b8072981be01e01997ee8c34b6b3
ms.sourcegitcommit: 0edbeb66d71b8df10fcb374cfca4d731b58ccdb2
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86051136"
---
# <a name="memberinfocachecreation-mda"></a>MDA memberInfoCacheCreation
O MDA (Assistente de Depuração Gerenciado) de `memberInfoCacheCreation` é ativado quando um cache de <xref:System.Reflection.MemberInfo> é criado. Isso é uma indicação forte de um programa que está fazendo uso de recursos de reflexão com consumo elevado de recursos computacionais.  
  
## <a name="symptoms"></a>Sintomas  
 O conjunto de trabalho de um programa aumenta porque o programa está usando reflexão com consumo elevado de recursos.  
  
## <a name="cause"></a>Causa  
 Operações de reflexão que envolvem objetos <xref:System.Reflection.MemberInfo> são consideradas grandes consumidoras de recursos porque elas devem ler os metadados que são armazenados em páginas frias e, em geral, eles indicam que o programa está usando algum tipo de cenário de associação tardia.  
  
## <a name="resolution"></a>Resolução  
 Você pode determinar o local em que a reflexão está sendo usada em seu programa habilitando esse MDA e, em seguida, executando o código em um depurador ou anexando com um depurador quando o MDA é ativado. Em um depurador, você obterá um rastreamento de pilha mostrando o local em que o cache <xref:System.Reflection.MemberInfo> foi criado e, desse ponto em diante, você poderá determinar o local em que o programa está usando reflexão.  
  
 A resolução depende dos objetivos do código. Esse MDA alerta você de que o programa tem um cenário de associação tardia. Você talvez queira determinar se você pode substituir um cenário de associação precoce ou considerar o desempenho do cenário de associação tardia.  
  
## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime  
 Esse MDA é ativado para cada cache <xref:System.Reflection.MemberInfo> criado. O impacto de desempenho é negligenciável.  
  
## <a name="output"></a>Saída  
 O MDA gera uma mensagem indicando que o cache <xref:System.Reflection.MemberInfo> foi criado. Use um depurador para obter um rastreamento de pilha mostrando o local em que o programa está usando reflexão.  
  
## <a name="configuration"></a>Configuração  
  
```xml  
<mdaConfig>  
  <assistants>  
    <memberInfoCacheCreation/>  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="example"></a>Exemplo  
 Esse código de exemplo ativará o MDA `memberInfoCacheCreation`.  
  
```csharp
using System;  
  
public class Exe  
{  
    public static void Main()  
    {  
        typeof(object).GetMethods();  
    }  
}  
```  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Reflection.MemberInfo>
- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
