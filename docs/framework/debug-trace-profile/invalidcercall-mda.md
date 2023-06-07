---
title: MDA invalidCERCall
description: Examine o MDA (Assistente de depuração gerenciada) invalidCERCall, que será ativado se houver uma chamada inválida dentro do grafo CER (região de execução restrita).
ms.date: 03/30/2017
helpviewer_keywords:
- invalid CER calls
- InvalidCERCall MDA
- MDAs (managed debugging assistants), CER calls
- constrained execution regions
- CER calls
- managed debugging assistants (MDAs), CER calls
ms.assetid: c4577410-602e-44e5-9dab-fea7c55bcdfe
ms.openlocfilehash: dec32a81929d72274757b75cb03d6615d9fa948b
ms.sourcegitcommit: 0edbeb66d71b8df10fcb374cfca4d731b58ccdb2
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86051786"
---
# <a name="invalidcercall-mda"></a>MDA invalidCERCall
O MDA (Assistente de Depuração Gerenciado) de `invalidCERCall` é ativado quando há uma chamada de dentro do gráfico de CER (região de execução restrita) a um método que não tem nenhum contrato de confiabilidade um tem um contrato excessivamente fraco. Um contrato fraco é um contrato que declara que o pior caso de corrupção de estado tem escopo maior do que a instância passou para a chamada, ou seja, o <xref:System.AppDomain> ou o estado do processo pode se corromper ou o resultado dele não é sempre computável de forma determinística quando chamado dentro de uma CER.  
  
## <a name="symptoms"></a>Sintomas  
 Resultados inesperados ao executar código em uma CER. Os sintomas não são específicos. Eles poderiam ser uma <xref:System.OutOfMemoryException> inesperada, uma <xref:System.Threading.ThreadAbortException> ou outras exceções na chamada para o método não confiável, porque o runtime não o preparou com antecedência nem o protegeu de exceções <xref:System.Threading.ThreadAbortException> em runtime. Uma ameaça maior é que qualquer exceção resultante do método em tempo de execução pode deixar o <xref:System.AppDomain> ou o processo em um estado instável, o que contraria o objetivo de uma CER. O motivo pelo qual uma CER é criada é para evitar corrupções de estado como essa. Os sintomas de estado corrompido são específicos do aplicativo porque a definição de estado consistente é diferente entre aplicativos.  
  
## <a name="cause"></a>Causa  
 O código dentro de uma CER está chamando uma função sem nenhum <xref:System.Runtime.ConstrainedExecution.ReliabilityContractAttribute> ou com um <xref:System.Runtime.ConstrainedExecution.ReliabilityContractAttribute> fraco que não é compatível com a execução em uma CER.  
  
 Em termos de sintaxe de contrato de confiabilidade, um contrato fraco é um contrato que não especifica um valor de enumeração <xref:System.Runtime.ConstrainedExecution.Consistency> ou especifica um valor de <xref:System.Runtime.ConstrainedExecution.Consistency> de <xref:System.Runtime.ConstrainedExecution.Consistency.MayCorruptProcess>, <xref:System.Runtime.ConstrainedExecution.Consistency.MayCorruptAppDomain> ou <xref:System.Runtime.ConstrainedExecution.Cer.None>. Qualquer uma das condições a seguir indica que o código de chamada pode impedir os esforços de outro código na CER para manter o estado consistente.  CERs permitem que o código trate erros de maneira muito determinística, mantendo invariáveis internas que são importantes para o aplicativo e permitindo que ele continue em execução ao enfrentar erros transitórios, como exceções de memória insuficiente.  
  
 A ativação desse MDA indica uma possibilidade de que o método ser chamado na CER pode falhar de forma que o chamador não esperava ou que deixa o <xref:System.AppDomain> ou o estado do processo irrecuperável ou corrompido. Naturalmente, o código chamado pode ser executado corretamente e o problema é simplesmente um contrato ausente. No entanto, os problemas envolvidos ao escrever código confiável são sutis e a ausência de um contrato é um bom indicador de que o código pode não ser executado corretamente. Os contratos são indicadores de que o programador codificou de modo confiável e também são promessas de que essas garantias não serão alteradas em revisões futuras do código.  Ou seja, os contratos são declarações de intenção e não apenas detalhes de implementação.  
  
 Já que qualquer método com um contrato fraco ou inexistente pode vir a falhar de muitas maneiras imprevisíveis, o runtime não tenta remover do método nenhuma das suas próprias falhas imprevisíveis que são introduzidas por compilação JIT lenta, população do dicionário genérico ou anulações de thread, por exemplo. Ou seja, quando esse MDA é ativado, ele indica que o runtime não incluiu o método chamado na CER sendo definida; o grafo de chamadas foi encerrado neste nó porque continuar a preparar essa subárvore ajudaria a mascarar o erro em potencial.  
  
## <a name="resolution"></a>Resolução  
 Adicionar um contrato de confiabilidade válido para a função ou evitar o uso dessa chamada de função.  
  
## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime  
 O efeito de chamar um contrato fraco de uma CER pode ser a falha da CER em concluir suas operações. Isso pode levar à corrupção do estado do processo <xref:System.AppDomain>.  
  
## <a name="output"></a>Saída  
 A seguir, a saída de exemplo deste MDA.  
  
 `Method 'MethodWithCer', while executing within a constrained execution region, makes a call at IL offset 0x000C to 'MethodWithWeakContract', which does not have a sufficiently strong reliability contract and might cause non-deterministic results.`  
  
## <a name="configuration"></a>Configuração  
  
```xml  
<mdaConfig>  
  <assistants>  
    <invalidCERCall />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareMethod%2A>
- <xref:System.Runtime.ConstrainedExecution>
- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
