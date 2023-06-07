---
title: Instruções de definição de diretório virtual
ms.date: 03/30/2017
ms.assetid: 3c62cab5-81a4-48b6-ac8c-9ce33a85a157
ms.openlocfilehash: 2d9443431601ffc712da40bd1c085f595471336b
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84602356"
---
# <a name="virtual-directory-setup-instructions"></a>Instruções de definição de diretório virtual
Os exemplos de Windows Communication Foundation (WCF) destinam-se a compartilhar um diretório virtual comum denominado servicemodelsamples que é mapeado para a pasta%SystemDrive%\inetpub\wwwroot\servicemodelsamples.  
  
> [!NOTE]
> % SystemDrive% geralmente é C: ou D:, dependendo do local da unidade em que o Serviços de Informações da Internet (IIS) está instalado.  
  
 Você pode executar os arquivos Setupvroot. bat e Cleanupvroot. bat do [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md) para criar o diretório virtual. Se preferir criar o diretório virtual manualmente, use os procedimentos a seguir.  
  
## <a name="procedures"></a>Procedimentos  
  
#### <a name="to-create-a-virtual-directory-in-iis-70-or-75"></a>Para criar um diretório virtual no IIS 7,0 ou 7,5  
  
1. No menu **Iniciar** , clique em **executar**e digite **inetmgr** para abrir o snap-in do MMC serviços de informações da Internet (IIS).  
  
2. No painel esquerdo, expanda o nó com o nome do computador e, em seguida, expanda o nó **sites** .  
  
3. Clique com o botão direito do mouse em **site padrão**e selecione **Adicionar aplicativo** para abrir a **janela Adicionar aplicativo**.  
  
4. Na janela, digite `servicemodelsamples` como o alias para o diretório virtual que você está criando.  
  
5. Crie o seguinte diretório:%SystemDrive%\inetpub\wwwroot\servicemodelsamples  
  
6. Definir o caminho físico como%SystemDrive%\inetpub\wwwroot\servicemodelsamples.  A maioria dos exemplos do WCF copia arquivos executáveis do serviço para esse local quando compilado.  
  
7. Clique em **OK**. O aplicativo Web agora é criado para os exemplos do WCF.  
  
    > [!NOTE]
    > Essa tarefa deve ser executada apenas uma vez, porque todos os exemplos do WCF usam o mesmo aplicativo Web servicemodelsamples.  
  
    > [!NOTE]
    > Para a finalidade desta documentação, o termo `virtual directory` é sinônimo de `Web application` .  
  
     Além de criar o diretório virtual, você também deve definir suas propriedades para permitir que os serviços WCF sejam executados. Veja abaixo para obter mais detalhes.  
  
#### <a name="to-create-a-virtual-directory-in-iis-51-or-60"></a>Para criar um diretório virtual no IIS 5,1 ou 6,0  
  
1. Abra uma janela de prompt de comando e digite `start inetmgr` para abrir o snap-in do MMC serviços de informações da Internet (IIS).  
  
2. No painel esquerdo, expanda o nó com o nome do computador e, em seguida, expanda o nó **sites** .  
  
3. Clique com o botão direito do mouse em **site padrão** e selecione **novo, diretório virtual** para abrir o assistente para criação de diretório virtual.  
  
4. No assistente, digite `servicemodelsamples` como o alias para o diretório virtual que você está criando.  
  
5. Defina o caminho para%SystemDrive%\inetpub\wwwroot\servicemodelsamples. A maioria dos exemplos do WCF copia arquivos executáveis do serviço para esse local quando compilado.  
  
6. Clique em **Avançar**.  
  
7. Por padrão, as seguintes caixas de seleção são selecionadas:  
  
    - **Leitura**  
  
    - **Executar scripts (como ASP)**  
  
8. Clique em **Avançar**e em **concluir** para concluir o assistente.  
  
    > [!NOTE]
    > Esta tarefa deve ser executada apenas uma vez porque todos os exemplos do WCF usam o mesmo diretório virtual servicemodelsamples.  
  
#### <a name="to-set-additional-virtual-directory-properties-in-iis-70-or-75"></a>Para definir propriedades de diretório virtual adicionais no IIS 7,0 ou 7,5  
  
1. Clique no nó servicemodelsamples. Ao longo da parte inferior da janela, são listadas duas exibições. Selecione **recursos exibir** se ainda não estiver selecionado.  
  
2. Clique duas vezes na entrada para **pesquisa no diretório**.  
  
3. No painel Ações, selecione a opção **habilitar** . Isso permite que você acesse o diretório do diretório usando o Internet Explorer, que ajuda a depurar um serviço.  
  
 Por fim, você deve definir as propriedades de segurança da pasta servicemodelsamples para permitir que ela seja acessada por outras pessoas. Veja abaixo para obter mais detalhes.  
  
#### <a name="to-set-additional-virtual-directory-properties-in-iis-51-or-60"></a>Para definir propriedades de diretório virtual adicionais no IIS 5,1 ou 6,0  
  
1. Clique com o botão direito do mouse no nó servicemodelsamples e clique em **Propriedades**.  
  
2. Por padrão, as seguintes caixas de seleção são selecionadas:  
  
    - **Leitura**  
  
    - **Registrar visitas**  
  
    - **Indexar este recurso**  
  
3. Marque a caixa de seleção **pesquisa no diretório** . Isso permite que você acesse o diretório do diretório usando o Internet Explorer, que ajuda a depurar um serviço.  
  
