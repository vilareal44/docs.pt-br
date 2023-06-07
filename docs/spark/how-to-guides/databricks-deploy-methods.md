---
title: Enviar um trabalho .NET para Apache Spark para o databricks
description: Saiba como enviar um .NET para Apache Spark trabalho para o databricks usando Spark-Submit e Set jar.
ms.date: 06/25/2020
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: bebd170a689d8ae56aa6c55486d70354da2437ea
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617763"
---
# <a name="submit-a-net-for-apache-spark-job-to-databricks"></a>Enviar um trabalho .NET para Apache Spark para o databricks

Você pode executar o .NET para Apache Spark trabalhos em clusters do databricks, mas ele não está disponível de pronto para uso. Há duas maneiras de implantar seu .NET para Apache Spark trabalho no databricks: `spark-submit` e definir jar.

[!INCLUDE [spark-preview-note](../../../includes/spark-preview-note.md)]

## <a name="deploy-using-spark-submit"></a>Implantar usando Spark-Submit

Você pode usar o comando [Spark-Submit](https://spark.apache.org/docs/latest/submitting-applications.html) para enviar o .net para trabalhos do Apache Spark para o databricks. `spark-submit`permite o envio somente para um cluster que é criado sob demanda.

1. Navegue até o espaço de trabalho do databricks e crie um trabalho. Escolha um título para seu trabalho e, em seguida, selecione **Configurar Spark-Submit**. Cole os seguintes parâmetros na configuração do trabalho e, em seguida, selecione **confirmar**.

    ```
    ["--files","/dbfs/<path-to>/<app assembly/file to deploy to worker>","--class","org.apache.spark.deploy.dotnet.DotnetRunner","/dbfs/<path-to>/microsoft-spark-<spark_majorversion.spark_minorversion.x>-<spark_dotnet_version>.jar","/dbfs/<path-to>/<app name>.zip","<app bin name>","app arg1","app arg2"]
    ```

    > [!NOTE]
    > Atualize o conteúdo do parâmetro acima com base em seus arquivos e configurações específicos. Por exemplo, referencie a versão do arquivo JAR do Microsoft. Spark que você carregou em DBFS e use o nome apropriado do seu aplicativo e o arquivo zip do aplicativo publicado.

2. Navegue até seu trabalho e selecione **Editar** para configurar o cluster do trabalho. Defina a versão Databricks Runtime com base na versão do Apache Spark que você deseja usar em sua implantação. Em seguida, selecione **Opções avançadas > scripts de inicialização**e defina caminho do script de inicialização como `dbfs:/spark-dotnet/db-init.sh` . Selecione **confirmar** para confirmar as configurações de cluster.

3. Navegue até seu trabalho e selecione **executar agora** para executar seu trabalho em seu cluster Spark recém configurado. Leva alguns minutos para que o cluster do trabalho seja criado. Depois que ele for criado, seu trabalho será enviado. Você pode exibir a saída selecionando **clusters** no menu à esquerda do seu espaço de trabalho do databricks e, em seguida, selecionar **logs de driver**.

## <a name="deploy-using-set-jar"></a>Implantar usando Set jar

Como alternativa, você pode usar [set jar](https://docs.microsoft.com/azure/databricks/jobs#--create-a-job) em seu espaço de trabalho do databricks para enviar Apache Spark trabalhos do .net para o databricks. *Set jar* permite o envio de trabalhos para um cluster ativo existente.

### <a name="one-time-setup"></a>Configuração única

1. Navegue até o cluster do databricks e selecione **trabalhos** no menu do lado esquerdo, seguido por **set jar**.

2. Carregue o apropriado `microsoft-spark-<spark-version>-<spark-dotnet-version>.jar` .

3. Modifique os seguintes parâmetros para incluir o nome correto para o executável que você publicou no lugar de `<your-app-name>` :

    ```
    Main Class: org.apache.spark.deploy.dotnet.DotnetRunner
    Arguments /dbfs/apps/<your-app-name>.zip <your-app-name>
    ```

4. Configure o cluster para apontar para um cluster existente para o qual você já definiu o script de inicialização.

### <a name="publish-and-run-your-app"></a>Publicar e executar seu aplicativo

1. Verifique se você publicou seu aplicativo e se o código do aplicativo não usa `SparkSession.Stop()` .

2. Use a [CLI do databricks](https://docs.microsoft.com/azure/databricks/dev-tools/databricks-cli) para carregar seu aplicativo em seu cluster do databricks. Por exemplo, use o seguinte comando para carregar seu aplicativo publicado em seu cluster:

    ```console
    cd <path-to-your-app-publish-directory>
    databricks fs cp <your-app-name>.zip dbfs:/apps/<your-app-name>.zip
    ```

3. Se você tiver funções definidas pelo usuário em seu aplicativo, os assemblies de aplicativo, como DLLs que contêm funções definidas pelo usuário, juntamente com suas dependências, precisam ser colocados no diretório de trabalho de cada *Microsoft. Spark. Worker*.

    Carregue seus assemblies de aplicativo em seu cluster do databricks:

    ```console
    cd <path-to-your-app-publish-directory>
    databricks fs cp <assembly>.dll dbfs:/apps/dependencies
    ```

    Remova a marca de comentário e modifique a seção de dependências do aplicativo em [DB-init.sh](https://github.com/dotnet/spark/blob/master/deployment/db-init.sh) para apontar para o caminho de dependências do aplicativo. Em seguida, carregue o *DB-init.sh* atualizado em seu cluster:

    ```console
    cd <path-to-db-init-and-install-worker>
    databricks fs cp db-init.sh dbfs:/spark-dotnet/db-init.sh
    ```

4. Navegue até **cluster do databricks > trabalhos > [nome-do-trabalho] > executar agora** para executar seu trabalho.

## <a name="next-steps"></a>Próximas etapas

* [Introdução ao .NET para Apache Spark](../tutorials/get-started.md)
* [Implantar um aplicativo .NET para Apache Spark no Databricks](../tutorials/databricks-deployment.md)
* [Documentação do Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/)
