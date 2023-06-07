---
title: Como configurar uma porta com um certificado SSL
description: Saiba como configurar uma porta com um certificado X. 509, necessário para um serviço WCF auto-hospedado com a classe WSHttpBinding usando a segurança de transporte.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- SSL
- WCF, security mode
- WCF, security
ms.assetid: b8abcc8e-a5f5-4317-aca5-01e3c40ab24d
ms.openlocfilehash: 0eccdf916dae7b886cbc4e6563e6dfe17039c321
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247176"
---
# <a name="how-to-configure-a-port-with-an-ssl-certificate"></a>Como configurar uma porta com um certificado SSL

Ao criar um serviço de Windows Communication Foundation (WCF) auto-hospedado com a <xref:System.ServiceModel.WSHttpBinding> classe que usa a segurança de transporte, você também deve configurar uma porta com um certificado X. 509. Se você estiver criando um serviço auto-hospedado, você poderá hospedá-lo serviço no IIS (Serviços de Informações da Internet). Para obter mais informações, consulte [segurança de transporte http](http-transport-security.md).  
  
 Para configurar uma porta, a ferramenta usada depende do sistema operacional que está sendo executado no computador.  
  
 Se você estiver executando o Windows Server 2003, use a ferramenta HttpCfg.exe. No Windows Server 2003, essa ferramenta está instalada. Para obter mais informações, consulte a [visão geral do Httpcfg](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc787508(v=ws.10)). A [documentação das ferramentas de suporte do Windows](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc781601(v=ws.10)) explica a sintaxe da ferramenta de Httpcfg.exe.  
  
 Se você estiver executando o Windows Vista, use a ferramenta Netsh.exe que já está instalada.
  
> [!NOTE]
> A modificação de certificados armazenados no computador requer privilégios administrativos.  
  
## <a name="determine-how-ports-are-configured"></a>Determinar como as portas são configuradas  
  
1. No Windows Server 2003 ou no Windows XP, use a ferramenta HttpCfg.exe para exibir a configuração da porta atual, usando os comutadores **SSL** e de **consulta** , conforme mostrado no exemplo a seguir.  
  
    ```console
    httpcfg query ssl  
    ```  
  
2. No Windows Vista, use a ferramenta Netsh.exe para exibir a configuração da porta atual, conforme mostrado no exemplo a seguir.  
  
    ```console  
    netsh http show sslcert  
    ```  
  
## <a name="get-a-certificates-thumbprint"></a>Obter a impressão digital de um certificado  
  
1. Use o snap-in do MMC dos Certificados para localizar um certificado X.509 cuja finalidade seja a autenticação de cliente. Para saber mais, consulte [Como Exibir Certificados com o Snap-in do MMC](how-to-view-certificates-with-the-mmc-snap-in.md).  
  
2. Para acessar a impressão digital do certificado. Para saber mais, confira [Como recuperar a impressão digital de um certificado](how-to-retrieve-the-thumbprint-of-a-certificate.md).  
  
3. Copie a impressão digital do certificado em um editor de texto, como o Bloco de Notas.  
  
4. Remova todos os espaços entre os caracteres hexadecimais. Uma maneira de fazer isso é usar o recurso localizar e substituir do editor de texto e substituir cada espaço com um caractere nulo.  
  
## <a name="bind-an-ssl-certificate-to-a-port-number"></a>Associar um certificado SSL a um número de porta  
  
1. No Windows Server 2003 ou no Windows XP, use a ferramenta HttpCfg.exe no modo "set" no repositório de protocolo SSL (SSL) para associar o certificado a um número de porta. A ferramenta usa a impressão digital para identificar o certificado, conforme mostrado no exemplo o seguir.  
  
    ```console  
    httpcfg set ssl -i 0.0.0.0:8012 -h 0000000000003ed9cd0c315bbb6dc1c08da5e6  
    ```  
  
    - A opção **-i** tem a sintaxe de `IP` : `port` e instrui a ferramenta para definir o certificado para a porta 8012 do computador. Opcionalmente, os quatro zeros que precedem o número também podem ser substituídos pelo endereço IP real do computador.  
  
    - A opção **-h** especifica a impressão digital do certificado.  
  
