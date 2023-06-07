---
title: Instalar o .NET para Apache Spark em blocos de anotações do Jupyter em clusters Azure HDInsight Spark
description: Saiba como instalar o .NET para Apache Spark nos notebooks Jupyter do Azure HDInsight.
ms.date: 06/25/2020
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: 14babf7a551192b286f309393e3bbff25d4745d5
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617730"
---
# <a name="install-net-for-apache-spark-on-jupyter-notebooks-on-azure-hdinsight-spark-clusters"></a>Instalar o .NET para Apache Spark em blocos de anotações do Jupyter em clusters Azure HDInsight Spark

Este artigo ensina como instalar o .NET para Apache Spark em blocos de anotações do Jupyter em clusters Azure HDInsight Spark. Você pode implantar o .NET para Apache Spark em clusters do Azure HDInsight por meio de uma combinação da linha de comando e do portal do Azure (para obter mais informações, consulte [como implantar um aplicativo .net para Apache Spark no Azure HDInsight](../tutorials/hdinsight-deployment.md)), mas os notebooks fornecem uma experiência mais interativa e iterativa.

Os clusters do Azure HDInsight já vêm com notebooks Jupyter, portanto, tudo o que você precisa fazer é configurar os notebooks Jupyter para executar o .NET para Apache Spark. Para usar o .NET para Apache Spark em seus notebooks Jupyter, é necessário um REPL em C# para executar o código em C# linha por linha e para preservar o estado de execução quando necessário. [Experimente o .net](https://github.com/dotnet/try) seja integrado como o .net repl oficial.

Para habilitar o .NET para Apache Spark por meio da experiência do Jupyter notebooks, você precisa seguir algumas etapas manuais por meio de [Ambari](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-manage-ambari) e enviar [ações de script](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux) no cluster HDInsight Spark.

> [!NOTE]
> Esse recurso é *experimental* e não tem suporte da equipe do HDInsight Spark.

