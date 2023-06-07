---
title: Como criar certificados temporários para uso durante o desenvolvimento
description: Saiba como usar um cmdlet do PowerShell para criar dois certificados X. 509 temporários para uso no desenvolvimento de um cliente ou serviço WCF seguro.
ms.date: 03/30/2017
helpviewer_keywords:
- certificates [WCF], creating temporary certificates
- temporary certificates [WCF]
ms.assetid: bc5f6637-5513-4d27-99bb-51aad7741e4a
ms.openlocfilehash: 0a21548386639a9f6a8c8572e5d7928ffdb270d6
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247033"
---
# <a name="how-to-create-temporary-certificates-for-use-during-development"></a>Como criar certificados temporários para uso durante o desenvolvimento

Ao desenvolver um serviço ou cliente seguro usando Windows Communication Foundation (WCF), geralmente é necessário fornecer um certificado X. 509 para ser usado como uma credencial. O certificado normalmente faz parte de uma cadeia de certificados com uma autoridade raiz encontrada no repositório de autoridades de certificação raiz confiáveis do computador. Ter uma cadeia de certificados permite que você defina o escopo de um conjunto de certificados em que normalmente a autoridade raiz é de sua organização ou unidade de negócios. Para emular isso no momento do desenvolvimento, você pode criar dois certificados para atender aos requisitos de segurança. O primeiro é um certificado autoassinado que é colocado no repositório de autoridades de certificação raiz confiáveis e o segundo certificado é criado a partir do primeiro e é colocado no repositório pessoal do local do computador local ou no repositório pessoal do local do usuário atual. Este tópico percorre as etapas para criar esses dois certificados usando o cmdlet [New-SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate) do PowerShell.