2. No Windows Vista, use a ferramenta Netsh.exe, conforme mostrado no exemplo a seguir.  
  
    ```console  
    netsh http add sslcert ipport=0.0.0.0:8000 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}
    ```  
  
    - O parâmetro **certhash** especifica a impressão digital do certificado.  
  
    - O parâmetro **ipport** especifica o endereço IP e a porta e funciona exatamente como a opção **-i** da ferramenta de Httpcfg.exe descrita.  
  
    - O parâmetro **AppID** é um GUID que pode ser usado para identificar o aplicativo proprietário.  
  
## <a name="bind-an-ssl-certificate-to-a-port-number-and-support-client-certificates"></a>Associar um certificado SSL a um número de porta e a certificados de cliente de suporte  
  
1. No Windows Server 2003 ou no Windows XP, para dar suporte a clientes que se autenticam com certificados X. 509 na camada de transporte, siga o procedimento anterior, mas passe um parâmetro de linha de comando adicional para HttpCfg.exe, conforme mostrado no exemplo a seguir.  
  
    ```console  
    httpcfg set ssl -i 0.0.0.0:8012 -h 0000000000003ed9cd0c315bbb6dc1c08da5e6 -f 2  
    ```  
  
     A opção **-f** tem a sintaxe de `n` onde n é um número entre 1 e 7. Um valor de 2, conforme mostrado no exemplo anterior, permite certificados do cliente na camada de transporte. Um valor de 3 permite certificados do cliente e mapeia esses certificados para uma conta do Windows. Consulte a Ajuda do HttpCfg.exe para obter o comportamento de outros valores.  
  
2. No Windows Vista, para dar suporte a clientes que se autenticam com certificados X. 509 na camada de transporte, siga o procedimento anterior, mas com um parâmetro adicional, conforme mostrado no exemplo a seguir.  
  
    ```console  
    netsh http add sslcert ipport=0.0.0.0:8000 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF} clientcertnegotiation=enable  
    ```  
  
## <a name="delete-an-ssl-certificate-from-a-port-number"></a>Excluir um certificado SSL de um número de porta  
  
1. Use a ferramenta HttpCfg.exe ou Netsh.exe para ver as portas e as impressões digitais de todas as associações no computador. Para imprimir as informações em disco, use o caractere de redirecionamento ">", conforme mostrado no exemplo a seguir.  
  
    ```console  
    httpcfg query ssl>myMachinePorts.txt  
    ```
  
2. No Windows Server 2003 ou no Windows XP, use a ferramenta HttpCfg.exe com as palavras-chave **delete** e **SSL** . Use a opção **-i** para especificar o `IP` `port` número: e a opção **-h** para especificar a impressão digital.  
  
    ```console  
    httpcfg delete ssl -i 0.0.0.0:8005 -h 0000000000003ed9cd0c315bbb6dc1c08da5e6  
    ```  
  
3. No Windows Vista, use a ferramenta Netsh.exe, conforme mostrado no exemplo a seguir.  
  
    ```console  
    Netsh http delete sslcert ipport=0.0.0.0:8005  
    ```  
  
## <a name="example"></a>Exemplo  

 O código a seguir mostra como criar um serviço auto-hospedado usando o conjunto de classes <xref:System.ServiceModel.WSHttpBinding> para segurança no transporte. Ao criar um aplicativo, especifique o número da porta no endereço.  
  
 [!code-csharp[c_WsHttpService#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_wshttpservice/cs/source.cs#3)]
 [!code-vb[c_WsHttpService#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_wshttpservice/vb/source.vb#3)]  
  
## <a name="see-also"></a>Veja também

- [Segurança de transporte de HTTP](http-transport-security.md)
