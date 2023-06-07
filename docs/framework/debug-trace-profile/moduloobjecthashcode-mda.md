---
title: MDA moduloObjectHashcode
description: Examine o MDA (Assistente de depuração gerenciada) do moduloObjectHashcode, que altera a classe de objeto para obter um valor restante em um resultado de método GetHashCode.
ms.date: 03/30/2017
helpviewer_keywords:
- managed debugging assistants (MDAs), hashcode modulus
- Modulo object hash code
- moduloObjectHashcode MDA
- hashcode modulus
- MDAs (managed debugging assistants), hashcode modulus
- GetHashCode method
- modulus of hashcodes
ms.assetid: b45366ff-2a7a-4b8e-ab01-537b72e9de68
ms.openlocfilehash: a929ec2b9196f1f6cad0528fdf7323839a86fa55
ms.sourcegitcommit: 0edbeb66d71b8df10fcb374cfca4d731b58ccdb2
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86052059"
---
# <a name="moduloobjecthashcode-mda"></a>MDA moduloObjectHashcode
O MDA (Assistente de Depuração Gerenciado) de `moduloObjectHashcode` altera o comportamento da classe <xref:System.Object> para executar uma operação de módulo no código hash retornado pelo método <xref:System.Object.GetHashCode%2A>. O módulo padrão para esse MDA é 1, o que faz com que <xref:System.Object.GetHashCode%2A> retorne 0 para todos os objetos.  
  
## <a name="symptoms"></a>Sintomas  
 Depois de migrar para uma nova versão do CLR (Common Language Runtime), um programa não é mais executado corretamente:  
  
- O programa está obtendo o objeto errado de um <xref:System.Collections.Hashtable>.  
  
- A ordem de enumeração de um <xref:System.Collections.Hashtable> tem uma alteração que interrompe o programa.  
  
- Dois objetos que costumavam ser iguais não o são mais.  
  
- Dois objetos que costumavam não ser iguais agora o são.  
  
## <a name="cause"></a>Causa  
 Seu programa pode estar obtendo o objeto errado de um <xref:System.Collections.Hashtable> porque a implementação do método <xref:System.Object.Equals%2A> na classe para a chave no <xref:System.Collections.Hashtable> testa a igualdade de objetos ao comparar os resultados da chamada para o método <xref:System.Object.GetHashCode%2A>. Códigos hash não devem ser usados para testar a igualdade de objetos porque os dois objetos podem ter o mesmo código hash, mesmo que seus respectivos campos tenham valores diferentes. Esses conflitos de código hash, embora raros na prática, ocorrem. O efeito disso em uma pesquisa <xref:System.Collections.Hashtable> é que duas chaves que não são iguais parecem ser e o objeto errado é retornado do <xref:System.Collections.Hashtable>. Por motivos de desempenho, a implementação de <xref:System.Object.GetHashCode%2A> pode variar entre as versões de runtime, então colisões que podem não ocorrer em uma versão podem ocorrer em versões subsequentes. Quando códigos hash entrarem em conflito, habilite esse MDA para testar se o seu código tem bugs. Quando habilitado, esse MDA faz com que o método <xref:System.Object.GetHashCode%2A> retorne 0, resultando em uma colisão de todos os códigos hash. O único efeito que habilitar esse MDA deve ter em seu programa é tornar a execução dele mais lenta.  
  
 A ordem de enumeração de um <xref:System.Collections.Hashtable> poderá variar de uma versão de runtime para outra se o algoritmo usado para calcular os códigos hash para a chave for alterado. Para testar se o seu programa obteve uma dependência na ordem de enumeração de chaves ou valores de uma tabela de hash, você pode habilitar esse MDA.  
  
## <a name="resolution"></a>Resolução  
 Nunca use códigos hash como um substituto para a identidade do objeto. Implemente a substituição do método <xref:System.Object.Equals%2A?displayProperty=nameWithType> para não comparar códigos hash.  
  
 Não crie dependências na ordem das enumerações de chaves ou valores em tabelas de hash.  
  
## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime  
 Os aplicativos são executados mais lentamente quando esse MDA está habilitado. Esse MDA simplesmente usa o código hash que teria sido retornado e em vez disso, retorna o resto quando dividido por um módulo.  
  
## <a name="output"></a>Saída  
 Não há nenhuma saída para esse MDA.  
  
## <a name="configuration"></a>Configuração  
 O atributo `modulus` especifica o módulo usado no código hash. O valor padrão é 1.  
  
```xml  
<mdaConfig>  
  <assistants>  
    <moduloObjectHashcode modulus="1" />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Object.GetHashCode%2A?displayProperty=nameWithType>
- <xref:System.Object.Equals%2A?displayProperty=nameWithType>
- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
