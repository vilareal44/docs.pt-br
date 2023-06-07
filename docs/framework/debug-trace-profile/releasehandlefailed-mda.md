---
title: MDA releaseHandleFailed
description: Examine o MDA (Assistente de depuração gerenciada) releaseHandleFailed, que pode ser ativado devido a vazamentos de recursos ou de memória no .NET.
ms.date: 03/30/2017
helpviewer_keywords:
- managed debugging assistants (MDAs), handles
- release handle failed
- CriticalHandle class, run-time errors
- releaseHandleFailed MDA
- ReleaseHandle method
- SafeHandle class, run-time errors
- MDAs (managed debugging assistants), handles
ms.assetid: 44cd98ba-95e5-40a1-874d-e8e163612c51
ms.openlocfilehash: 167a304b4571aa35f758a2054caf6ae1c60a3c60
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85803632"
---
# <a name="releasehandlefailed-mda"></a>MDA releaseHandleFailed
O MDA (Assistente de Depuração Gerenciado) de `releaseHandleFailed` é ativado é notificar os desenvolvedores quando o método <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> de uma classe derivada de <xref:System.Runtime.InteropServices.SafeHandle> ou <xref:System.Runtime.InteropServices.CriticalHandle> retorna `false`.  
  
## <a name="symptoms"></a>Sintomas  
 Perdas de memória ou de recursos.  Se o método <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> da classe derivada de <xref:System.Runtime.InteropServices.SafeHandle> ou <xref:System.Runtime.InteropServices.CriticalHandle> falhar, então o recurso encapsulado pela classe talvez não tenha sido liberado ou limpo.  
  
## <a name="cause"></a>Causa  
 Os usuários devem fornecer a implementação do método <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> se eles criarem classes que derivam de <xref:System.Runtime.InteropServices.SafeHandle> ou <xref:System.Runtime.InteropServices.CriticalHandle>; assim, as circunstâncias são específicas do recurso individual. No entanto, os requisitos de configuração são os seguintes:  
  
- Os tipos <xref:System.Runtime.InteropServices.SafeHandle> e <xref:System.Runtime.InteropServices.CriticalHandle> representam wrappers em torno de recursos vitais do processo. Uma perda de memória inutilizaria o processo ao longo do tempo.  
  
- O método <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> não deve falhar ao executar sua função. Depois que o processo adquire um recurso desse tipo, <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> é a única maneira de liberá-lo. Portanto, a falha implica em perda de recursos.  
  
- Qualquer falha que ocorra durante a execução de <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A>, impedindo a liberação do recurso, é um bug na implementação do método <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> em si. É responsabilidade do programador garantir que o contrato seja atendido, mesmo que esse código, para executar sua função, chame código criado por outra pessoa.  
  
## <a name="resolution"></a>Resolução  
 O código que usa o tipo <xref:System.Runtime.InteropServices.SafeHandle> específico (ou <xref:System.Runtime.InteropServices.CriticalHandle>) que gerou a notificação de MDA deve ser revisado, procurando os locais em que o valor do identificador bruto é extraído do <xref:System.Runtime.InteropServices.SafeHandle> e copiado em outro lugar. Essa é a causa comum de falhas em implementações de <xref:System.Runtime.InteropServices.SafeHandle> ou <xref:System.Runtime.InteropServices.CriticalHandle>, porque o uso do valor do identificador bruto não é mais controlado pelo runtime. Se a cópia de identificador bruto subsequentemente for fechada, isso poderá causar falha em uma chamada <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> posterior porque a tentativa de fechar ocorrerá no mesmo identificador, que será então inválido.  
  
 Há várias maneiras em que a duplicação de identificador incorreto pode ocorrer:  
  
- Procure por chamadas para o método <xref:System.Runtime.InteropServices.SafeHandle.DangerousGetHandle%2A>. Chamadas para esse método deverão ser muito raras e qualquer uma que você encontrar deverá ficar entre chamadas para os métodos <xref:System.Runtime.InteropServices.SafeHandle.DangerousAddRef%2A> e <xref:System.Runtime.InteropServices.SafeHandle.DangerousRelease%2A>. Esses métodos especificam a região do código na qual o valor do identificador bruto pode ser usado com segurança. Fora dessa região ou se a contagem de referência nunca chegar a ser incrementada, o valor do identificador poderá ser invalidado a qualquer momento por uma chamada para <xref:System.Runtime.InteropServices.SafeHandle.Dispose%2A> ou <xref:System.Runtime.InteropServices.SafeHandle.Close%2A> em outro thread. Uma vez que todos os usos de <xref:System.Runtime.InteropServices.SafeHandle.DangerousGetHandle%2A> tiverem sido identificados, você deverá percorrer o caminho seguido pelo identificador bruto para assegurar que ele não seja cedido a nenhum componente que, eventualmente, chame `CloseHandle` ou outro método nativo de baixo nível que libere o identificador.  
  
