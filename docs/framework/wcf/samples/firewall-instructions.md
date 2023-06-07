---
title: Instruções do firewall
description: Saiba como habilitar portas ou programas no firewall para exemplos do WCF. Use um desses procedimentos, dependendo de seus requisitos e ambiente de segurança.
ms.date: 03/30/2017
ms.assetid: a7dc429f-65ac-4faf-974a-77d5fb977fe1
ms.openlocfilehash: de55d067960b8f2096c129f6feaf037219e06a96
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246130"
---
# <a name="firewall-instructions"></a>Instruções de firewall

Você deve habilitar várias portas ou programas no firewall para que os exemplos de Windows Communication Foundation (WCF) possam funcionar. Muitos dos exemplos se comunicam usando portas no intervalo de 8000-8003 e a porta 9000. O firewall é ativado por padrão e impede o acesso a essas portas. Para habilitar o firewall para os exemplos, conclua um dos seguintes procedimentos, dependendo de seus requisitos e ambiente de segurança:

- Opção 1: habilitar os exemplos interativamente durante a execução. Não faça nenhuma alteração antecipada na configuração do firewall e continue para começar a criar e executar os exemplos. Quando um exemplo é executado, uma caixa de diálogo **alerta de segurança do Windows** é exibida. O programa de exemplo em questão pode ser adicionado interativamente a uma lista desbloqueada. Com esse procedimento, talvez seja necessário reiniciar o exemplo.

- Opção 2: habilite os programas de exemplo com antecedência. Inicie o miniaplicativo do **painel de controle do firewall do Windows** e habilite os programas de exemplo que você planeja executar. Você deve criar os programas primeiro para que os arquivos executáveis existam. Você pode encontrar instruções mais detalhadas no procedimento a seguir.

- Opção 3: habilitar um intervalo de portas com antecedência. Inicie o miniaplicativo do **painel de controle do firewall do Windows** e habilite as portas 80, 443, 8000-8003 e 9000, que são usadas pelos exemplos. Você pode encontrar instruções mais detalhadas no procedimento a seguir. Essa opção é menos segura do que as outras porque permite que qualquer programa use essas portas, não apenas os exemplos.

Se você não tiver certeza de qual procedimento usar, escolha a primeira opção. Se você estiver executando um firewall de outro fornecedor, talvez seja necessário fazer alterações semelhantes.

> [!IMPORTANT]
> A alteração da configuração do firewall afeta sua segurança. É recomendável que você registre as alterações feitas e remova-as quando terminar de trabalhar com os exemplos.

## <a name="enable-samples-programs-in-advance"></a>Habilitar programas de exemplo com antecedência

1. Compile o exemplo.

2. Escolha **Iniciar**  >  **execução**e digite `firewall.cpl` . Isso abre o miniaplicativo do **painel de controle do firewall do Windows** .

    > [!NOTE]
    > Você deve ter permissão para alterar as configurações de firewall para executar exemplos que exigem a capacidade de comunicação por meio do firewall do Windows. Se algumas configurações de firewall estiverem indisponíveis e o computador estiver conectado a um domínio, o administrador do sistema poderá estar controlando essas configurações por meio de Política de Grupo.

3. Conclua uma das seguintes etapas específicas de operação para permitir um programa por meio do firewall do Windows:

    - No Windows 7 ou no Windows Server 2008 R2, clique em **permitir um programa ou recurso pelo firewall do Windows**. Clique em **alterar configurações**  >  **permitir outro programa**.

    - No Windows Vista ou no Windows Server 2008, clique em **permitir um programa pelo firewall do Windows**.

4. Na guia **exceções** , clique em **Adicionar programa**.

5. Clique no botão **procurar** e selecione o arquivo executável do exemplo que você planeja executar.

6. Repita as etapas 4 e 5 até adicionar os arquivos executáveis de todos os exemplos que você planeja executar.

7. Clique em **OK** para fechar o miniaplicativo firewall.

## <a name="enable-a-port-range-in-advance"></a>Habilitar um intervalo de portas com antecedência

1. Escolha **Iniciar**  >  **execução**e digite `firewall.cpl` . Isso abre o miniaplicativo do **painel de controle do firewall do Windows** .

2. No Windows 7 ou no Windows Server 2008 R2, siga estas etapas.

    1. Clique em **Configurações avançadas** na coluna esquerda da janela Firewall do Windows.

    2. Clique em **regras de entrada** na coluna esquerda.

    3. Clique em **novas regras** na coluna à direita.

    4. Selecione **porta** e clique em **Avançar**.

    5. Selecione **TCP** e insira `8000, 8001, 8002, 8003, 9000, 80, 443` no campo **portas locais específicas** .

    6. Clique em **Próximo**.

    7. Selecione **permitir a conexão**e clique em **Avançar** .

    8. Selecione **domínio** e **privado**e clique em **Avançar**.

    9. Nomeie essa regra `WCF-WF 4.0 Samples` e clique em **concluir**.

    10. Clique em **regras de saída** e repita as etapas de c a h.

3. No Windows Vista ou no Windows Server 2008, siga estas etapas.

    1. Clique em **Permitir um programa pelo Firewall do Windows**.

    2. Na guia **exceções** , clique em **Adicionar porta**.

    3. Insira um nome, insira 8000 como o número da porta e selecione a opção **TCP** .

    4. Clique no botão **alterar escopo** , selecione a opção **minha rede** (sub-rede) somente e clique em **OK**.

    5. Repita as etapas de b a d para as portas 8001, 8002, 8003, 9000, 80 e 443.

4. Clique em **OK** para fechar o miniaplicativo firewall.

> [!NOTE]
> Remova as exceções de firewall quando terminar de trabalhar com os exemplos. Para fazer isso, abra o miniaplicativo do **painel de controle do firewall do Windows** e remova todos os programas ou entradas de porta que foram adicionados pelos procedimentos anteriores.
