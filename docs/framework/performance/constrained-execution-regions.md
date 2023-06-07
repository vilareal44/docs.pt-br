---
title: Regiões de execução restrita
description: Comece a usar as CER (regiões de execução restrita), que fazem parte de um mecanismo de criação de código gerenciado confiável.
ms.date: 03/30/2017
helpviewer_keywords:
- constrained execution regions
- CERs
ms.assetid: 99354547-39c1-4b0b-8553-938e8f8d1808
ms.openlocfilehash: d928c9357af4a02e389d9ffd5df4ad0195edab06
ms.sourcegitcommit: 0fa2b7b658bf137e813a7f4d09589d64c148ebf5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/14/2020
ms.locfileid: "86309605"
---
# <a name="constrained-execution-regions"></a>Regiões de execução restrita
Uma CER (região de execução restrita) faz parte de um mecanismo para a criação de código gerenciado confiável. A CER define uma área na qual o CLR (Common Language Runtime) é restrito de gerar exceções fora de banda que possam impedir que o código na área seja executado em sua totalidade. Nessa região, o código do usuário é restrito de executar um código que poderá resultar na geração de exceções fora de banda. O método <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegions%2A> deve preceder imediatamente um bloco `try` e marca os blocos `catch`, `finally` e `fault` como regiões de execução restrita. Depois de marcado como uma região restrita, o código deverá chamar apenas outro código com contratos de confiabilidade forte e o código não deverá alocar nem fazer chamadas virtuais a métodos não preparados ou não confiáveis, a menos que o código esteja preparado para manipular falhas. O CLR atrasa as anulações de thread do código que está sendo executado em uma CER.  

> [!IMPORTANT]
> Só há suporte para CER no .NET Framework. Este artigo não se aplica ao .NET Core ou ao .NET 5 e posterior.

 As regiões de execução restrita são usadas de formas diferentes no CLR, além de um bloco `try` anotado, especialmente, finalizadores críticos executados em classes derivadas da classe <xref:System.Runtime.ConstrainedExecution.CriticalFinalizerObject> e um código executado com o método <xref:System.Runtime.CompilerServices.RuntimeHelpers.ExecuteCodeWithGuaranteedCleanup%2A>.  
  
## <a name="cer-advance-preparation"></a>Preparação antecipada da CER  
 O CLR prepara as CERs com antecedência para evitar condições de memória insuficiente. A preparação antecipada é necessária para que a CLR não cause uma condição de memória insuficiente durante a compilação Just-In-Time ou o carregamento de tipo.  
  
 O desenvolvedor deve indicar que uma região de código é uma CER:  
  
- A região CER de nível superior e os métodos no grafo de chamadas completo que têm o atributo <xref:System.Runtime.ConstrainedExecution.ReliabilityContractAttribute> aplicado são preparados com antecedência. O <xref:System.Runtime.ConstrainedExecution.ReliabilityContractAttribute> somente pode indicar garantias de <xref:System.Runtime.ConstrainedExecution.Cer.Success> ou <xref:System.Runtime.ConstrainedExecution.Cer.MayFail>.  
  
- A preparação antecipada não pode ser executada para chamadas que não podem ser determinadas estaticamente, como a expedição virtual. Use o método <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareMethod%2A> nesses casos. Ao usar o método <xref:System.Runtime.CompilerServices.RuntimeHelpers.ExecuteCodeWithGuaranteedCleanup%2A>, o atributo <xref:System.Runtime.ConstrainedExecution.PrePrepareMethodAttribute> deve ser aplicado ao código de limpeza.  
  
## <a name="constraints"></a>Restrições  
 Os usuários são restritos no tipo de código que podem gravar em uma CER. O código não pode causar uma exceção fora de banda, como as que podem ser o resultado das seguintes operações:  
  
- Alocação explícita.  
  
- Conversão boxing.  
  
- Aquisição de um bloqueio.  
  
- Chamada virtual a métodos não preparados.  
  
- Chamada a métodos com um contrato de confiabilidade fraca ou não existente.  
  
 No .NET Framework versão 2.0, essas restrições são diretrizes. O diagnóstico é fornecido por meio das ferramentas de análise de código.  
  
## <a name="reliability-contracts"></a>Contratos de confiabilidade  
 O <xref:System.Runtime.ConstrainedExecution.ReliabilityContractAttribute> é um atributo personalizado que documenta as garantias de confiabilidade e o estado de corrupção de determinado método.  
  
### <a name="reliability-guarantees"></a>Garantias de confiabilidade  
 As garantias de confiabilidade, representadas pelos valores de enumeração <xref:System.Runtime.ConstrainedExecution.Cer>, indicam o grau de confiabilidade de determinado método:  
  
- <xref:System.Runtime.ConstrainedExecution.Cer.MayFail>. Em condições excepcionais, o método poderá falhar. Nesse caso, o método relata novamente ao método de chamada se ele teve êxito ou falhou. O método deve estar contido em uma CER para garantir que ele possa relatar o valor retornado.  
  
- <xref:System.Runtime.ConstrainedExecution.Cer.None>. O método, tipo ou assembly não tem nenhum conceito de uma CER e, provavelmente, não é seguro de ser chamado em uma CER sem uma mitigação significativa dos danos ao estado. Ele não aproveita as garantias da CER. Isso significa o seguinte:  
  
    1. Em condições excepcionais, o método poderá falhar.  
  
    2. O método poderá ou não relatar sua falha.  
  
    3. O método não é escrito para usar uma CER, o cenário mais provável.  
  
    4. Se um método, tipo ou assembly não for identificado explicitamente para ter êxito, ele será identificado implicitamente como <xref:System.Runtime.ConstrainedExecution.Cer.None>.  
  
