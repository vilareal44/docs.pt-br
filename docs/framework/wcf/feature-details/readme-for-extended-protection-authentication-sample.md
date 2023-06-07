---
title: LeiaMe para exemplo de autenticação de proteção estendida
ms.date: 03/30/2017
ms.assetid: 80bf2e97-398d-4db5-9040-d96478a2ccab
ms.openlocfilehash: 9b0a3535282a1fcc1103651f5601459e80d3d8d4
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84601095"
---
# <a name="readme-for-extended-protection-authentication-sample"></a>LeiaMe para exemplo de autenticação de proteção estendida

A proteção estendida é uma iniciativa de segurança para proteger contra ataques MITM (Man-in-the-Middle), em que um invasor (o "Man-in-the-middle") intercepta as credenciais de um cliente e as usa para acessar recursos seguros no servidor pretendido do cliente.

Para obter mais informações, consulte [proteção estendida para autenticação visão geral](extended-protection-for-authentication-overview.md).

> [!NOTE]
> Este exemplo só funciona quando hospedado no IIS. Ele não funciona em Visual Studio Development Server porque isso não oferece suporte a HTTPS.

## <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar e executar o exemplo

1. Instale o IIS no computador por meio de adicionar/remover programas-> recursos do Windows.

2. Ative a autenticação do Windows em recursos do Windows: Serviços de Informações da Internet-> serviços de World Wide Web-> segurança-> autenticação do Windows.

3. Ative a ativação HTTP nos recursos do Windows: Microsoft .NET Framework 3.5.1-> Windows Communication Foundation ativação HTTP.

4. Este exemplo requer que o cliente estabeleça um canal seguro com o servidor e, por isso, ele requer a presença de um certificado de servidor que pode ser instalado no Gerenciador Serviços de Informações da Internet (IIS).

    1. Abra o Gerenciador do IIS-> certificados do servidor (na guia exibição de recurso).

    2. Para testar esse exemplo, você pode criar um certificado autoassinado. (Se você não quiser que o Internet Explorer solicite que o certificado não seja seguro, você poderá instalá-lo no repositório de autoridade raiz do certificado confiável).

5. Vá para o painel Ações do site da Web padrão. Clique em Editar associações de > de site. Adicione HTTPS como um tipo se ele ainda não estiver presente, com o número da porta 443 e atribua o certificado SSL criado na etapa anterior.

6. Crie o serviço. Isso cria um diretório virtual no IIS para você (a partir da ação de Build especificada nas propriedades do projeto) e copia os arquivos dll,. svc e config conforme necessário para que um serviço seja hospedado na Web.

7. Abra o Gerenciador do IIS. Clique com o botão direito do mouse no diretório virtual (ExtendedProtection) que você criou na etapa anterior e selecione converter para aplicativo.

8. Abra o módulo de autenticação no Gerenciador do IIS para este diretório virtual e habilite a autenticação do Windows.

9. Abra as configurações avançadas para autenticação do Windows para este diretório virtual e defina-o como obrigatório, já que, no exemplo, a configuração ExtendedProtection correspondente é definida como Always.

10. Você pode testar o serviço acessando a URL em uma janela do navegador. Se você quiser acessar essa URL de um computador cruzado, certifique-se de que o firewall esteja aberto para todas as conexões HTTP e HTTPS de entrada.

11. Abra o arquivo de configuração do cliente e forneça um nome de máquina completo para o \<client>  -  \<endpoint> atributo-address, substituindo \<full_machine_name> .

12. Execute o cliente. O cliente se comunica com o serviço estabelecendo um canal seguro e usando a proteção estendida nos bastidores.