> [!IMPORTANT]
> Os certificados que o cmdlet New-SelfSignedCertificate gera são fornecidos apenas para fins de teste. Ao implantar um serviço ou cliente, certifique-se de usar um certificado apropriado fornecido por uma autoridade de certificação. Isso pode ser de um servidor de certificado do Windows Server em sua organização ou de terceiros.
>
> Por padrão, o cmdlet [New-SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate) cria certificados que são autoassinados e esses certificados são inseguros. Colocar os certificados autoassinados no repositório de autoridades de certificação raiz confiáveis permite que você crie um ambiente de desenvolvimento que simula mais de forma mais minuciosa seu ambiente de implantação.

 Para obter mais informações sobre como criar e usar certificados, consulte [trabalhando com certificados](working-with-certificates.md). Para obter mais informações sobre como usar um certificado como uma credencial, consulte [protegendo serviços e clientes](securing-services-and-clients.md). Para obter um tutorial sobre como usar a tecnologia Authenticode da Microsoft, consulte [visões gerais e tutoriais do Authenticode](https://docs.microsoft.com/previous-versions/windows/internet-explorer/ie-developer/platform-apis/ms537360(v=vs.85)).

## <a name="to-create-a-self-signed-root-authority-certificate-and-export-the-private-key"></a>Para criar um certificado de autoridade raiz autoassinado e exportar a chave privada

O comando a seguir cria um certificado autoassinado com o nome da entidade "RootCA" no repositório pessoal do usuário atual.

```powershell
$rootcert = New-SelfSignedCertificate -CertStoreLocation Cert:\CurrentUser\My -DnsName "RootCA" -TextExtension @("2.5.29.19={text}CA=true") -KeyUsage CertSign,CrlSign,DigitalSignature
```

Precisamos exportar o certificado para um arquivo PFX para que ele possa ser importado para onde for necessário em uma etapa posterior. Ao exportar um certificado com a chave privada, é necessária uma senha para protegê-lo. Salvamos a senha em um `SecureString` e usamos o cmdlet [Export-PfxCertificate](/powershell/module/pkiclient/export-pfxcertificate) para exportar o certificado com a chave privada associada para um arquivo PFX. Também salvamos apenas o certificado público em um arquivo CRT usando o cmdlet [Export-Certificate](/powershell/module/pkiclient/export-certificate) .

```powershell
[System.Security.SecureString]$rootcertPassword = ConvertTo-SecureString -String "password" -Force -AsPlainText
[String]$rootCertPath = Join-Path -Path 'cert:\CurrentUser\My\' -ChildPath "$($rootcert.Thumbprint)"
Export-PfxCertificate -Cert $rootCertPath -FilePath 'RootCA.pfx' -Password $rootcertPassword
Export-Certificate -Cert $rootCertPath -FilePath 'RootCA.crt'
```

## <a name="to-create-a-new-certificate-signed-by-a-root-authority-certificate"></a>Para criar um novo certificado assinado por um certificado de autoridade raiz

O comando a seguir cria um certificado assinado pelo `RootCA` com um nome de assunto de "SignedByRootCA" usando a chave privada do emissor.

```powershell
$testCert = New-SelfSignedCertificate -CertStoreLocation Cert:\LocalMachine\My -DnsName "SignedByRootCA" -KeyExportPolicy Exportable -KeyLength 2048 -KeyUsage DigitalSignature,KeyEncipherment -Signer $rootCert
```

Da mesma forma, salvamos o certificado assinado com a chave privada em um arquivo PFX e apenas a chave pública em um arquivo CRT.

```powershell
[String]$testCertPath = Join-Path -Path 'cert:\LocalMachine\My\' -ChildPath "$($testCert.Thumbprint)"
Export-PfxCertificate -Cert $testCertPath -FilePath testcert.pfx -Password $rootcertPassword
Export-Certificate -Cert $testCertPath -FilePath testcert.crt
```

## <a name="installing-a-certificate-in-the-trusted-root-certification-authorities-store"></a>Instalando um certificado no repositório de autoridades de certificação raiz confiáveis

Depois que um certificado autoassinado for criado, você poderá instalá-lo no repositório de autoridades de certificação raiz confiáveis. Todos os certificados assinados com o certificado neste momento são confiáveis para o computador. Por esse motivo, exclua o certificado da loja assim que você não precisar mais dele. Quando você exclui esse certificado de autoridade raiz, todos os outros certificados assinados com ele se tornam não autorizados. Os certificados de autoridade raiz são simplesmente um mecanismo no qual um grupo de certificados pode ser definido como sendo necessário. Por exemplo, em aplicativos ponto a ponto, normalmente não há necessidade de uma autoridade raiz porque você simplesmente confia na identidade de um indivíduo por seu certificado fornecido.

### <a name="to-install-a-self-signed-certificate-in-the-trusted-root-certification-authorities"></a>Para instalar um certificado autoassinado nas autoridades de certificação raiz confiáveis

1. Abra o snap-in certificado. Para saber mais, consulte [Como Exibir Certificados com o Snap-in do MMC](how-to-view-certificates-with-the-mmc-snap-in.md).

2. Abra a pasta para armazenar o certificado, o **computador local** ou o **usuário atual**.

3. Abra a pasta **autoridades de certificação raiz confiáveis** .

4. Clique com o botão direito do mouse na pasta **certificados** e clique em **todas as tarefas**e clique em **importar**.

5. Siga as instruções do assistente na tela para importar o RootCA. pfx para o repositório.

## <a name="using-certificates-with-wcf"></a>Usando certificados com o WCF

Depois de configurar os certificados temporários, você pode usá-los para desenvolver soluções WCF que especificam certificados como um tipo de credencial de cliente. Por exemplo, a configuração XML a seguir especifica a segurança da mensagem e um certificado como o tipo de credencial do cliente.

### <a name="to-specify-a-certificate-as-the-client-credential-type"></a>Para especificar um certificado como o tipo de credencial do cliente

1. No arquivo de configuração para um serviço, use o XML a seguir para definir o modo de segurança como mensagem e o tipo de credencial do cliente como certificado.

    ```xml
    <bindings>
      <wsHttpBinding>
        <binding name="CertificateForClient">
          <security>
            <message clientCredentialType="Certificate" />
          </security>
        </binding>
      </wsHttpBinding>
    </bindings>
    ```

2. No arquivo de configuração para um cliente, use o seguinte XML para especificar que o certificado é encontrado no repositório do usuário e pode ser encontrado pesquisando o campo SubjectName para o valor "CohoWinery".

    ```xml
    <behaviors>
      <endpointBehaviors>
        <behavior name="CertForClient">
          <clientCredentials>
            <clientCertificate findValue="CohoWinery" x509FindType="FindBySubjectName" />
          </clientCredentials>
        </behavior>
      </endpointBehaviors>
    </behaviors>
    ```

Para obter mais informações sobre como usar certificados no WCF, consulte [trabalhando com certificados](working-with-certificates.md).

## <a name="net-framework-security"></a>Segurança do .NET Framework

Certifique-se de excluir qualquer certificado de autoridade raiz temporário das **autoridades de certificação raiz confiáveis** e pastas **pessoais** clicando com o botão direito do mouse no certificado e clicando em **excluir**.

## <a name="see-also"></a>Veja também

- [Trabalhando com certificados](working-with-certificates.md)
- [Como exibir certificados com o snap-in do MMC](how-to-view-certificates-with-the-mmc-snap-in.md)
- [Protegendo serviços e clientes](securing-services-and-clients.md)
