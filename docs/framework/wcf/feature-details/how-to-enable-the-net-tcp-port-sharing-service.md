---
title: Como habilitar o serviço de compartilhamento de porta Net.TCP
description: Saiba como configurar o serviço de compartilhamento de porta TCP NET usando o MMC para habilitar net. TCP, que está desabilitado por padrão.
ms.date: 03/30/2017
helpviewer_keywords:
- port sharing [WCF]
- activation services [WCF]
ms.assetid: c9175af4-c27c-4765-bf45-b8f7528a7282
ms.openlocfilehash: 0292559e3befde7f0b00b36aa10a2d9615daf049
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246994"
---
# <a name="how-to-enable-the-nettcp-port-sharing-service"></a>Como habilitar o serviço de compartilhamento de porta Net.TCP
O Windows Communication Foundation (WCF) usa um serviço do Windows chamado serviço de compartilhamento de porta Net. TCP para facilitar o compartilhamento de portas TCP em vários processos. Esse serviço é instalado como parte do WCF, mas o serviço não é habilitado por padrão como uma precaução de segurança e, portanto, deve ser habilitado manualmente antes do primeiro uso. Este tópico descreve como configurar o serviço de compartilhamento de porta TCP NET usando o snap-in do MMC (console de gerenciamento Microsoft).  
  
 Depois de habilitar o serviço de compartilhamento de porta Net. TCP e iniciá-lo manualmente, consulte [como: configurar um serviço WCF para usar o compartilhamento de porta](how-to-configure-a-wcf-service-to-use-port-sharing.md) para obter informações sobre como configurar seu serviço para usar esse serviço.  
  
 Para obter um exemplo que usa net. TCP://compartilhamento de porta, consulte o [exemplo de compartilhamento de porta Net. TCP](../samples/net-tcp-port-sharing-sample.md).  
  
### <a name="to-enable-the-nettcp-port-sharing-service-using-mmc"></a>Para habilitar o serviço de compartilhamento de porta Net. TCP usando o MMC  
  
1. No menu Iniciar, abra o console de gerenciamento de serviços abrindo uma janela de prompt de comando e digitando `services.msc` ou abrindo executar e digitando `services.msc` na caixa abrir.  
  
2. Na coluna **nome** da lista de serviços, clique com o botão direito do mouse no **serviço de compartilhamento de porta Net. TCP**e selecione **Propriedades** no menu.  
  
3. Para habilitar a inicialização manual do serviço, na janela **Propriedades** , selecione a guia **geral** e, na caixa **tipo de inicialização** , selecione manual e, em seguida, clique em **aplicar**.  
  
4. Para iniciar o serviço, na área status do serviço, clique no botão **Iniciar** . O status do serviço agora deve exibir "iniciado".  
  
5. Para retornar à lista de serviços, clique em **OK**e saia do console do MMC.  
  
## <a name="example"></a>Exemplo  
  
## <a name="see-also"></a>Confira também

- [Compartilhamento de porta Net.TCP](net-tcp-port-sharing.md)
- [Configurando o serviço de compartilhamento de porta Net.TCP](configuring-the-net-tcp-port-sharing-service.md)