- Verifique se o código usado para inicializar o <xref:System.Runtime.InteropServices.SafeHandle> com um valor de identificador bruto válido é proprietário do identificador. Se você formar um <xref:System.Runtime.InteropServices.SafeHandle> em torno de um identificador que não for propriedade de seu código sem configurar o parâmetro `ownsHandle` como `false` no construtor base, então ambos o <xref:System.Runtime.InteropServices.SafeHandle> e o proprietário do identificador real poderão tentar fechar o identificador, levando a um erro em <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> se o <xref:System.Runtime.InteropServices.SafeHandle> perder a corrida.  
  
- Quando um <xref:System.Runtime.InteropServices.SafeHandle> sofre marshaling entre domínios de aplicativo, confirme que a derivação <xref:System.Runtime.InteropServices.SafeHandle> sendo usada foi marcada como serializável. Nos raros casos em que uma classe derivada de <xref:System.Runtime.InteropServices.SafeHandle> foi tornada serializável, ela deve implementar a interface <xref:System.Runtime.Serialization.ISerializable> ou usar uma das outras técnicas para controlar o processo de serialização e desserialização manualmente. Isso é necessário porque a ação de serialização padrão é criar um clone bit a bit do valor de identificador bruto anexado, o que resulta em duas instâncias <xref:System.Runtime.InteropServices.SafeHandle> pensando que são proprietárias do mesmo identificador. Ambas tentarão chamar <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> no mesmo identificador em algum momento. O segundo <xref:System.Runtime.InteropServices.SafeHandle> a fazer isso falhará. A maneira correta de serializar um <xref:System.Runtime.InteropServices.SafeHandle> é chamar a função `DuplicateHandle` ou uma função semelhante para seu tipo de identificador nativo fazer uma cópia de identificador legal distinta. Se o tipo de identificador não dá suporte a isso, então o tipo <xref:System.Runtime.InteropServices.SafeHandle> encapsulando-o não pode ser transformado em serializável.  
  
- É possível acompanhar casos em que um identificador está sendo fechado precocemente, o que leva a uma falha quando o método <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> finalmente é chamado, colocando-se um ponto de interrupção do depurador na rotina nativa usada para liberar o identificador, por exemplo, a função `CloseHandle`. Isso talvez não seja possível para cenários de estresse ou até mesmo testes funcionais de tamanho médio em razão do tráfego intenso com que essas rotinas costumam lidar. Talvez seja útil instrumentar o código que chama o método de liberação nativo para capturar a identidade do chamador ou, possivelmente, um rastreamento de pilha completo e o valor do identificador sendo liberado.  O valor do identificador pode ser comparado com o valor indicado por esse MDA.  
  
- Observe que alguns tipos de identificador nativo que podem ser liberados por meio da função `CloseHandle`, tais como Win32, compartilham o mesmo namespace de identificador. Uma liberação incorreta de um tipo de identificador pode causar problemas com outro. Por exemplo, fechar acidentalmente um identificador de evento Win32 duas vezes pode resultar no fechamento prematuro de um identificador de arquivo aparentemente não relacionado. Isso ocorre quando o identificador é liberado e o valor dele se torna disponível para ser usado para acompanhar outro recurso, potencialmente de outro tipo. Se isso ocorre e é seguido por uma segunda liberação errônea, o identificador de um thread não relacionado pode invalidado.  
  
## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime  
 Esse MDA não tem efeito sobre o CLR.  
  
## <a name="output"></a>Saída  
 Uma mensagem indicando que um <xref:System.Runtime.InteropServices.SafeHandle> ou <xref:System.Runtime.InteropServices.CriticalHandle> falhou em liberar o identificador. Por exemplo:  
  
```output
"A SafeHandle or CriticalHandle of type 'MyBrokenSafeHandle'
failed to properly release the handle with value 0x0000BEEF. This
usually indicates that the handle was released incorrectly via
another means (such as extracting the handle using DangerousGetHandle
and closing it directly or building another SafeHandle around it."  
```  
  
## <a name="configuration"></a>Configuração  
  
```xml  
<mdaConfig>  
  <assistants>  
    <releaseHandleFailed/>  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="example"></a>Exemplo  
 A seguir está um exemplo de código que pode ativar o MDA `releaseHandleFailed`.  
  
```csharp
bool ReleaseHandle()  
{  
    // Calling the Win32 CloseHandle function to release the
    // native handle wrapped by this SafeHandle. This method returns
    // false on failure, but should only fail if the input is invalid
    // (which should not happen here). The method specifically must not
    // fail simply because of lack of resources or other transient
    // failures beyond the user’s control. That would make it unacceptable
    // to call CloseHandle as part of the implementation of this method.  
    return CloseHandle(handle);  
}  
```  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
- [Realizando marshaling de interoperabilidade](../interop/interop-marshaling.md)
