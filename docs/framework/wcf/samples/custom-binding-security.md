---
title: Segurança de associação personalizada
ms.date: 03/30/2017
ms.assetid: a6383dff-4308-46d2-bc6d-acd4e18b4b8d
ms.openlocfilehash: eb575594cec9ea714578bc104344acc14b00e9df
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84592457"
---
# <a name="custom-binding-security"></a>Segurança de associação personalizada

Este exemplo demonstra como configurar a segurança usando uma associação personalizada. Ele mostra como usar uma associação personalizada para habilitar a segurança em nível de mensagem com um transporte seguro. Isso é útil quando um transporte seguro é necessário para transmitir as mensagens entre o cliente e o serviço e simultaneamente as mensagens devem ser seguras no nível de mensagem. Não há suporte para essa configuração por associações fornecidas pelo sistema.

Este exemplo consiste em um programa de console do cliente (EXE) e um programa de console do serviço (EXE). O serviço implementa um contrato duplex. O contrato é definido pela `ICalculatorDuplex` interface, que expõe operações matemáticas (adicionar, subtrair, multiplicar e dividir). A `ICalculatorDuplex` interface permite que o cliente execute operações matemáticas, calculando um resultado em execução em uma sessão. Independentemente, o serviço pode retornar resultados na `ICalculatorDuplexCallback` interface. Um contrato duplex requer uma sessão, porque um contexto deve ser estabelecido para correlacionar o conjunto de mensagens que estão sendo enviadas entre o cliente e o serviço. Uma associação personalizada é definida com suporte à comunicação duplex e é segura.

> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.

A configuração de serviço define uma associação personalizada que dá suporte ao seguinte:

- Comunicação TCP protegida usando o protocolo TLS/SSL.

- Segurança de mensagem do Windows.

A configuração de associação personalizada permite o transporte seguro habilitando simultaneamente a segurança em nível de mensagem. A ordenação de elementos de associação é importante na definição de uma associação personalizada, pois cada uma representa uma camada na pilha de canais (consulte [associações personalizadas](../extending/custom-bindings.md)). A associação personalizada é definida nos arquivos de configuração do serviço e do cliente, conforme mostrado na seguinte configuração de exemplo.

```xml
<bindings>
  <!-- Configure a custom binding. -->
  <customBinding>
    <binding name="Binding1">
      <security authenticationMode="SecureConversation"
                 requireSecurityContextCancellation="true">
      </security>
      <textMessageEncoding messageVersion="Soap12WSAddressing10" writeEncoding="utf-8"/>
      <sslStreamSecurity requireClientCertificate="false"/>
      <tcpTransport/>
    </binding>
  </customBinding>
</bindings>
```

A associação personalizada usa um certificado de serviço para autenticar o serviço no nível de transporte e para proteger as mensagens durante a transmissão entre o cliente e o serviço. Isso é feito pelo `sslStreamSecurity` elemento Binding. O certificado do serviço é configurado usando um comportamento de serviço, conforme mostrado na seguinte configuração de exemplo.

```xml
<behaviors>
    <serviceBehaviors>
    <behavior name="CalculatorServiceBehavior">
        <serviceMetadata />
        <serviceDebug includeExceptionDetailInFaults="False" />
        <serviceCredentials>
        <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName"/>
        </serviceCredentials>
    </behavior>
    </serviceBehaviors>
</behaviors>
```

Além disso, a associação personalizada usa segurança de mensagem com o tipo de credencial do Windows – esse é o tipo de credencial padrão. Isso é feito pelo `security` elemento Binding. O cliente e o serviço são autenticados usando a segurança no nível da mensagem se o mecanismo de autenticação Kerberos estiver disponível. Isso ocorrerá se o exemplo for executado no ambiente de Active Directory. Se o mecanismo de autenticação Kerberos não estiver disponível, a autenticação NTLM será usada. O NTLM autentica o cliente para o serviço, mas não autentica o serviço para o cliente. O `security` elemento Binding é configurado para usar `SecureConversation` `authenticationType` , o que resulta na criação de uma sessão de segurança no cliente e no serviço. Isso é necessário para permitir que o contrato duplex do serviço funcione.

Quando você executa o exemplo, as solicitações e respostas da operação são exibidas na janela do console do cliente. Pressione ENTER na janela do cliente para desligar o cliente.

```console
Press <ENTER> to terminate client.
Result(100)
Result(50)
Result(882.5)
Result(441.25)
Equation(0 + 100 - 50 * 17.65 / 2 = 441.25)
```

Ao executar o exemplo, você verá as mensagens retornadas para o cliente na interface de retorno de chamada enviadas do serviço. Cada resultado intermediário é exibido, seguido de toda a equação após a conclusão de todas as operações. Pressione ENTER para desligar o cliente.

O arquivo setup. bat incluído permite que você configure o cliente e o servidor com o certificado de serviço relevante para executar um aplicativo hospedado que requer segurança baseada em certificado. Esse arquivo em lotes deve ser modificado para funcionar em computadores ou para funcionar em um caso não hospedado.

Veja a seguir uma breve visão geral das diferentes seções dos arquivos em lotes que se aplicam a esse exemplo para que eles possam ser modificados para serem executados na configuração apropriada:

- Criando o certificado do servidor.

  As linhas a seguir do arquivo setup. bat criam o certificado do servidor a ser usado. A `%SERVER_NAME%` variável especifica o nome do servidor. Altere essa variável para especificar seu próprio nome de servidor. Esse arquivo em lotes padroniza o nome do servidor para localhost.

  O certificado é armazenado no armazenamento CurrentUser para os serviços hospedados na Web.

  ```bat
  echo ************
  echo Server cert setup starting
  echo %SERVER_NAME%
  echo ************
  echo making server cert
  echo ************
  makecert.exe -sr LocalMachine -ss MY -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe
  ```