#### <a name="to-set-security-properties-of-the-folder-in-iis-70-or-75"></a>Para definir as propriedades de segurança da pasta no IIS 7,0 ou 7,5  
  
1. Navegue até%SystemDrive%\inetpub\wwwroot\servicemodelsamples.  
  
2. Clique com o botão direito do mouse na pasta servicemodelsamples e clique em **compartilhar** ou **compartilhar com**.  
  
3. Clique na seta para baixo à esquerda do botão **Adicionar** .  
  
4. Selecione a entrada **Localizar** . A janela **Selecionar usuários ou grupos** é aberta.  
  
5. Clique em **Avançado**.  
  
6. Clique em **locais**. A janela **locais** agora está aberta.  
  
7. Selecione a entrada para o computador que está sendo usado. É importante selecionar o computador local e não uma entrada para todos os domínios ou redes listados. Depois de selecionar o computador, clique em **OK**.  
  
8. Clique em **Localizar Agora**. Isso popula os resultados da pesquisa com objetos associados ao computador local.  
  
9. Localize a entrada de **IIS_IUSRS** na coluna **nome (nome distinto relativo)** . Selecione essa entrada e clique em **OK** para fechar a janela resultados da pesquisa.  
  
10. Clique em **OK** para fechar a janela **Selecionar usuários ou grupos** .  
  
11. Clique em **compartilhar** para manter as alterações.  
  
12. Depois que as alterações para habilitar o compartilhamento forem concluídas, clique em **concluído** para fechar a janela de **compartilhamento de arquivos** .  
  
#### <a name="to-set-security-properties-of-the-folder-in-iis-51-or-60"></a>Para definir as propriedades de segurança da pasta no IIS 5,1 ou 6,0  
  
1. Navegue até%SystemDrive%\inetpub\wwwroot\servicemodelsamples.  
  
2. Clique com o botão direito do mouse na pasta **servicemodelsamples** e clique em **compartilhamento e segurança.**  
  
3. Clique na guia **Segurança**.  
  
4. Se você estiver usando o IIS 6,0, na caixa **nomes de grupo ou de usuário** , verifique se a conta de convidado da **Internet** está listada.  
  
     Se não estiver listado:  
  
    1. Clique em **Iniciar** e em **painel de controle**.  
  
    2. Se você não vir o ícone **contas de usuário** , clique em **alternar para a exibição de categoria**.  
  
    3. Clique no ícone **contas de usuário** .  
  
    4. Em "ou selecione um ícone do painel de controle", clique em **contas de usuário**.  
  
    5. Na caixa de diálogo **contas de usuário** , clique na guia **avançado** .  
  
    6. Clique em **Avançado**.  
  
    7. Na caixa de diálogo **usuários e grupos locais** , clique para expandir a pasta **usuários** .  
  
    8. No painel direito, clique duas vezes em **conta de convidado da Internet**.  
  
    9. Na caixa de diálogo **Propriedades** , copie o nome usado como a conta de convidado da Internet. Por padrão, o nome começa com "USR_" seguido do nome do computador.  
  
    10. Feche a caixa de diálogo **Propriedades** .  
  
    11. Feche a caixa de diálogo **usuários e grupos locais** .  
  
    12. Feche a caixa de diálogo **contas de usuário** .  
  
    13. Feche a caixa de diálogo outras **contas de usuário** .  
  
    14. Na caixa de diálogo **Propriedades de servicemodelsamples** , na guia **segurança** , clique em **Adicionar**.  
  
    15. Digite o nome do computador seguido por uma barra invertida e cole o nome da conta de usuário da Internet, por exemplo, MyMachineName \\ % InternetGuestAccountName%  
  
    16. Clique em **verificar nomes** para verificar a adição. Se for válido, o nome estará em letras maiúsculas e será sublinhado.  
  
5. Para o IIS 6,0, verifique também se serviço de rede está listado na caixa **nomes de grupo ou de usuário** .  
  
     Se o serviço de rede não estiver listado:  
  
    1. Clique em **Adicionar**.  
  
    2. Na caixa de diálogo **Selecionar usuários ou grupos** , digite o nome do computador seguido por uma barra invertida.  
  
    3. Digite o **serviço** após a barra invertida (sem espaço).  
  
    4. Clique em **verificar nomes**.  
  
    5. Se vários nomes forem encontrados, selecione **serviço de rede** e clique em **OK**.  
  
    6. Clique em **OK** para fechar a caixa de diálogo **Selecionar usuários ou grupos** .  
  
6. Se você estiver usando o Windows XP SP2 com o IIS 5,1, verifique se a conta de convidado da Internet e o ASPNET estão listados na caixa **nomes de grupo ou de usuário** .  
  
     Observe que o usuário ASPNET pode ser membro do grupo de segurança **usuários** internos. Nesse caso, se o grupo **usuários** estiver listado na caixa de diálogo, você não precisará adicioná-lo como um item separado à lista de usuários permitidos.  
  
     Para verificar se o ASPNET faz parte do grupo de segurança **usuários** :  
  
    1. No menu **Iniciar** , clique em **Painel de Controle**.  
  
    2. Clique no ícone **contas de usuário** .  
  
    3. Na coluna **grupo** , verifique se o valor de **ASPNET** é "Users".  
  
## <a name="see-also"></a>Consulte também

- [Instruções de hospedagem de serviço de informação de internet](internet-information-service-hosting-instructions.md)