- <xref:System.Runtime.ConstrainedExecution.Cer.Success>. Em condições excepcionais, o método tem a garantia de ter êxito. Para atingir esse nível de confiabilidade, você sempre deve construir uma CER em torno do método chamado, mesmo quando ele é chamado em uma região fora da CER. Um método é bem-sucedido se cumpre a finalidade para a qual se destina, embora o êxito possa ser visto de forma subjetiva. Por exemplo, a marcação de Count com `ReliabilityContractAttribute(Cer.Success)` significa que quando ele é executado em uma CER, ele sempre retorna uma contagem do número de elementos no <xref:System.Collections.ArrayList> e ele nunca pode deixar os campos internos em um estado indeterminado.  No entanto, o método <xref:System.Threading.Interlocked.CompareExchange%2A> é marcado como êxito também, com o entendimento de que o êxito pode indicar que o valor não pode ser substituído por um novo valor devido a uma condição de corrida.  O ponto principal é que o método se comporta da maneira que está documentado para se comportar e o código da CER não precisa ser escrito para esperar qualquer comportamento incomum além do que um código correto, mas não confiável, poderá ter.  
  
### <a name="corruption-levels"></a>Níveis de corrupção  
 Os níveis de corrupção, representados pelos valores de enumeração <xref:System.Runtime.ConstrainedExecution.Consistency>, indicam o provável nível de danos ao estado em determinado ambiente:  
  
- <xref:System.Runtime.ConstrainedExecution.Consistency.MayCorruptAppDomain>. Em condições excepcionais, o CLR (Common Language Runtime) não oferece garantias em relação à consistência de estado no domínio do aplicativo atual.  
  
- <xref:System.Runtime.ConstrainedExecution.Consistency.MayCorruptInstance>. Em condições excepcionais, o método tem a garantia de limitar os danos ao estado na instância atual.  
  
- <xref:System.Runtime.ConstrainedExecution.Consistency.MayCorruptProcess>, em condições excepcionais, o CLR não oferece nenhuma garantia em relação à consistência de estado; ou seja, a condição pode corromper o processo.  
  
- <xref:System.Runtime.ConstrainedExecution.Consistency.WillNotCorruptState>. Em condições excepcionais, o método tem a garantia de não corromper o estado.  
  
## <a name="reliability-trycatchfinally"></a>Confiabilidade try/catch/finally  
 A confiabilidade `try/catch/finally` é um mecanismo de tratamento de exceção com o mesmo nível de garantias de previsibilidade da versão não gerenciada. O bloco `catch/finally` é a CER. Os métodos no bloco exigem preparação antecipada e não devem ser interrompíveis.  
  
 No .NET Framework versão 2.0, o código informa o runtime de que um try é confiável chamando <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegions%2A> imediatamente antes de um bloco try. <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegions%2A> é membro do <xref:System.Runtime.CompilerServices.RuntimeHelpers>, uma classe de suporte do compilador. Chame <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegions%2A> diretamente, pendente sua disponibilidade por meio dos compiladores.  
  
## <a name="noninterruptible-regions"></a>Regiões não interrompíveis  
 Uma região não interrompível agrupa um conjunto de instruções em uma CER.  
  
 No .NET Framework versão 2.0, pendente disponibilidade por meio do suporte de compilador, o código do usuário cria regiões não interrompíveis com um try/catch/finally confiável que contém um bloco try/catch vazio precedido por uma chamada de método <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegions%2A>.  
  
## <a name="critical-finalizer-object"></a>Objeto do finalizador crítico  
 Um <xref:System.Runtime.ConstrainedExecution.CriticalFinalizerObject> garante que a coleta de lixo executará o finalizador. Após a alocação, o finalizador e seu grafo de chamadas são preparados com antecedência. O método do finalizador é executado em uma CER e deve respeitar todas as restrições nas CERs e nos finalizadores.  
  
 Os tipos que herdam <xref:System.Runtime.InteropServices.SafeHandle> e <xref:System.Runtime.InteropServices.CriticalHandle> têm a garantia de que seu finalizador será executado em uma CER. Implemente <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> nas classes derivadas <xref:System.Runtime.InteropServices.SafeHandle> para executar qualquer código que deve liberar o identificador.  
  
## <a name="code-not-permitted-in-cers"></a>Código não permitido em CERs  
 As seguintes operações não são permitidas em CERs:  
  
- Alocações explícitas.  
  
- Aquisição de um bloqueio.  
  
- Conversão boxing.  
  
- Acesso de matriz multidimensional.  
  
- Chamadas de método por meio de reflexão.  
  
- <xref:System.Threading.Monitor.Enter%2A> ou <xref:System.IO.FileStream.Lock%2A>.  
  
- Verificações de segurança. Não execute demandas, apenas demandas de link.  
  
- <xref:System.Reflection.Emit.OpCodes.Isinst> e <xref:System.Reflection.Emit.OpCodes.Castclass> para objetos COM e proxies  
  
- Obtenção ou configuração de campos em um proxy transparente.  
  
- Serialização.  
  
- Ponteiros de função e representantes.  
  
## <a name="see-also"></a>Confira também

- [Práticas recomendadas de confiabilidade](reliability-best-practices.md)
