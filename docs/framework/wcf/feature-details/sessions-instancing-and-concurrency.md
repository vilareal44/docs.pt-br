---
title: Sessões,instanciação e simultaneidade
description: Saiba mais sobre sessões, instanciação e simultaneidade, como usá-las e as interações entre elas em WFC.
ms.date: 03/30/2017
ms.assetid: 50797a3b-7678-44ed-8138-49ac1602f35b
ms.openlocfilehash: 41eef5a962c702eebd6b9a34607b542ec6bbd97b
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246539"
---
# <a name="sessions-instancing-and-concurrency"></a>Sessões,instanciação e simultaneidade
Uma *sessão* é uma correlação de todas as mensagens enviadas entre dois pontos de extremidade. A *instanciação* refere-se ao controle do tempo de vida de objetos de serviço definidos pelo usuário e seus <xref:System.ServiceModel.InstanceContext> objetos relacionados. *Simultaneidade* é o termo dado ao controle do número de threads em execução em um <xref:System.ServiceModel.InstanceContext> ao mesmo tempo.  
  
 Este tópico descreve essas configurações, como usá-las e as várias interações entre elas.  
  
## <a name="sessions"></a>Sessões  
 Quando um contrato de serviço define a <xref:System.ServiceModel.ServiceContractAttribute.SessionMode%2A?displayProperty=nameWithType> propriedade como <xref:System.ServiceModel.SessionMode.Required?displayProperty=nameWithType> , esse contrato diz que todas as chamadas (ou seja, as trocas de mensagens subjacentes que dão suporte às chamadas) devem fazer parte da mesma conversa. Se um contrato especifica que ele permite sessões, mas não requer uma, os clientes podem se conectar e estabelecer uma sessão ou não. Se a sessão terminar e uma mensagem for enviada pelo mesmo canal baseado em sessão, uma exceção será lançada.  
  
 As sessões do WCF têm os seguintes recursos conceituais principais:  
  
- Elas são explicitamente iniciadas e terminadas pelo aplicativo de chamada.  
  
- As mensagens entregues durante uma sessão são processadas na ordem em que são recebidas.  
  
- As sessões correlacionam um grupo de mensagens em uma conversa. O significado dessa correlação é uma abstração. Por exemplo, um canal baseado em sessão pode correlacionar mensagens com base em uma conexão de rede compartilhada enquanto outro canal baseado em sessão pode correlacionar mensagens com base em uma marca compartilhada no corpo da mensagem. Os recursos que podem ser derivados da sessão dependem da natureza da correlação.  
  
- Não há nenhum repositório de dados geral associado a uma sessão WCF.  
  
 Se você estiver familiarizado com a <xref:System.Web.SessionState.HttpSessionState?displayProperty=nameWithType> classe em aplicativos ASP.net e a funcionalidade que ele fornece, você poderá observar as seguintes diferenças entre esse tipo de sessão e sessões do WCF:  
  
- As sessões ASP.NET são sempre iniciadas pelo servidor.  
  
- As sessões ASP.NET são implicitamente desordenadas.  
  
- As sessões ASP.NET fornecem um mecanismo de armazenamento de dados geral entre solicitações.  
  
 Aplicativos cliente e aplicativos de serviço interagem com sessões de maneiras diferentes. Os aplicativos cliente iniciam sessões e recebem e processam as mensagens enviadas dentro da sessão. Os aplicativos de serviço podem usar sessões como um ponto de extensibilidade para adicionar um comportamento adicional. Isso é feito trabalhando diretamente com o <xref:System.ServiceModel.InstanceContext> ou implementando um provedor de contexto de instância personalizada.  
  
## <a name="instancing"></a>Instanciação  
 O comportamento de instanciação (definido usando a <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> Propriedade) controla como o <xref:System.ServiceModel.InstanceContext> é criado em resposta a mensagens de entrada. Por padrão, cada <xref:System.ServiceModel.InstanceContext> um é associado a um objeto de serviço definido pelo usuário, portanto (no caso padrão) a configuração da <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A> propriedade também controla a instanciação de objetos de serviço definidos pelo usuário. A <xref:System.ServiceModel.InstanceContextMode> enumeração define os modos de instanciação.  
  
 Os seguintes modos de instanciação estão disponíveis:  
  
- <xref:System.ServiceModel.InstanceContextMode.PerCall>: Um novo <xref:System.ServiceModel.InstanceContext> (e, portanto, objeto de serviço) é criado para cada solicitação do cliente.  
  
