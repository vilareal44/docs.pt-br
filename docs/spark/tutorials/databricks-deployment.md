---
title: Implantar um aplicativo .NET para Apache Spark no Databricks
description: Descubra como implantar um aplicativo do .NET para Apache Spark no Databricks.
ms.date: 06/25/2020
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 66a5493f0084f5fa86c3eb928d2e4a4b4999e764
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86924585"
---
# <a name="tutorial-deploy-a-net-for-apache-spark-application-to-databricks"></a>Tutorial: implantar um aplicativo .NET para Apache Spark no databricks

Este tutorial ensina como implantar seu aplicativo na nuvem por meio de Azure Databricks, uma plataforma de análise baseada em Apache Spark com instalação com um clique, fluxos de trabalho simplificados e espaço de trabalho interativo que permite a colaboração.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
>
> - Criar um workspace do Azure Databricks.
> - Publique seu aplicativo .NET para Apache Spark.
> - Crie um trabalho do Spark e um cluster Spark.
> - Execute seu aplicativo no cluster do Spark.

[!INCLUDE [spark-preview-note](../../../includes/spark-preview-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, execute as seguintes tarefas:

* Se você não tiver uma conta do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/dotnet/).
* Entre no [portal do Azure](https://portal.azure.com/).
* Conclua o [.net para Apache Spark-introdução no tutorial de 10 minutos](https://dotnet.microsoft.com/learn/data/spark-tutorial/intro) .

## <a name="create-an-azure-databricks-workspace"></a>Criar um workspace do Azure Databricks

> [!Note]
> Este tutorial não pode ser realizado usando a **Assinatura de avaliação gratuita do Azure**.
> Se você tiver uma conta gratuita, acesse seu perfil e altere para uma assinatura **pré-paga**. Para saber mais, confira [Conta gratuita do Azure](https://azure.microsoft.com/free/dotnet/). Em seguida, [remova o limite de gastos](https://docs.microsoft.com/azure/billing/billing-spending-limit#why-you-might-want-to-remove-the-spending-limit) e [solicite um aumento de cota](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) para as vCPUs da sua região. Quando você cria seu espaço de trabalho do Azure Databricks, pode selecionar o tipo de preço **Versão de avaliação (Premium - DBUs gratuitas por 14 dias)** para conceder ao espaço de trabalho acesso gratuito aos DBUs do Premium Azure Databricks por 14 dias.

Nesta seção, você deve cria um workspace do Azure Databricks usando o Portal do Azure.

1. No Portal do Azure, selecione **Criar um recurso** > **Análise** > **Azure Databricks**.

   ![Criar um recurso de Azure Databricks no portal do Azure](./media/databricks-deployment/create-databricks-resource.png)

2. Em **Serviço do Azure Databricks**, forneça os valores para criar um workspace do Databricks.

    |Propriedade  |Descrição  |
    |---------|---------|
    |**Nome do workspace**     | Forneça um nome para o seu workspace do Databricks.        |
    |**Assinatura**     | Na lista suspensa, selecione sua assinatura do Azure.        |
    |**Grupo de recursos**     | Especifique se deseja criar um novo grupo de recursos ou usar um existente. Um grupo de recursos é um contêiner que mantém os recursos relacionados a uma solução do Azure. Para obter mais informações, consulte [Visão geral do Grupo de Recursos do Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). |
    |**Localidade**     | Selecione sua região preferida. Para obter informações sobre regiões disponíveis, consulte [Serviços do Azure disponíveis por região](https://azure.microsoft.com/regions/services/).        |
    |**Tipo de preço**     |  Escolha entre o cluster **Standard**, **Premium** ou **Avaliação**. Para saber mais sobre essas camadas, confira [Página de preços do Databricks](https://azure.microsoft.com/pricing/details/databricks/).       |
    |**Rede Virtual**     |   Não       |

3. Selecione **Criar**. A criação do workspace leva alguns minutos. Durante a criação do workspace, você pode exibir o status da implantação em **Notificações**.

## <a name="install-azure-databricks-tools"></a>Instalar ferramentas de Azure Databricks

Você pode usar a **CLI do databricks** para se conectar a Azure Databricks clusters e carregar arquivos para eles no computador local. Os clusters do databricks acessam arquivos por meio do DBFS (sistema de arquivos do databricks).

1. A CLI do databricks requer o Python 3,6 ou superior. Se você já tiver o Python instalado, poderá ignorar esta etapa.

   **Para Windows:**

   [Baixar o Python para Windows](https://www.python.org/ftp/python/3.7.4/python-3.7.4.exe)

   **Para Linux:** O Python vem pré-instalado na maioria das distribuições do Linux. Execute o seguinte comando para ver qual versão você instalou:

   ```bash
   python3 --version
   ```

2. Use o Pip para instalar a CLI do databricks. Python 3,4 e posterior incluem Pip por padrão. Use pip3 para Python 3. Execute o comando a seguir:

   ```bash
   pip3 install databricks-cli
   ```

3. Depois de instalar a CLI do databricks, abra um novo prompt de comando e execute o comando `databricks` . Se você receber um **"databricks" não é reconhecido como um erro de comando interno ou externo**, certifique-se de que você abriu um novo prompt de comando.

## <a name="set-up-azure-databricks"></a>Configurar Azure Databricks

Agora que a CLI do databricks está instalada, você precisa configurar os detalhes de autenticação.

1. Execute o comando da CLI do databricks `databricks configure --token` .

2. Depois de executar o comando configurar, você será solicitado a inserir um host. A URL do host usa o formato: `https://<Location>.azuredatabricks.net` . Por exemplo, se você selecionou **eastus2** durante a criação Azure Databricks serviço, o host seria `https://eastus2.azuredatabricks.net` .

3. Depois de inserir o host, você será solicitado a inserir um token. Na portal do Azure, selecione **Iniciar espaço de trabalho** para iniciar o espaço de trabalho do Azure Databricks.

   ![Iniciar Azure Databricks espaço de trabalho](./media/databricks-deployment/launch-databricks-workspace.png)

4. Na home page do espaço de trabalho, selecione **configurações do usuário**.

   ![Configurações do usuário no espaço de trabalho Azure Databricks](./media/databricks-deployment/databricks-user-settings.png)

5. Na página Configurações do usuário, você pode gerar um novo token. Copie o token gerado e cole-o de volta no prompt de comando.

   ![Gerar novo token de acesso no espaço de trabalho Azure Databricks](./media/databricks-deployment/generate-token.png)

Agora você deve ser capaz de acessar quaisquer Azure Databricks clusters que você criar e carregar arquivos para o DBFS.

## <a name="download-worker-dependencies"></a>Baixar dependências de trabalhador

1. O Microsoft. Spark. Worker ajuda a Apache Spark executar seu aplicativo, como qualquer UDFs (funções definidas pelo usuário) que você possa ter escrito. Baixe [Microsoft. Spark. Worker](https://github.com/dotnet/spark/releases/download/v0.6.0/Microsoft.Spark.Worker.netcoreapp2.1.linux-x64-0.6.0.tar.gz).

2. O *install-Worker.sh* é um script que permite que você copie o .net para Apache Spark arquivos dependentes nos nós do cluster.

   Crie um novo arquivo chamado **install-Worker.sh** no computador local e cole o conteúdo do [install-Worker.sh](https://raw.githubusercontent.com/dotnet/spark/master/deployment/install-worker.sh) localizado no github.

3. O *DB-init.sh* é um script que instala dependências em seu cluster do databricks Spark.

   Crie um novo arquivo chamado **DB-init.sh** no computador local e cole o conteúdo do [DB-init.sh](https://github.com/dotnet/spark/blob/master/deployment/db-init.sh) localizado no github.

   No arquivo que você acabou de criar, defina a `DOTNET_SPARK_RELEASE` variável como `https://github.com/dotnet/spark/releases/download/v0.6.0/Microsoft.Spark.Worker.netcoreapp2.1.linux-x64-0.6.0.tar.gz` . Deixe o restante do arquivo *DB-init.sh* como está.

> [!Note]
> Se você estiver usando o Windows, verifique se as terminações de linha nos scripts *install-Worker.sh* e *DB-init.sh* são de estilo UNIX (LF). Você pode alterar as terminações de linha por meio de editores de texto como o notepad + + e Atom.

## <a name="publish-your-app"></a>Publicar seu aplicativo

Em seguida, você publica o *mySparkApp* criado no [.net para Apache Spark-introdução no tutorial de 10 minutos](https://dotnet.microsoft.com/learn/data/spark-tutorial/intro) para garantir que o cluster Spark tenha acesso a todos os arquivos necessários para executar seu aplicativo.

1. Execute os seguintes comandos para publicar o *mySparkApp*:

   ```dotnetcli
   cd mySparkApp
   dotnet publish -c Release -f netcoreapp3.1 -r ubuntu.16.04-x64
   ```

2. Execute as seguintes tarefas para compactar seus arquivos de aplicativo publicados para que você possa carregá-los facilmente em seu cluster do databricks Spark.

   **No Windows:**

   Navegue até mySparkApp/bin/Release/netcoreapp 3.1/Ubuntu. 16.04-x64. Em seguida, clique com o botão direito do mouse em **publicar** pasta e selecione **Enviar para > pasta compactada (zipada)**. Nomeie a nova pasta **publish.zip**.

   **No Linux, execute o seguinte comando:**

   ```bash
   zip -r publish.zip .
   ```

## <a name="upload-files"></a>Carregar arquivos

Nesta seção, você carrega vários arquivos em DBFS para que o cluster tenha tudo o que precisa para executar seu aplicativo na nuvem. Sempre que você carregar um arquivo para o DBFS, verifique se você está no diretório em que o arquivo está localizado no computador.

1. Execute os comandos a seguir para carregar o *DB-init.sh*, o *install-Worker.sh*e o *Microsoft. Spark. Worker* para o DBFS:

   ```console
   databricks fs cp db-init.sh dbfs:/spark-dotnet/db-init.sh
   databricks fs cp install-worker.sh dbfs:/spark-dotnet/install-worker.sh
   databricks fs cp Microsoft.Spark.Worker.netcoreapp3.1.linux-x64-0.6.0.tar.gz dbfs:/spark-dotnet/Microsoft.Spark.Worker.netcoreapp2.1.linux-x64-0.6.0.tar.gz
   ```

2. Execute os seguintes comandos para carregar os arquivos restantes que o cluster precisará para executar seu aplicativo: a pasta de publicação compactada, *input.txt*e *Microsoft-Spark-2.4. x-0.3.1. jar*.

   ```console
   cd mySparkApp
   databricks fs cp input.txt dbfs:/input.txt

   cd mySparkApp\bin\Release\netcoreapp3.1\ubuntu.16.04-x64 directory
   databricks fs cp publish.zip dbfs:/spark-dotnet/publish.zip
   databricks fs cp microsoft-spark-2.4.x-0.6.0.jar dbfs:/spark-dotnet/microsoft-spark-2.4.x-0.6.0.jar
   ```

## <a name="create-a-job"></a>Criar um trabalho

Seu aplicativo é executado em Azure Databricks por meio de um trabalho que executa o **Spark-Submit**, que é o comando usado para executar o .net para trabalhos do Apache Spark.

1. No espaço de trabalho Azure Databricks, selecione o ícone **trabalhos** e **+ criar trabalho**.

   ![Criar um trabalho de Azure Databricks](./media/databricks-deployment/create-job.png)

2. Escolha um título para seu trabalho e, em seguida, selecione **Configurar Spark-Submit**.

   ![Configurar o Spark-enviar para o trabalho do databricks](./media/databricks-deployment/configure-spark-submit.png)

3. Cole os seguintes parâmetros na configuração do trabalho. Em seguida, selecione **confirmar**.

   ```
   ["--class","org.apache.spark.deploy.dotnet.DotnetRunner","/dbfs/spark-dotnet/microsoft-spark-2.4.x-0.6.0.jar","/dbfs/spark-dotnet/publish.zip","mySparkApp"]
   ```

## <a name="create-a-cluster"></a>Criar um cluster

1. Navegue até seu trabalho e selecione **Editar** para configurar o cluster do trabalho.

2. Defina o cluster para o **Spark 2.4.1**. Em seguida, selecione **Opções avançadas**  >  **scripts de inicialização**. Defina caminho do script de inicialização como `dbfs:/spark-dotnet/db-init.sh` .

   ![Configurar o cluster Spark no Azure Databricks](./media/databricks-deployment/cluster-config.png)

3. Selecione **confirmar** para confirmar as configurações de cluster.

## <a name="run-your-app"></a>Executar o aplicativo

1. Navegue até seu trabalho e selecione **executar agora** para executar seu trabalho em seu cluster Spark recém configurado.

2. Leva alguns minutos para que o cluster do trabalho seja criado. Depois que ele for criado, seu trabalho será enviado e você poderá exibir a saída.

3. Selecione **clusters** no menu à esquerda e, em seguida, o nome e a execução do seu trabalho.

4. Selecione **logs de driver** para exibir a saída do seu trabalho. Quando seu aplicativo concluir a execução, você verá a mesma tabela de contagem de palavras da execução local de introdução gravada no console de saída padrão.

   ![Azure Databricks tabela de saída de trabalho](./media/databricks-deployment/table-output.png)

   Parabéns, você executou seu primeiro .NET para Apache Spark aplicativo na nuvem!

## <a name="clean-up-resources"></a>Limpar os recursos

Se você não precisar mais do espaço de trabalho do databricks, poderá excluir seu recurso de Azure Databricks no portal do Azure. Também é possível selecionar o nome do grupo de recursos para abrir a página do grupo de recursos, e depois selecionar **Excluir grupo de recursos**.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você implantou seu aplicativo .NET para Apache Spark para o Databricks. Para saber mais sobre o Databricks, leia a documentação do Azure Databricks.

> [!div class="nextstepaction"]
> [Documentação do Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/)