- Instalar o certificado do servidor no repositório de certificados confiáveis do cliente.

  As linhas a seguir no arquivo setup. bat copiam o certificado do servidor no repositório de pessoas confiáveis do cliente. Essa etapa é necessária porque os certificados gerados pelo MakeCert. exe não são implicitamente confiáveis pelo sistema cliente. Se você já tiver um certificado com raiz em um certificado raiz confiável do cliente — por exemplo, um certificado emitido pela Microsoft — essa etapa de popular o repositório de certificados do cliente com o certificado do servidor não será necessária.

  ```console
  certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople
  ```

  > [!NOTE]
  > O arquivo em lotes setup. bat foi projetado para ser executado em um prompt de comando do Visual Studio 2010. Ele requer que a variável de ambiente MSSDK aponte para o diretório em que o SDK está instalado. Essa variável de ambiente é definida automaticamente em um prompt de comando do Visual Studio 2010.

### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo

1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

3. Para executar o exemplo em uma configuração de computador único ou entre computadores, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).

### <a name="to-run-the-sample-on-the-same-computer"></a>Para executar o exemplo no mesmo computador

1. Abra um Prompt de Comando do Desenvolvedor para a janela do Visual Studio com privilégios de administrador e execute Setup. bat na pasta de instalação de exemplo. Isso instala todos os certificados necessários para executar o exemplo.

    > [!NOTE]
    > O arquivo em lotes setup. bat foi projetado para ser executado em um prompt de comando do Visual Studio 2012. A variável de ambiente PATH definida no prompt de comando do Visual Studio 2012 aponta para o diretório que contém os executáveis exigidos pelo script setup. bat.

2. Inicie o Service. exe em \service\bin.

3. Inicie o Client. exe em \client\bin. A atividade do cliente é exibida no aplicativo de console do cliente.

4. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).

### <a name="to-run-the-sample-across-computers"></a>Para executar o exemplo entre computadores

1. No computador do serviço:

    1. Crie um diretório virtual chamado servicemodelsamples no computador do serviço.

    2. Copie os arquivos de programa do serviço do \inetpub\wwwroot\servicemodelsamples para o diretório virtual no computador do serviço. Certifique-se de copiar os arquivos no subdiretório \bin.

    3. Copie os arquivos Setup. bat e Cleanup. bat para o computador de serviço.

    4. Execute o seguinte comando em um Prompt de Comando do Desenvolvedor para Visual Studio aberto com privilégios de administrador: `Setup.bat service` . Isso cria o certificado de serviço com o nome da entidade correspondente ao nome do computador em que o arquivo em lotes foi executado.

        > [!NOTE]
        > O arquivo em lotes setup. bat foi projetado para ser executado em um prompt de comando do Visual Studio 2010. Ele requer que a variável de ambiente Path aponte para o diretório em que o SDK está instalado. Essa variável de ambiente é definida automaticamente em um prompt de comando do Visual Studio 2010.

    5. Altere o [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md) dentro do arquivo Service. exe. config para refletir o nome da entidade do certificado gerado na etapa anterior.

    6. Execute o Service. exe em um prompt de comando.

2. No computador cliente:

    1. Copie os arquivos de programa do cliente da pasta \client\bin\ para o computador cliente. Copie também o arquivo Cleanup. bat.

    2. Execute Cleanup. bat para remover quaisquer certificados antigos de exemplos anteriores.

    3. Exporte o certificado do serviço abrindo um Prompt de Comando do Desenvolvedor para o Visual Studio com privilégios administrativos e executando o seguinte comando no computador do serviço (substitua `%SERVER_NAME%` pelo nome totalmente qualificado do computador em que o serviço está em execução):

        ```console
        certmgr -put -r LocalMachine -s My -c -n %SERVER_NAME% %SERVER_NAME%.cer
        ```

    4. Copie% SERVER_NAME%. cer para o computador cliente (substitua% SERVER_NAME% pelo nome totalmente qualificado do computador em que o serviço está em execução).

    5. Importe o certificado do serviço abrindo um Prompt de Comando do Desenvolvedor para o Visual Studio com privilégios administrativos e executando o seguinte comando no computador cliente (substitua% SERVER_NAME% pelo nome totalmente qualificado do computador em que o serviço está em execução):

        ```console
        certmgr.exe -add -c %SERVER_NAME%.cer -s -r CurrentUser TrustedPeople
        ```

        As etapas c, d e e não serão necessárias se o certificado for emitido por um emissor confiável.

    6. Modifique o arquivo app. config do cliente da seguinte maneira:

        ```xml
        <client>
            <endpoint name="default"
                address="net.tcp://ReplaceThisWithServiceMachineName:8000/ServiceModelSamples/Service"
                binding="customBinding"
                bindingConfiguration="Binding1"
                contract="Microsoft.ServiceModel.Samples.ICalculatorDuplex"
                behaviorConfiguration="CalculatorClientBehavior" />
        </client>
        ```

    7. Se o serviço estiver sendo executado em uma conta diferente da conta NetworkService ou LocalSystem em um ambiente de domínio, talvez seja necessário modificar a identidade do ponto de extremidade para o ponto de extremidade de serviço dentro do arquivo app. config do cliente para definir o UPN ou o SPN apropriado com base na conta usada para executar o serviço. Para obter mais informações sobre a identidade do ponto de extremidade, consulte o tópico [identidade e autenticação de serviço](../feature-details/service-identity-and-authentication.md) .

    8. Execute Client. exe em um prompt de comando.

### <a name="to-clean-up-after-the-sample"></a>Para limpar após o exemplo

- Execute o Cleanup. bat na pasta Samples depois de concluir a execução do exemplo.