- <xref:System.ServiceModel.InstanceContextMode.PerSession>: Um novo <xref:System.ServiceModel.InstanceContext> (e, portanto, objeto de serviço) é criado para cada nova sessão de cliente e mantido durante o tempo de vida dessa sessão (isso requer uma associação que dá suporte a sessões).  
  
- <xref:System.ServiceModel.InstanceContextMode.Single>: Um único <xref:System.ServiceModel.InstanceContext> (e, portanto, o objeto de serviço) trata todas as solicitações de cliente durante o tempo de vida do aplicativo.  
  
 O exemplo de código a seguir mostra o <xref:System.ServiceModel.InstanceContextMode> valor padrão, <xref:System.ServiceModel.InstanceContextMode.PerSession> sendo explicitamente definido em uma classe de serviço.  
  
```csharp  
[ServiceBehavior(InstanceContextMode=InstanceContextMode.PerSession)]
public class CalculatorService : ICalculatorInstance
{
    ...  
}  
```  
  
 E enquanto a <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> propriedade controla com que frequência o <xref:System.ServiceModel.InstanceContext> é liberado, as <xref:System.ServiceModel.OperationBehaviorAttribute.ReleaseInstanceMode%2A?displayProperty=nameWithType> Propriedades e são <xref:System.ServiceModel.ServiceBehaviorAttribute.ReleaseServiceInstanceOnTransactionComplete%2A?displayProperty=nameWithType> controladas quando o objeto de serviço é liberado.  
  
### <a name="well-known-singleton-services"></a>Serviços singleton conhecidos  
 Às vezes, uma variação em objetos de serviço de instância única é útil: você pode criar um objeto de serviço por conta própria e criar o host de serviço usando esse objeto. Para fazer isso, você também deve definir a <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> propriedade como <xref:System.ServiceModel.InstanceContextMode.Single> ou uma exceção é lançada quando o host de serviço é aberto.  
  
 Use o <xref:System.ServiceModel.ServiceHost.%23ctor%28System.Object%2CSystem.Uri%5B%5D%29> construtor para criar um serviço desse tipo. Ele fornece uma alternativa à implementação de um personalizado <xref:System.ServiceModel.Dispatcher.IInstanceContextInitializer?displayProperty=nameWithType> quando você deseja fornecer uma instância de objeto específica para uso por um serviço singleton. Você pode usar essa sobrecarga quando o tipo de implementação de serviço for difícil de construir (por exemplo, se ele não implementar um construtor público sem parâmetros).  
  
 Observe que, quando um objeto é fornecido a esse construtor, alguns recursos relacionados ao comportamento de instanciação do Windows Communication Foundation (WCF) funcionam de maneira diferente. Por exemplo, <xref:System.ServiceModel.InstanceContext.ReleaseServiceInstance%2A?displayProperty=nameWithType> a chamada não tem efeito quando uma instância de objeto singleton é fornecida. Da mesma forma, qualquer outro mecanismo de liberação de instância é ignorado. O <xref:System.ServiceModel.ServiceHost> sempre se comporta como se a <xref:System.ServiceModel.OperationBehaviorAttribute.ReleaseInstanceMode%2A?displayProperty=nameWithType> Propriedade fosse definida como <xref:System.ServiceModel.ReleaseInstanceMode.None?displayProperty=nameWithType> para todas as operações.  
  
### <a name="sharing-instancecontext-objects"></a>Compartilhando objetos InstanceContext  
 Você também pode controlar qual canal ou chamada de sessão está associado ao <xref:System.ServiceModel.InstanceContext> objeto executando a associação por conta própria.  
  
## <a name="concurrency"></a>Simultaneidade  
 Simultaneidade é o controle do número de threads ativos em um <xref:System.ServiceModel.InstanceContext> de cada vez. Isso é controlado usando o <xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A?displayProperty=nameWithType> com a <xref:System.ServiceModel.ConcurrencyMode> enumeração.  
  
 Os três modos de simultaneidade a seguir estão disponíveis:  
  
- <xref:System.ServiceModel.ConcurrencyMode.Single>: Cada contexto de instância tem permissão para ter um máximo de um thread processando mensagens no contexto de instância por vez. Outros threads que desejam usar o mesmo contexto de instância devem ser bloqueados até que o thread original saia do contexto da instância.  
  
- <xref:System.ServiceModel.ConcurrencyMode.Multiple>: Cada instância de serviço pode ter vários threads processando mensagens simultaneamente. A implementação do serviço deve ser thread-safe para usar esse modo de simultaneidade.  
  
- <xref:System.ServiceModel.ConcurrencyMode.Reentrant>: Cada instância de serviço processa uma mensagem por vez, mas aceita chamadas de operação reentrante. O serviço só aceita essas chamadas quando está chamando por meio de um objeto de cliente WCF.  
  