[!INCLUDE [spark-preview-note](../../../includes/spark-preview-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Se você ainda não tiver uma, crie um cluster [Azure HDInsight Spark](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-jupyter-spark-sql-use-portal#create-an-apache-spark-cluster-in-hdinsight) .

1. Visite a [portal do Azure](https://portal.azure.com) e selecione **+ criar um recurso**.

1. Crie um novo recurso de cluster do Azure HDInsight. Selecione **Spark 2,4** e **HDI 4,0** durante a criação do cluster.

## <a name="install-net-for-apache-spark"></a>Instalar .NET para Apache Spark

No portal do Azure, selecione o **cluster HDInsight Spark** criado na etapa anterior.

### <a name="stop-the-livy-server"></a>Parar o servidor Livy

1. No portal, selecione **visão geral**e, em seguida, selecione **Ambari página inicial**. Se solicitado, insira as credenciais de logon para o cluster.

   ![Parar o servidor Livy](./media/hdinsight-notebook-installation/select-ambari.png)

2. Selecione **Spark2** no menu de navegação à esquerda e selecione **LIVY para o servidor Spark2**.

   ![Parar o servidor Livy](./media/hdinsight-notebook-installation/select-livyserver.png)

3. Selecione **hn0... host**.

   ![Parar o servidor Livy](./media/hdinsight-notebook-installation/select-host.png)

4. Selecione as reticências ao lado de **Livy para servidor Spark2** e selecione **parar**. Quando solicitado, selecione **OK** para continuar.

   Pare o Livy para o servidor Spark2.
   ![Parar o servidor Livy](./media/hdinsight-notebook-installation/stop-server.png)

5. Repita as etapas anteriores para **hn1... host**.

### <a name="submit-an-hdinsight-script-action"></a>Enviar uma ação de script do HDInsight

1. O `install-interactive-notebook.sh` é um script que instala o .net para Apache Spark e faz alterações no Apache Livy e sparkmagic. Antes de enviar uma ação de script para o HDInsight, você precisa criar e carregar `install-interactive-notebook.sh` .

   Crie um novo arquivo chamado **install-Interactive-notebook.sh** no computador local e cole o conteúdo do [conteúdo do install-Interactive-notebook.sh](https://raw.githubusercontent.com/dotnet/spark/master/deployment/HDI-Spark/Notebooks/install-interactive-notebook.sh).

   Carregue o script em um [URI](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#understand-script-actions) acessível do cluster HDInsight. Por exemplo, `https://<my storage account>.blob.core.windows.net/<my container>/<some dir>/install-interactive-notebook.sh`.

2. Execute `install-interactive-notebook.sh` no cluster usando as [Ações de Script do HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).

   Volte para o cluster HDI no portal do Azure e selecione ações de **script** nas opções à esquerda. Você envia uma ação de script para implantar o .NET para Apache Spark REPL em seu cluster HDInsight Spark. Use as configurações a seguir:

   |Propriedade  |Descrição  |
   |---------|---------|
   | Tipo de script | Personalizado |
   | Name | *Instalar o .NET para Apache Spark experiência de notebook interativa* |
   | URI do script Bash | O URI para o qual você carregou `install-interactive-notebook.sh`. |
   | Tipo(s) de nó| Cabeçalho e trabalho |
   | Parâmetros | .NET para Apache Spark versão. Você pode verificar o [.net em busca de Apache Spark versões](https://github.com/dotnet/spark/releases). Por exemplo, se você quiser instalar o Sparkdotnet versão 0.6.0, ele seria `0.6.0` .

   Vá para a próxima etapa quando as marcas de seleção verdes aparecerem ao lado do status da ação de script.

### <a name="start-the-livy-server"></a>Iniciar o servidor Livy

Siga as instruções na seção [parar servidor Livy](#stop-the-livy-server) para **Iniciar** (em vez de **parar**) o Livy do Spark2 Server para hosts **hn0** e **hn1**.

### <a name="set-up-spark-default-configurations"></a>Configurar as configurações padrão do Spark

1. No portal, selecione **visão geral**e, em seguida, selecione **Ambari página inicial**. Em caso de solicitação, insira as credenciais de logon do cluster.

2. Selecione **Spark2** e **configurações**. Em seguida, selecione **Spark2 personalizados-padrões**.

   ![Definir configurações](./media/hdinsight-notebook-installation/spark-configs.png)

3. Selecione **Adicionar Propriedade...** para adicionar as configurações padrão do Spark.

   ![Adicionar propriedade](./media/hdinsight-notebook-installation/add-property.png)

   Há três propriedades individuais. Adicione um de cada vez usando o tipo de propriedade **texto** no modo de adição de propriedade única. Verifique se você não tem espaços extras antes ou depois de qualquer uma das chaves/valores.

   * **Propriedade 1**
       * Chaves&ensp;&ensp;`spark.dotnet.shell.command`
       * Valor: `/usr/share/dotnet-tools/dotnet-try,kernel-server,--default-kernel,csharp`

   * **Propriedade 2** Use a versão do .NET para Apache Spark que você incluiu na ação de script anterior.
       * Chaves&ensp;&ensp;`spark.dotnet.packages`
       * Valor: `["nuget: Microsoft.Spark, 0.6.0", "nuget: Microsoft.Spark.Extensions.Delta, 0.6.0"]`

   * **Propriedade 3**
       * Chaves&ensp;&ensp;`spark.dotnet.interpreter`
       * Valor: `try`

   Por exemplo, a imagem a seguir captura a configuração para adicionar a propriedade 1:

   ![Definir configurações](./media/hdinsight-notebook-installation/add-sparkconfig.png)

   Depois de adicionar as três propriedades, selecione **salvar**. Se você vir uma tela de aviso de recomendações de configuração, selecione **continuar mesmo assim**.

4. Reinicie os componentes afetados.

   Depois de adicionar as novas propriedades, você precisa reiniciar os componentes que foram afetados pelas alterações. Na parte superior, selecione **reiniciar**e **reinicie todos os afetados** da lista suspensa.

   ![Definir configurações](./media/hdinsight-notebook-installation/restart-affected.png)

   Quando solicitado, selecione **confirmar reiniciar tudo** para continuar e clique em **OK** para concluir.

## <a name="submit-jobs-through-a-jupyter-notebook"></a>Enviar trabalhos por meio de um notebook Jupyter

Depois de concluir as etapas anteriores, agora você pode enviar seu .NET para trabalhos de Apache Spark por meio de notebooks Jupyter.

1. Crie um novo .NET para Apache Spark notebook. Inicie um notebook Jupyter do seu cluster do HDI no portal do Azure.

   ![Iniciar Jupyter Notebook](./media/hdinsight-notebook-installation/launch-notebook.png)

   Em seguida, selecione **novo**  >  **.net Spark (C#)** para criar um bloco de anotações.

   ![Bloco de anotações do Jupyter](./media/hdinsight-notebook-installation/create-sparkdotnet-notebook.png)

2. Envie trabalhos usando o .NET para Apache Spark.

   Use o seguinte trecho de código para criar um dataframe:

   ```csharp
   var df = spark.Range(0,5);
   df.Show();
   ```

   ![Enviar trabalho do Spark](./media/hdinsight-notebook-installation/create-df.png)

   Use o trecho de código a seguir para registrar uma UDF (função definida pelo usuário) e usar o UDF com quadros de itens:

   ```csharp
   var myawesomeudf = Udf<int, string>((id) => $"hello {id}");
   df.Select(myawesomeudf(df["id"])).Show();
   ```

   ![Enviar trabalho do Spark](./media/hdinsight-notebook-installation/run-udf.png)

## <a name="next-steps"></a>Próximas etapas

* [Implantar um aplicativo .NET para Apache Spark no Azure HDInsight](../tutorials/hdinsight-deployment.md)
* [Documentação do HDInsight](https://docs.microsoft.com/azure/hdinsight/)
