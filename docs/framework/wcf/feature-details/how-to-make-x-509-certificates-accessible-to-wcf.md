---
title: Como criar certificados X.509 que podem ser acessados pelo WCF
description: Saiba como tornar um certificado X. 509 acessível ao WCF. O código do aplicativo deve especificar o nome e o local do repositório de certificados. Pode haver outros requisitos.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- X.509 certificates [WCF]
- certificates [WCF], making X.509 certificates accessible to WCF
- X.509 certificates [WCF], making accessible to WCF
ms.assetid: a54e407c-c2b5-4319-a648-60e43413664b
ms.openlocfilehash: 5cc1118640bcf1262d88cb8cdb39939ae315cae3
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246864"
---
# <a name="how-to-make-x509-certificates-accessible-to-wcf"></a>Como criar certificados X.509 que podem ser acessados pelo WCF
Para tornar um certificado X. 509 acessível a Windows Communication Foundation (WCF), o código do aplicativo deve especificar o nome e o local do repositório de certificados. Em determinadas circunstâncias, a identidade do processo deve ter acesso ao arquivo que contém a chave privada associada ao certificado X. 509. Para obter a chave privada associada a um certificado X. 509 em um repositório de certificados, o WCF deve ter permissão para fazer isso. Por padrão, somente o proprietário e a conta do sistema podem acessar a chave privada de um certificado.  
  
### <a name="to-make-x509-certificates-accessible-to-wcf"></a>Para tornar os certificados X. 509 acessíveis para o WCF  
  
1. Conceda à conta sob a qual o WCF está executando o acesso de leitura para o arquivo que contém a chave privada associada ao certificado X. 509.  
  
    1. Determine se o WCF requer acesso de leitura à chave privada para o certificado X. 509.  
  
         A tabela a seguir detalha se uma chave privada deve estar disponível ao usar um certificado X. 509.  
  
        |Uso do certificado X. 509|Chave privada|  
        |---------------------------|-----------------|  
        |Assinando digitalmente uma mensagem SOAP de saída.|Yes|  
        |Verificando a assinatura de uma mensagem SOAP de entrada.|No|  
        |Criptografar uma mensagem SOAP de saída.|No|  
        |Descriptografar uma mensagem SOAP de entrada.|Yes|  
  
    2. Determine o local do repositório de certificados e o nome no qual o certificado está armazenado.  
  
         O repositório de certificados no qual o certificado é armazenado é especificado no código do aplicativo ou na configuração. Por exemplo, o exemplo a seguir especifica que o certificado está localizado no `CurrentUser` repositório de certificados chamado `My` .  
  
         [!code-csharp[x509Accessible#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/x509accessible/cs/source.cs#1)]
         [!code-vb[x509Accessible#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/x509accessible/vb/source.vb#1)]  
  
    3. Determine onde a chave privada do certificado está localizada no computador usando a ferramenta [FindPrivateKey](../samples/findprivatekey.md) .  
  
         A ferramenta [FindPrivateKey](../samples/findprivatekey.md) requer o nome do repositório de certificados, o local do repositório de certificados e algo que identifica exclusivamente o certificado. A ferramenta aceita o nome da entidade do certificado ou sua impressão digital como um identificador exclusivo. Para obter mais informações sobre como determinar a impressão digital de um certificado, consulte [como recuperar a impressão digital de um certificado](how-to-retrieve-the-thumbprint-of-a-certificate.md).  
  
         O exemplo de código a seguir usa a ferramenta [FindPrivateKey](../samples/findprivatekey.md) para determinar o local da chave privada de um certificado no `My` repositório `CurrentUser` com uma impressão digital de `46 dd 0e 7a ed 0b 7a 31 9b 02 a3 a0 43 7a d8 3f 60 40 92 9d` .  
  
        ```console
        findprivatekey.exe My CurrentUser -t "46 dd 0e 7a ed 0b 7a 31 9b 02 a3 a0 43 7a d8 3f 60 40 92 9d" -a  
        ```  
  
    4. Determine a conta sob a qual o WCF está sendo executado.  
  
         A tabela a seguir detalha a conta sob a qual o WCF está sendo executado para um determinado cenário.  
  
        |Cenário|Identidade do processo|  
        |--------------|----------------------|  
        |Cliente (aplicativo de console ou WinForms).|Usuário conectado no momento.|  
        |Serviço que é auto-hospedado.|Usuário conectado no momento.|  
        |Serviço hospedado no IIS 6,0 (Windows Server 2003) ou IIS 7,0 (Windows Vista).|SERVIÇO DE REDE|  
        |Serviço hospedado no IIS 5. X (Windows XP).|Controlado pelo `<processModel>` elemento no arquivo de Machine.config. A conta padrão é ASPNET.|  
  
    5. Conceda acesso de leitura ao arquivo que contém a chave privada para a conta em que o WCF está sendo executado, usando uma ferramenta como o icacls.exe.  
  
         O exemplo de código a seguir edita a DACL (lista de controle de acesso discricionário) do arquivo especificado para conceder à conta de serviço de rede acesso de leitura (: R) ao arquivo.  
  
        ```console
        icacls.exe "C:\Documents and Settings\All Users\Application Data\Microsoft\Crypto\RSA\MachineKeys\8aeda5eb81555f14f8f9960745b5a40d_38f7de48-5ee9-452d-8a5a-92789d7110b1" /grant "NETWORK SERVICE":R  
        ```  
  
## <a name="see-also"></a>Veja também

- [FindPrivateKey](../samples/findprivatekey.md)
- [Como recuperar a impressão digital de um certificado](how-to-retrieve-the-thumbprint-of-a-certificate.md)
- [Trabalhando com certificados](working-with-certificates.md)