> [!NOTE]
> Compreender e desenvolver código que usa com segurança mais de um thread pode ser difícil de gravar com êxito. Antes <xref:System.ServiceModel.ConcurrencyMode.Multiple> de usar <xref:System.ServiceModel.ConcurrencyMode.Reentrant> os valores ou, verifique se o serviço foi projetado corretamente para esses modos. Para obter mais informações, consulte <xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A>.  
  
 O uso da simultaneidade está relacionado ao modo de instanciação. Em <xref:System.ServiceModel.InstanceContextMode.PerCall> instanciação, a simultaneidade não é relevante, pois cada mensagem é processada por um novo <xref:System.ServiceModel.InstanceContext> e, portanto, nunca mais de um thread está ativa no <xref:System.ServiceModel.InstanceContext> .  
  
 O exemplo de código a seguir demonstra como definir a <xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A> propriedade como <xref:System.ServiceModel.ConcurrencyMode.Multiple> .  
  
```csharp
[ServiceBehavior(ConcurrencyMode=ConcurrencyMode.Multiple, InstanceContextMode = InstanceContextMode.Single)]
public class CalculatorService : ICalculatorConcurrency
{
    ...  
}  
```  
  
## <a name="sessions-interact-with-instancecontext-settings"></a>Sessões interagem com configurações de InstanceContext  
 As sessões e <xref:System.ServiceModel.InstanceContext> interagem dependendo da combinação do valor da <xref:System.ServiceModel.SessionMode> enumeração em um contrato e da <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> Propriedade na implementação do serviço, que controla a associação entre canais e objetos de serviço específicos.  
  
 A tabela a seguir mostra o resultado de um canal de entrada que dá suporte a sessões ou não suporte a sessões dadas a combinação de valores da propriedade e da propriedade por um serviço <xref:System.ServiceModel.ServiceContractAttribute.SessionMode%2A?displayProperty=nameWithType> <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> .  
  
|Valor de InstanceContextmode|<xref:System.ServiceModel.SessionMode.Required>|<xref:System.ServiceModel.SessionMode.Allowed>|<xref:System.ServiceModel.SessionMode.NotAllowed>|  
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|  
|PerCall|-Comportamento com o canal de sessão: uma sessão e <xref:System.ServiceModel.InstanceContext> para cada chamada.<br />-Comportamento com canal sem sessão: uma exceção é lançada.|-Comportamento com o canal de sessão: uma sessão e <xref:System.ServiceModel.InstanceContext> para cada chamada.<br />-Comportamento com canal sem sessão: um <xref:System.ServiceModel.InstanceContext> para cada chamada.|-Comportamento com o canal de sessão: uma exceção é lançada.<br />-Comportamento com canal sem sessão: um <xref:System.ServiceModel.InstanceContext> para cada chamada.|  
|PerSession|-Comportamento com o canal de sessão: uma sessão e <xref:System.ServiceModel.InstanceContext> para cada canal.<br />-Comportamento com canal sem sessão: uma exceção é lançada.|-Comportamento com o canal de sessão: uma sessão e <xref:System.ServiceModel.InstanceContext> para cada canal.<br />-Comportamento com canal sem sessão: um <xref:System.ServiceModel.InstanceContext> para cada chamada.|-Comportamento com o canal de sessão: uma exceção é lançada.<br />-Comportamento com canal sem sessão: um <xref:System.ServiceModel.InstanceContext> para cada chamada.|  
|Single|-Comportamento com o canal de sessão: uma sessão e outra <xref:System.ServiceModel.InstanceContext> para todas as chamadas.<br />-Comportamento com canal sem sessão: uma exceção é lançada.|-Comportamento com o canal de sessão: uma sessão e <xref:System.ServiceModel.InstanceContext> para o singleton criado ou especificado pelo usuário.<br />-Comportamento com canal sem sessão: um <xref:System.ServiceModel.InstanceContext> para o singleton criado ou especificado pelo usuário.|-Comportamento com o canal de sessão: uma exceção é lançada.<br />-Comportamento com canal sem sessão: um <xref:System.ServiceModel.InstanceContext> para cada singleton criado ou para o singleton especificado pelo usuário.|  
  
## <a name="see-also"></a>Veja também

- [Usando sessões](../using-sessions.md)
- [Como criar um serviço que requer sessões](how-to-create-a-service-that-requires-sessions.md)
- [Como controlar instanciação de serviço](how-to-control-service-instancing.md)
- [Simultaneidade](../samples/concurrency.md)
- [Instanciação](../samples/instancing.md)
- [Sessão](../samples/session.md)
