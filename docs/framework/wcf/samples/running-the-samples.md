---
title: Executando os exemplos do Windows Communication Foundation
ms.date: 03/30/2017
ms.assetid: db8a83da-95c1-4a21-a9d2-48caeb6398ea
ms.openlocfilehash: f4c7a7fa759d7339dee3d189540fb85f3883f828
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594563"
---
# <a name="running-the-windows-communication-foundation-samples"></a>Executando os exemplos do Windows Communication Foundation
Os exemplos de Windows Communication Foundation (WCF) podem ser executados em uma configuração de computador único ou entre computadores. Conforme fornecido, os exemplos estão prontos para execução em um único computador. Em uma configuração entre computadores, é necessário modificar as configurações do arquivo de configuração de um exemplo. Os procedimentos a seguir explicam como executar um exemplo em configurações de mesma máquina e entre computadores. Observe que há variações nas etapas para serviços hospedados no Serviços de Informações da Internet (IIS) e nos exemplos hospedados internamente. A maioria dos exemplos é hospedada no IIS; Consulte as informações do Leiame de exemplo para determinar como ele está hospedado.  
  
 No Windows Vista, os exemplos que não são hospedados no IIS exigem privilégios elevados para registrar um ouvinte com http. sys. Use Httpcfg. exe para registrar os endereços de escuta do serviço com a conta em que o serviço está sendo executado ou inicie o serviço a partir de um prompt de comando executando com privilégios de administrador.  
  
> [!NOTE]
> Antes de criar ou executar qualquer um dos exemplos do WCF, certifique-se de ter executado o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
### <a name="to-run-the-sample-on-the-same-machine"></a>Para executar o exemplo no mesmo computador  
  
1. Se o serviço for hospedado pelo IIS, verifique se você pode acessar o serviço usando um navegador digitando o seguinte endereço: `http://localhost/servicemodelsamples/service.svc` . Uma página de confirmação deve ser exibida em resposta. Se a página de confirmação não for exibida, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
2. Se o serviço for auto-hospedado, execute Service. exe em \service\bin, na pasta específica do idioma. A atividade de serviço é exibida na janela do console de serviço.  
  
3. Execute Client. exe de \client\bin \\ , de dentro da pasta específica do idioma. A atividade do cliente é exibida na janela do console do cliente.  
  
4. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
### <a name="to-run-the-sample-across-machines"></a>Para executar o exemplo entre computadores  
  
1. Se o serviço estiver hospedado no IIS:  
  
    1. Na máquina de serviço, crie um diretório virtual chamado ServiceModelSamples. O arquivo em lotes Setupvroot. bat incluído com [um único procedimento de configuração para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md) pode ser usado para criar o diretório de disco e o diretório virtual.  
  
    2. Copie os arquivos de programa do serviço de%SystemDrive%\Inetpub\wwwroot\servicemodelsamples para o diretório virtual ServiceModelSamples no computador de serviço. Certifique-se de incluir os arquivos no diretório \bin.  
  
    3. Teste se você pode acessar o serviço do computador cliente usando um navegador.  
  
     Se o serviço for auto-hospedado:  
  
    1. Na máquina de serviço, crie um diretório para armazenar os arquivos de serviço.  
  
    2. Copie os arquivos de programa do serviço da pasta \service\bin\, na pasta específica do idioma, para o computador de serviço.  
  
    3. No arquivo de configuração de serviço, altere o valor de endereço da definição do ponto de extremidade para corresponder ao novo endereço do serviço. Substitua todas as referências a "localhost" por um nome de domínio totalmente qualificado no endereço.  
  
    4. Inicie o Service. exe em um prompt de comando.  
  
2. Copie os arquivos de programas do cliente da pasta \client\bin\, na pasta específica do idioma, para o computador cliente.  
  
3. Defina o endereço do ponto de extremidade.  
  
    1. Se o serviço não estiver em execução em uma conta de domínio, abra o arquivo de configuração do cliente e altere o valor de endereço da definição do ponto de extremidade para corresponder ao novo endereço do serviço. Substitua todas as referências a "localhost" por um nome de domínio totalmente qualificado no endereço.  
  
    2. Se o serviço estiver sendo executado em uma conta de domínio, gere novamente a configuração do cliente executando svcutil. exe em relação ao serviço. Para obter mais informações sobre como executar svcutil. exe, consulte [Building the Windows Communication Foundation Samples](building-the-samples.md). Use o arquivo gerado em vez do arquivo de configuração no exemplo. O arquivo de configuração gerado tem informações de identidade adicionais e contém todas as configurações necessárias para se conectar ao ponto de extremidade do serviço, mesmo que sejam as configurações padrão. Para obter mais informações sobre informações de identidade, consulte [identidade e autenticação de serviço](../feature-details/service-identity-and-authentication.md)e [\<identity>](../../configure-apps/file-schema/wcf/identity.md) .  
  
4. No computador cliente, inicie o Client. exe em um prompt de comando.  
  
### <a name="to-debug-a-service"></a>Para depurar um serviço  
  
1. Compile a solução (cliente e serviço) usando o menu **Compilar** ou Ctrl + Shift + B.  
  
2. Se o serviço estiver hospedado no IIS:  
  
    1. Ative o serviço usando um navegador inserindo o endereço `http://localhost/servicemodelsamples/service.svc` .  
  
    2. Na solução, escolha o menu **depurar** e o item de menu **anexar ao processo** .  
  
    3. Marque a caixa de seleção **Mostrar processos de todos os usuários**.  
  
    4. Selecione o processo de trabalho do host w3wp. exe para depurar (selecione ASPNet_wp. exe no Windows XP).  
  
3. Agora você pode definir pontos de interrupção no código do serviço e habilitar pontos de interrupção em exceções.  
  
4. Clique com o botão direito do mouse no item de projeto cliente e escolha **depurar**, **Iniciar nova instância**.  
  
### <a name="to-clean-up-after-the-sample"></a>Para limpar após o exemplo  
  
- Se o serviço estiver hospedado no IIS para fins de segurança, remova a definição de diretório virtual e as permissões concedidas nas etapas de instalação quando você tiver concluído os exemplos.  
  
## <a name="see-also"></a>Consulte também

- [Compilando os exemplos do Windows Communication Foundation](building-the-samples.md)
- [Dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))
