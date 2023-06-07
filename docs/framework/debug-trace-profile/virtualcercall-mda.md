---
title: MDA virtualCERCall
description: Examine o MDA (Assistente de depuração gerenciada) virtualCERCall, que é invocado se uma CER contém uma chamada para um método virtual que não pode ser preparado automaticamente.
ms.date: 03/30/2017
helpviewer_keywords:
- MDAs (managed debugging assistants), CER calls
- virtualCERCall MDA
- virtual CER calls
- constrained execution regions
- CER calls
- managed debugging assistants (MDAs), CER calls
ms.assetid: 1eb18c7a-f5e0-443f-80fb-67bfbb047da2
ms.openlocfilehash: fab0686b1c7d2fbb1485f6e4b82d008495a553cd
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85803554"
---
# <a name="virtualcercall-mda"></a>MDA virtualCERCall
O MDA (assistente para depuração gerenciada) `virtualCERCall` é ativado como um aviso, indicando que um site de chamada em um gráfico de chamadas da CER (região de execução restrita) se refere a um destino virtual, ou seja, uma chamada virtual a um método virtual não final ou uma chamada usando uma interface. O CLR (Common Language Runtime) não pode prever o método de destino dessas chamadas pela análise de metadados e de linguagem intermediária apenas. Como resultado, a árvore de chamadas não pode ser preparada como parte do gráfico da CER e as anulações de thread nessa subárvore não podem ser bloqueadas automaticamente. Esse MDA avisa sobre casos em que uma CER talvez precise ser estendida usando chamadas explícitas ao método <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareMethod%2A> depois que as informações adicionais necessárias para calcular o destino de chamada são conhecidas em tempo de execução.  
  
## <a name="symptoms"></a>Sintomas  
 As CERs não são executadas quando um thread é anulado ou um domínio do aplicativo é descarregado.  
  
## <a name="cause"></a>Causa  
 Uma CER contém uma chamada a um método virtual que não pode ser preparado automaticamente.  
  
## <a name="resolution"></a>Resolução  
 Chame <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareMethod%2A> para o método virtual.  
  
## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime  
 Esse MDA não tem efeito sobre o CLR.  
  
## <a name="output"></a>Saída  
  
```output
Method 'MethodWithCer', while executing within a constrained execution region, makes a call  
at IL offset 0x0024 to 'VirtualMethod', which is virtual and cannot be prepared automatically  
at compile time. The caller must ensure this method is prepared explicitly at  
runtime before entering the constrained execution region.  
method name="VirtualMethod"  
declaringType name="VirtualCERCall+MyClass"  
  declaringModule name="mda"  
    callsite name="MethodWithCer" offset="0x0024"  
```  
  
## <a name="configuration"></a>Configuração  
  
```xml  
<mdaConfig>  
  <assistants>  
    <VirtualCERCall />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="example"></a>Exemplo  
  
```csharp
class MyClass  
{  
    [ReliabilityContract(Consistency.MayCorruptProcess, CER.None)]  
    virtual void VirtualMethod()  
    {  
        ...  
    }  
}  
  
class MyDerivedClass : MyClass  
{  
    [ReliabilityContract(Consistency.MayCorruptProcess, CER.None)]  
    override void VirtualMethod()  
    {  
        ...  
    }  
}  
  
void MethodWithCer(MyClass object)  
{  
    RuntimeHelpers.PrepareConstrainedRegions();  
    try  
    {  
        ...  
    }  
    finally  
    {  
        // Start of the CER.  
  
        // Cannot tell at analysis time whether object is a MyClass  
        // or a MyDerivedClass, so we do not know which version of
        // VirtualMethod we are going to call.  
        object.VirtualMethod();  
    }  
}  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
- [Realizando marshaling de interoperabilidade](../interop/interop-marshaling.md)
