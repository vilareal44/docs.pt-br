---
title: Como proteger mensagens em sessões confiáveis
ms.date: 03/30/2017
ms.assetid: aee33e50-936f-4486-9ca8-c1520c19a62d
ms.openlocfilehash: 306d0f96b5163fe5c24d270b4b9a7c1d3f499e7e
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596949"
---
# <a name="how-to-secure-messages-within-reliable-sessions"></a>Como proteger mensagens em sessões confiáveis

Este tópico descreve as etapas necessárias para habilitar a segurança em nível de mensagem para mensagens trocadas em uma sessão confiável usando uma das associações fornecidas pelo sistema que dão suporte a essa sessão, mas não por padrão. Habilite uma sessão segura e confiável de forma imperativa usando código ou declarativamente no arquivo de configuração. Este procedimento usa os arquivos de configuração de cliente e serviço para habilitar a sessão segura e confiável.

Esse procedimento consiste nas seguintes três tarefas principais:

1. Especifique que o cliente e o serviço trocam mensagens em uma sessão confiável.

1. Exigir segurança em nível de mensagem na sessão confiável.

1. Especifique o tipo de credencial do cliente que o cliente deve usar para ser autenticado com o serviço.

É importante na primeira tarefa que o elemento de configuração do ponto de extremidade contém um `bindingConfiguration` atributo que faz referência a uma configuração de associação chamada (neste exemplo) `MessageSecurity` . O [**\<binding>**](../../configure-apps/file-schema/wcf/bindings.md) elemento de configuração, em seguida, faz referência a esse nome para habilitar sessões confiáveis definindo o `enabled` atributo do [**\<reliableSession>**](https://docs.microsoft.com/previous-versions/ms731375(v=vs.90)) elemento como `true` . Você pode exigir que as garantias de entrega ordenadas estejam disponíveis em uma sessão confiável definindo o `ordered` atributo como `true` .

Para a cópia de origem do exemplo no qual esse procedimento de configuração se baseia, consulte a [sessão confiável do WS](../samples/ws-reliable-session.md).

Os itens essenciais da segunda tarefa são realizados definindo o `mode` atributo do **\<security>** elemento contido no **\<binding>** elemento do cliente e do serviço como `Message` .

Os itens essenciais da terceira tarefa são realizados definindo o `clientCredentialType` atributo do **\<message>** elemento contido no **\<security>** elemento do cliente e do serviço como `Certificate` .

> [!NOTE]
> Ao usar a segurança de mensagem com sessões confiáveis, o sistema de mensagens confiável tenta autenticar um cliente não autenticado até que ocorra um tempo limite em vez de lançar uma exceção na primeira falha.

### <a name="configure-the-service-with-a-wshttpbinding-to-use-a-reliable-session"></a>Configurar o serviço com uma WSHttpBinding para usar uma sessão confiável

Este procedimento é descrito em [como trocar mensagens em uma sessão confiável](how-to-exchange-messages-within-a-reliable-session.md).

### <a name="configure-the-client-with-a-wshttpbinding-to-use-a-reliable-session"></a>Configurar o cliente com um WSHttpBinding para usar uma sessão confiável

Este procedimento é descrito em [como trocar mensagens em uma sessão confiável](how-to-exchange-messages-within-a-reliable-session.md).

### <a name="set-the-mode-and-clientcredentialtype-in-configuration"></a>Definir o modo e ClientCredentialtype na configuração

1. Adicione um elemento de associação apropriado ao [**\<bindings>**](../../configure-apps/file-schema/wcf/bindings.md) elemento do arquivo de configuração. O exemplo a seguir adiciona um [**\<wsHttpBinding>**](../../configure-apps/file-schema/wcf/wshttpbinding.md) elemento.

1. Adicione um **\<binding>** elemento e defina seu `name` atributo como um valor apropriado. O exemplo usa o nome `MessageSecurity` .

1. Adicione um **\<security>** elemento e defina o `mode` atributo como `Message` .

1. Dentro do **\<security>** elemento, adicione um **\<message>** elemento e defina o `clientCredentialType` atributo como `Certificate` .

```xml
<wsHttpBinding>
  <binding name="MessageSecurity">
    <security mode="Message">
      <message clientCredentialType="Certificate" />
    </security>
  </binding>
</wsHttpBinding>
```
