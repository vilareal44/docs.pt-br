---
title: Demandas de link
description: Leia sobre as demandas de link, que causam uma verificação de segurança durante a compilação JIT (just-in-time) e examina apenas o assembly de chamada imediata do seu código.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- security [.NET Framework], demands
- demanded permissions
- permissions [.NET Framework], demands
- granting permissions, demands
- code access security, demands
- user demands for permission
- caller security checks
- link demands
ms.assetid: a33fd5f9-2de9-4653-a4f0-d9df25082c4d
ms.openlocfilehash: 7f5475d5bfff8cc3c500f95b05d54daacc9b253e
ms.sourcegitcommit: c37e8d4642fef647ebab0e1c618ecc29ddfe2a0f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87855784"
---
# <a name="link-demands"></a>Demandas de link

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]  
  
 Uma demanda de link causa uma verificação de segurança durante a compilação just-in-time e verifica apenas o assembly de chamada imediata do seu código. A vinculação ocorre quando seu código está associado a uma referência de tipo, incluindo referências de ponteiro de função e chamadas de método. Se o assembly de chamada não tiver permissão suficiente para vincular ao seu código, o link não será permitido e uma exceção de tempo de execução será lançada quando o código for carregado e executado. As demandas de link podem ser substituídas em classes que herdam do seu código.  
  
 Uma movimentação de pilha completa não é executada com esse tipo de demanda e o código ainda é suscetível a ataques chamariz. Por exemplo, se um método no assembly A for protegido por uma demanda de link, um chamador direto no assembly B será avaliado com base nas permissões do assembly B.  No entanto, a demanda de link não avaliará um método no assembly C se ele chamar indiretamente o método no assembly A usando o método no assembly B. A demanda de link especifica somente as permissões que os chamadores diretos no assembly de chamada imediata devem ter que vincular ao seu código. Ele não especifica as permissões que todos os chamadores devem ter para executar seu código.  
  
 Os <xref:System.Security.CodeAccessPermission.Assert%2A> <xref:System.Security.CodeAccessPermission.Deny%2A> <xref:System.Security.CodeAccessPermission.PermitOnly%2A> modificadores de movimentação de pilha, e não afetam a avaliação de demandas de link.  Como as demandas de link não executam uma movimentação de pilha, os modificadores de movimentação de pilha não têm nenhum efeito nas demandas de link.  
  
 Se um método protegido por uma demanda de link for acessado por meio de [reflexão](../reflection-and-codedom/reflection.md), uma demanda de link verificará o chamador imediato do código acessado por meio de reflexão. Isso é verdadeiro para a descoberta de método e para invocação de método executada usando reflexão. Por exemplo, suponha que o código use a reflexão para retornar um <xref:System.Reflection.MethodInfo> objeto que representa um método protegido por uma demanda de link e, em seguida, passe esse objeto **MethodInfo** para algum outro código que usa o objeto para invocar o método original. Nesse caso, a verificação de demanda de link ocorre duas vezes: uma vez para o código que retorna o objeto **MethodInfo** e uma vez para o código que o invoca.  
  
> [!NOTE]
> Uma demanda de link executada em um construtor de classe estática não protege o construtor porque construtores estáticos são chamados pelo sistema, fora do caminho de execução de código do aplicativo. Como resultado, quando uma demanda de link é aplicada a uma classe inteira, ela não pode proteger o acesso a um construtor estático, embora proteja o restante da classe.  
  
 O fragmento de código a seguir especifica declarativamente que qualquer vinculação de código para o `ReadData` método deve ter a `CustomPermission` permissão. Essa permissão é uma permissão personalizada hipotética e não existe no .NET Framework. A demanda é feita passando um sinalizador **SecurityAction. LinkDemand** para o `CustomPermissionAttribute` .  
  
```vb  
<CustomPermissionAttribute(SecurityAction.LinkDemand)> _  
Public Shared Function ReadData() As String  
    ' Access a custom resource.  
End Function
```  
  
```csharp  
[CustomPermissionAttribute(SecurityAction.LinkDemand)]  
public static string ReadData()  
{  
    // Access a custom resource.  
}  
```  
  
## <a name="see-also"></a>Veja também

- [Atributos](../../standard/attributes/index.md)
- [Segurança de acesso do código](code-access-security.md)
