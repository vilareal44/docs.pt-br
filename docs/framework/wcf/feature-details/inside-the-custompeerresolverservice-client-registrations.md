---
title: 'Sobre o CustomPeerResolverService: Registro de clientes'
ms.date: 03/30/2017
ms.assetid: 40236953-a916-4236-84a6-928859e1331a
ms.openlocfilehash: ce694408edbb40309d1750be49b8414ebcbce3f7
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596832"
---
# <a name="inside-the-custompeerresolverservice-client-registrations"></a>Sobre o CustomPeerResolverService: Registro de clientes
Cada nó na malha publica suas informações de ponto de extremidade no serviço de resolvedor por meio da `Register` função. O serviço resolvedor armazena essas informações como um registro de registro. Esse registro contém um identificador exclusivo (RegistrationId) e informações de ponto de extremidade (PeerNodeAddress) para o nó.  
  
## <a name="stale-records-and-expiration-time"></a>Registros obsoletos e tempo de expiração  
 O ideal é que, quando um nó sair da malha, ele chamará a `Unregister` função, o que faz com que o serviço resolvedor remova a entrada de registro. Às vezes, os nós desligam ou se tornam inacessíveis antes `Unregister` de chamar, deixando por trás de um registro de registro obsoleto.  
  
 Registros obsoletos no serviço de resolução podem causar conexões com falha. Se um nó tentando se conectar a uma malha receber informações de conexão obsoletas do serviço resolvedor, poderá levar mais tempo para ingressar a malha com êxito. Registros obsoletos também ocupam memória. Sem um processo de limpeza eficiente, o cache usado para armazenar os registros pode, eventualmente, ser estourado e travar o serviço resolvedor.  
  
 O <xref:System.ServiceModel.PeerResolvers.CustomPeerResolverService> marca cada registro com um tempo de expiração (DateTime) e armazena essas informações como parte do registro. O serviço usa o tempo de expiração para identificar registros obsoletos. Implementações personalizadas devem fazer algo semelhante.  
  
## <a name="refreshinterval-and-cleanupinterval"></a>RefreshInterval e CleanupInterval  
 A `RefreshInterval` propriedade de <xref:System.ServiceModel.PeerResolvers.CustomPeerResolverService> define por quanto tempo os registros de registro permanecem válidos na tabela de pesquisa de registro do serviço. Quando a quantidade de tempo fornecida para essa propriedade foi passada para um determinado registro, esse registro se torna obsoleto e é marcado para exclusão.  
  
 A `CleanupInterval` propriedade de <xref:System.ServiceModel.PeerResolvers.CustomPeerResolverService> informa ao serviço com que frequência Pesquisar e excluir registros de registro obsoletos. O `CleanupInterval` deve ser definido para uma hora maior ou igual ao `RefreshInterval` definido no serviço.  
  
 Para implementar seu próprio serviço resolvedor, você precisa escrever uma função de manutenção para remover registros de registro obsoletos. Há várias maneiras de fazer isso:  
  
- **Manutenção periódica**: defina um temporizador para sair periodicamente e passe pelo armazenamento de dados para excluir registros antigos. O <xref:System.ServiceModel.PeerResolvers.CustomPeerResolverService> usa essa abordagem.  
  
- **Exclusão passiva**: em vez de procurar ativamente registros obsoletos em intervalos regulares, você pode identificar e excluir registros obsoletos quando o serviço já estiver executando outra função. Isso pode retardar o tempo de resposta para solicitações dos clientes resolvedores, mas elimina a necessidade de um temporizador e pode ser mais eficiente se poucos nós esperarem sair sem chamar `Unregister` .  
  
## <a name="registrationlifetime-and-refresh"></a>RegistrationLifetime e atualizar  
 Quando um nó é registrado com um serviço de resolvedor, ele recebe um <xref:System.ServiceModel.PeerResolvers.RegisterResponseInfo> objeto do serviço. Esse objeto tem uma `RegistrationLifetime` propriedade que indica ao nó quanto tempo ele tem antes de o registro expirar e é removido pelo serviço resolvedor. Se, por exemplo, o `RegistrationLifetime` for de 2 minutos, o nó precisará chamar `Refresh` em menos de 2 minutos para garantir que o registro permaneça atualizado e não seja excluído. Quando o serviço resolvedor recebe uma `Refresh` solicitação, ele pesquisa o registro e redefine o tempo de expiração. Atualizar retorna um <xref:System.ServiceModel.PeerResolvers.RefreshResponseInfo> objeto com uma `RegistrationLifetime` propriedade.  
  
## <a name="see-also"></a>Consulte também

- [Resolvedores pares](peer-resolvers.md)
