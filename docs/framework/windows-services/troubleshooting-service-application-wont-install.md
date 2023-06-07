---
title: 'Solução de problemas: O aplicativo de serviço não é instalado'
description: Solucionar problemas se o aplicativo de serviço não for instalado. Certifique-se de que a propriedade ServiceName da classe de serviço esteja definida corretamente.
ms.date: 03/30/2017
helpviewer_keywords:
- troubleshooting service applications
- services, troubleshooting
- services, debugging
- Windows NT services, troubleshooting
- troubleshooting NT services
- Windows Service applications, troubleshooting
ms.assetid: 45c48e2e-b97d-44bc-8896-14f328e0ce33
author: ghogen
ms.openlocfilehash: 4a57fb6975a6ded48abf7c8fd7eacec16e4f94d8
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925521"
---
# <a name="troubleshooting-service-application-wont-install"></a>Solução de problemas: O aplicativo de serviço não é instalado
Se o aplicativo de serviço não for instalado corretamente, verifique se a propriedade <xref:System.ServiceProcess.ServiceBase.ServiceName%2A> da classe de serviço está definida com o mesmo valor que é mostrado no instalador do serviço. O valor precisa ser o mesmo em ambas as instâncias para que o serviço seja instalado corretamente.  
  
> [!NOTE]
> Você também pode examinar os logs de instalação para obter comentários sobre o processo de instalação.  
  
 Você também deve verificar se você tem outro serviço com o mesmo nome já está instalado. Os nomes de serviço precisam ser exclusivos para que a instalação tenha êxito.  
  
## <a name="see-also"></a>Confira também

- [Introdução a aplicativos do Serviço Windows](introduction-to-windows-service-applications.md)
