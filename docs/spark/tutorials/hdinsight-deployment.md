---
title: Implantar um aplicativo .NET para Apache Spark no Azure HDInsight
description: Descubra como implantar um aplicativo do .NET para Apache Spark no HDInsight.
ms.date: 06/25/2020
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: e6b2fdd1818250c47ce6cb64439ecab58ae99ad8
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617633"
---
# <a name="tutorial-deploy-a-net-for-apache-spark-application-to-azure-hdinsight"></a>Tutorial: implantar um aplicativo .NET para Apache Spark no Azure HDInsight

Este tutorial ensina como implantar seu .NET para Apache Spark aplicativo na nuvem por meio de um cluster do Azure HDInsight. O HDInsight facilita a criação e a configuração de um cluster Spark no Azure, pois os clusters do Spark no HDInsight são compatíveis com o armazenamento do Azure e Azure Data Lake Storage.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
>
> * Acesse suas contas de armazenamento usando Gerenciador de Armazenamento do Azure.
> * Crie um cluster HDInsight do Azure.
> * Publique seu aplicativo .NET para Apache Spark.
> * Crie e execute uma ação de script do HDInsight.
> * Execute um aplicativo .NET para Apache Spark em um cluster HDInsight.

[!INCLUDE [spark-preview-note](../../../includes/spark-preview-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, execute as seguintes tarefas:

* Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/dotnet/).
* Entre no [portal do Azure](https://portal.azure.com/).
* Instale o Gerenciador de Armazenamento do Azure em seu computador [Windows](https://go.microsoft.com/fwlink/?LinkId=708343&clcid=0x409), [Linux](https://go.microsoft.com/fwlink/?LinkId=722418&clcid=0x409)ou [MacOS](https://go.microsoft.com/fwlink/?LinkId=708342&clcid=0x409) .
* Conclua o [.net para Apache Spark-introdução no tutorial de 10 minutos](https://dotnet.microsoft.com/learn/data/spark-tutorial/intro) .

## <a name="access-your-storage-accounts"></a>Acessar suas contas de armazenamento

1. Abra o Gerenciador de Armazenamento do Azure.

2. Selecione **adicionar conta** no menu à esquerda e entre em sua conta do Azure.

    ![Entrar na conta do Azure de Gerenciador de Armazenamento](./media/hdinsight-deployment/signin-azure-storage-explorer.png)

   Depois de entrar, você deverá ver todas as contas de armazenamento que você tem e todos os recursos que você carregou em suas contas de armazenamento.

## <a name="create-an-hdinsight-cluster"></a>Crie um cluster HDInsight

> [!IMPORTANT]
> A cobrança por clusters HDInsight é rateada por minuto, mesmo se você não os estiver usando. Exclua seu cluster depois de terminar de usá-lo. Para obter mais informações, consulte a seção [limpar recursos](#clean-up-resources) deste tutorial.

1. Visite o [Portal do Azure](https://portal.azure.com).

2. Selecione **+ Criar um recurso**. Em seguida, selecione **HDInsight** na categoria **análise** .

    ![Criar recurso do HDInsight a partir de portal do Azure](./media/hdinsight-deployment/create-hdinsight-resource.png)

3. Em **Noções básicas**, forneça os seguintes valores:

    |Propriedade  |Descrição  |
    |---------|---------|
    |Subscription  | Na lista suspensa, escolha uma das suas assinaturas ativas do Azure. |
    |Grupo de recursos | Especifique se deseja criar um novo grupo de recursos ou usar um existente. Um grupo de recursos é um contêiner que mantém os recursos relacionados a uma solução do Azure. |
    |Nome do cluster | Dê um nome para seu cluster HDInsight Spark.|
    |Location   | Selecione um local para o grupo de recursos. O modelo usa esse local para criar o cluster, bem como para o armazenamento de cluster padrão. |
    |Tipo de cluster| Selecione **Spark** como o tipo de cluster.|
    |Versão do cluster|Este campo será preenchido automaticamente com a versão padrão depois que o tipo de cluster tiver sido selecionado. Selecione uma versão 2,3 ou 2,4 do Spark.|
    |Nome de usuário de logon do cluster| Insira o nome de logon do usuário do cluster.  O nome padrão é *admin*. |
    |Senha de logon do cluster| Insira qualquer senha de logon. |
    |Nome de usuário do Secure Shell (SSH)| Insira um Nome de Usuário SSH. Por padrão, essa conta tem a mesma senha que a conta de*nome de usuário de logon do cluster*. |

4. Selecione **Avançar: Armazenamento >>** para continuar para a página **Armazenamento**. Em **Armazenamento**, forneça os seguintes valores:

    |Propriedade  |Descrição  |
    |---------|---------|
    |Tipo de armazenamento primário|Use o valor padrão **Armazenamento do Azure**.|
    |Método de seleção|Use o valor padrão **Selecione na lista**.|
    |Conta de armazenamento primária|Escolha sua assinatura e uma de suas contas de armazenamento ativas nessa assinatura.|
    |Contêiner|Esse contêiner é o contêiner de blob específico em sua conta de armazenamento em que o cluster procura arquivos para executar seu aplicativo na nuvem. Você pode dar a ele qualquer nome disponível.|

5. Em **Examinar + criar**, selecione **Criar**. Demora cerca de 20 minutos para criar o cluster. O cluster deve ser criado para que você possa continuar para a próxima etapa.

## <a name="publish-your-app"></a>Publicar seu aplicativo

Em seguida, você publica o *mySparkApp* criado no [.net para Apache Spark-introdução no tutorial de 10 minutos](https://dotnet.microsoft.com/learn/data/spark-tutorial/intro) , que dá acesso ao cluster Spark a todos os arquivos necessários para executar seu aplicativo.

1. Execute os seguintes comandos para publicar o *mySparkApp*:

   **No Windows:**

   ```dotnetcli
   cd mySparkApp
   dotnet publish -c Release -f netcoreapp3.0 -r ubuntu.16.04-x64
   ```

   **No Linux:**

   ```bash
   cd mySparkApp
   foo@bar:~/path/to/app$ dotnet publish -c Release -f netcoreapp3.0 -r ubuntu.16.04-x64
   ```

2. Execute as seguintes tarefas para compactar seus arquivos de aplicativo publicados para que você possa carregá-los facilmente em seu cluster HDInsight.

   **No Windows:**

   Navegue até *mySparkApp/bin/Release/netcoreapp 3.0/Ubuntu. 16.04-x64*. Em seguida, clique com o botão direito do mouse em **publicar** pasta e selecione **Enviar para > pasta compactada (zipada)**. Nomeie a nova pasta **publish.zip**.

   **No Linux, execute o seguinte comando:**

   ```bash
   zip -r publish.zip
   ```

## <a name="upload-files-to-azure"></a>Carregar arquivos no Azure

Em seguida, use o Gerenciador de Armazenamento do Azure para carregar os cinco arquivos a seguir no contêiner de BLOBs que você escolheu para o armazenamento do cluster:

* Microsoft. Spark. Worker
* install-worker.sh
* publish.zip
* Microsoft-Spark-2.3. x-0.3.0. jar
* input.txt.

1. Abra Gerenciador de Armazenamento do Azure e navegue até sua conta de armazenamento no menu à esquerda. Faça uma busca detalhada no contêiner de BLOBs do cluster em **contêineres de blob** em sua conta de armazenamento.

2. O *Microsoft. Spark. Worker* ajuda a Apache Spark executar seu aplicativo, como qualquer UDFs (funções definidas pelo usuário) que você possa ter escrito. Baixe [Microsoft. Spark. Worker](https://github.com/dotnet/spark/releases/download/v0.3.0/Microsoft.Spark.Worker.netcoreapp2.1.linux-x64-0.3.0.tar.gz). Em seguida, selecione **carregar** em Gerenciador de armazenamento do Azure para carregar o trabalho.

   ![Carregar arquivos para Gerenciador de Armazenamento do Azure](./media/hdinsight-deployment/upload-files-to-storage.png)

3. O *install-Worker.sh* é um script que permite que você copie o .net para Apache Spark arquivos dependentes nos nós do cluster.

   Crie um novo arquivo chamado **install-Worker.sh** seu computador local e cole o [conteúdo do install-Worker.sh](https://raw.githubusercontent.com/dotnet/spark/master/deployment/install-worker.sh) localizado no github. Em seguida, carregue *install-Worker.sh* em seu contêiner de BLOB.

4. O cluster precisa do arquivo de publish.zip que contém os arquivos publicados do aplicativo. Navegue até a pasta publicada, **mySparkApp/bin/Release/netcoreapp 3.0/Ubuntu. 16.04-x64**, e localize **publish.zip**. Em seguida, carregue *publish.zip* em seu contêiner de BLOBs.

5. O cluster precisa do código do aplicativo que foi empacotado em um arquivo jar. Navegue até a pasta publicada, **mySparkApp/bin/Release/netcoreapp 3.0/Ubuntu. 16.04-x64**, e localize **Microsoft-Spark-2.3. x-0.3.0. jar**. Em seguida, carregue o arquivo jar em seu contêiner de BLOB.

   Pode haver vários arquivos. jar (para as versões 2.3. x e 2.4. x do Spark). Você precisa escolher o arquivo. jar que corresponde à versão do Spark escolhida durante a criação do cluster. Por exemplo, escolha *Microsoft-Spark-2.3. x-0.3.0. jar* se você escolheu Spark 2.3.2 durante a criação do cluster.

6. O cluster precisa da entrada para seu aplicativo. Navegue até o diretório **mySparkApp** e localize **input.txt**. Carregue o arquivo de entrada no diretório **User/sshuser** no seu contêiner de BLOB. Você se conectará ao cluster por meio de SSH e essa pasta será onde o cluster procurará sua entrada. O arquivo de *input.txt* é o único arquivo carregado em um diretório específico.

## <a name="run-the-hdinsight-script-action"></a>Executar a ação de script do HDInsight

Depois que o cluster estiver em execução e você carregou seus arquivos no Azure, execute o script **install-Worker.sh** no cluster.

1. Navegue até o cluster HDInsight Spark no portal do Azure e, em seguida, selecione **ações de script**.

2. Selecione **+ Enviar novo** e forneça os seguintes valores:

   |Propriedade  |Descrição  |
   |---------|---------|
   | Tipo de script |Personalizado|
   | Name | Instalar trabalho|
   | URI do script Bash |`https://mystorageaccount.blob.core.windows.net/mycontainer/install-worker.sh` </br> Para confirmar esse URI, clique com o botão direito do mouse em install-worker.sh em Gerenciador de Armazenamento do Azure e selecione Propriedades. |
   | Tipo(s) de nó| Trabalho|
   | Parâmetros | azure </br> wasbs://mycontainer@myStorageAccount.blob.core.windows.net/Microsoft.Spark.Worker.netcoreapp2.1.linux-x64-0.6.0.tar.gz </br> /usr/local/bin

3. Selecione **criar** para enviar o script.

## <a name="run-your-app"></a>Executar seu aplicativo

1. Navegue até o cluster HDInsight Spark no portal do Azure e, em seguida, selecione **SSH + logon do cluster**.

2. Copie as informações de logon SSH e cole o logon em um terminal. Entre no cluster usando a senha que você definiu durante a criação do cluster. Você deve ver as mensagens com boas-vindas ao Ubuntu e ao Spark.

3. Use o comando **Spark-Submit** para executar seu aplicativo em seu cluster HDInsight. Lembre-se de substituir **MyContainer** e **mystorageaccount** no script de exemplo pelos nomes reais do contêiner de BLOB e da conta de armazenamento.

   ```bash
   $SPARK_HOME/bin/spark-submit \
   --master yarn \
   --class org.apache.spark.deploy.dotnet.DotnetRunner \
   wasbs://mycontainer@mystorageaccount.blob.core.windows.net/microsoft-spark-2.3.x-0.6.0.jar \
   wasbs://mycontainer@mystorageaccount.blob.core.windows.net/publish.zip mySparkApp
   ```

   Quando seu aplicativo for executado, você verá a mesma tabela de contagem de palavras da execução local de introdução gravada no console. Parabéns, você executou seu primeiro .NET para Apache Spark aplicativo na nuvem!

## <a name="clean-up-resources"></a>Limpar recursos

O HDInsight salva seus dados no armazenamento do Azure, para que você possa excluir um cluster com segurança quando ele não estiver em uso. Você também é cobrado por um cluster HDInsight, mesmo quando ele não está em uso. Como os encargos para o cluster são muitas vezes maiores do que os encargos para armazenamento, faz sentido, do ponto de vista econômico, excluir os clusters quando não estiverem em uso.

Também é possível selecionar o nome do grupo de recursos para abrir a página do grupo de recursos, e depois selecionar **Excluir grupo de recursos**. Ao excluir o grupo de recursos, você exclui o cluster Spark do HDInsight e a conta de armazenamento padrão.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você implantou seu aplicativo .NET para Apache Spark para o Azure HDInsight. Para saber mais sobre o HDInsight, continue na Documentação do Azure HDInsight.

> [!div class="nextstepaction"]
> [Documentação do HDInsight do Azure](https://docs.microsoft.com/azure/hdinsight/)
