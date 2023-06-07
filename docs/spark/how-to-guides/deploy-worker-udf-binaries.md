---
title: Implantar .NET para Apache Spark trabalho e binários de função definidos pelo usuário
description: Saiba como implantar o .NET para Apache Spark o trabalho e os binários de função definidos pelo usuário.
ms.date: 06/25/2020
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: 672a32c430bd702167a294d2b895ac1ac90bf67e
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617711"
---
# <a name="deploy-net-for-apache-spark-worker-and-user-defined-function-binaries"></a>Implantar .NET para Apache Spark trabalho e binários de função definidos pelo usuário

Este "como" fornece instruções gerais sobre como implantar .NET para Apache Spark trabalho e binários de função definidos pelo usuário. Você aprende quais variáveis de ambiente configurar, bem como alguns parâmetros comumente usados para iniciar aplicativos com o `spark-submit` .

[!INCLUDE [spark-preview-note](../../../includes/spark-preview-note.md)]

## <a name="configurations"></a>Configurações
As configurações mostram as variáveis de ambiente geral e as configurações de parâmetros para implantar o .NET para Apache Spark trabalho e os binários de função definidos pelo usuário.

### <a name="environment-variables"></a>Variáveis de ambiente
Ao implantar trabalhadores e escrever UDFs, há algumas variáveis de ambiente usadas com frequência que talvez você precise definir:

| Variável de ambiente         | Descrição
| :--------------------------- | :----------
| DOTNET_WORKER_DIR            | Caminho em que o <code>Microsoft.Spark.Worker</code> binário foi gerado.</br>Ele é usado pelo driver do Spark e será passado para executores do Spark. Se essa variável não estiver configurada, os executores do Spark pesquisarão o caminho especificado na <code>PATH</code> variável de ambiente.</br>_por exemplo, "C:\bin\Microsoft.Spark.Worker"_
| DOTNET_ASSEMBLY_SEARCH_PATHS | Caminhos separados por vírgulas onde <code>Microsoft.Spark.Worker</code> carregarão assemblies.</br>Observe que, se um caminho começar com ".", o diretório de trabalho será anexado. Se no **modo yarn**, "." representaria o diretório de trabalho do contêiner.</br>_por exemplo, "C:\Users \\ &lt; User name &gt; \\ &lt; mysparkapp &gt; \bin\Debug \\ &lt; dotnet Version &gt; "_
| DOTNET_WORKER_DEBUG          | Se você quiser <a href="https://github.com/dotnet/spark/blob/master/docs/developer-guide.md#debugging-user-defined-function-udf">depurar um UDF</a>, defina essa variável de ambiente como <code>1</code> antes de executar <code>spark-submit</code> .

### <a name="parameter-options"></a>Opções de parâmetro
Depois que o aplicativo Spark for [agrupado](https://spark.apache.org/docs/latest/submitting-applications.html#bundling-your-applications-dependencies), você poderá iniciá-lo usando `spark-submit` . A tabela a seguir mostra algumas das opções mais usadas:

| Nome do Parâmetro        | Descrição
| :---------------------| :----------
| --classe               | O ponto de entrada para seu aplicativo.</br>_por exemplo, org. Apache. Spark. Deploy. dotnet. DotnetRunner_
| --Mestre              | A <a href="https://spark.apache.org/docs/latest/submitting-applications.html#master-urls">URL mestra</a> do cluster.</br>_por exemplo, yarn_
| --modo de implantação         | Se o driver deve ser implantado nos nós de trabalho ( <code>cluster</code> ) ou localmente como um cliente externo ( <code>client</code> ).</br>Padrão: <code>client</code>
| --conf                | Propriedade de configuração do Spark arbitrária no <code>key=value</code> formato.</br>_por exemplo, Spark. yarn. appMasterEnv. DOTNET_WORKER_DIR = .\worker\Microsoft.Spark.Worker_
| --arquivos               | Lista separada por vírgulas de arquivos a serem colocados no diretório de trabalho de cada executor.<br/><ul><li>Observe que essa opção só é aplicável ao modo yarn.</li><li>Ele dá suporte à especificação de nomes de arquivo com # semelhante ao Hadoop.</br></ul>_por exemplo, <code>myLocalSparkApp.dll#appSeen.dll</code> . Seu aplicativo deve usar o nome como <code>appSeen.dll</code> referência <code>myLocalSparkApp.dll</code> ao ser executado em yarn._</li>
| --arquivos mortos          | Lista separada por vírgulas de arquivos mortos a serem extraídos no diretório de trabalho de cada executor.</br><ul><li>Observe que essa opção só é aplicável ao modo yarn.</li><li>Ele dá suporte à especificação de nomes de arquivo com # semelhante ao Hadoop.</br></ul>_por exemplo, <code>hdfs://&lt;path to your worker file&gt;/Microsoft.Spark.Worker.zip#worker</code> . Isso irá copiar e extrair o arquivo zip para a <code>worker</code> pasta._</li>
| aplicativo-jar       | Caminho para um jar agrupado, incluindo seu aplicativo e todas as dependências.</br>_por exemplo, &lt; caminho HDFS://para seu jar &gt; /Microsoft-Spark- &lt; versão &gt; . jar_
| argumentos do aplicativo | Argumentos passados para o método Main de sua classe principal, se houver.</br>_por exemplo, HDFS:// &lt; caminho para seu aplicativo &gt; / &lt; seu aplicativo &gt; . zip &lt; seus args de &gt; &lt; aplicativo de nome de aplicativo&gt;_

> [!NOTE]
> Especifique todos os `--options` antes de `application-jar` iniciar aplicativos com `spark-submit` , caso contrário, eles serão ignorados. Para obter mais informações, consulte [ `spark-submit` Opções](https://spark.apache.org/docs/latest/submitting-applications.html) e [executando o Spark em detalhes do yarn](https://spark.apache.org/docs/latest/running-on-yarn.html).

## <a name="frequently-asked-questions"></a>Perguntas frequentes
### <a name="when-i-run-a-spark-app-with-udfs-i-get-a-filenotfoundexception-error-what-should-i-do"></a>Quando executo um aplicativo Spark com UDFs, obtenho um erro ' FileNotFoundException '. O que devo fazer?
> **Erro:** [ERROR] [TaskRunner] [0] ProcessStream () falhou com a exceção: System. IO. FileNotFoundException: assembly "MySparkApp, Version = 1.0.0.0, Culture = neutral, PublicKeyToken = null" arquivo não encontrado: "mySparkApp.dll"

**Resposta:** Verifique se a `DOTNET_ASSEMBLY_SEARCH_PATHS` variável de ambiente está definida corretamente. Deve ser o caminho que contém seu `mySparkApp.dll` .

### <a name="after-i-upgraded-my-net-for-apache-spark-version-and-reset-the-dotnet_worker_dir-environment-variable-why-do-i-still-get-the-following-ioexception-error"></a>Depois de atualizar o .NET para Apache Spark versão e redefinir a `DOTNET_WORKER_DIR` variável de ambiente, por que eu ainda obtenho o seguinte `IOException` erro?
> **Erro:** Tarefa perdida 0,0 no estágio 11,0 (TID 24, localhost, Driver de executor): Java. IO. IOException: não é possível executar o programa "Microsoft.Spark.Worker.exe": erro CreateProcess = 2, o sistema não pode localizar o arquivo especificado.

**Resposta:** Tente reiniciar a janela do PowerShell (ou outras janelas de comando) primeiro para que possa obter os valores de variáveis de ambiente mais recentes. Em seguida, inicie o programa.

### <a name="after-submitting-my-spark-application-i-get-the-error-systemtypeloadexception-could-not-load-type-systemruntimeremotingcontextscontext"></a>Depois de enviar meu aplicativo Spark, obtenho o erro `System.TypeLoadException: Could not load type 'System.Runtime.Remoting.Contexts.Context'` .
> **Erro:** [ERROR] [TaskRunner] [0] ProcessStream () falhou com a exceção: System. TypeLoadException: não foi possível carregar o tipo ' System. Runtime. Remoting. detexts. Context ' do assembly ' mscorlib, Version = 4.0.0.0, Culture = neutral, PublicKeyToken =... '.

**Resposta:** Verifique a `Microsoft.Spark.Worker` versão que você está usando. Há duas versões: **.NET Framework 4.6.1** e **.NET Core 2.1. x**. Nesse caso, `Microsoft.Spark.Worker.net461.win-x64-<version>` (que você pode [baixar](https://github.com/dotnet/spark/releases)) deve ser usado, pois `System.Runtime.Remoting.Contexts.Context` é apenas para .NET Framework.

### <a name="how-do-i-run-my-spark-application-with-udfs-on-yarn-which-environment-variables-and-parameters-should-i-use"></a>Como fazer executar meu aplicativo Spark com UDFs no YARN? Quais variáveis e parâmetros de ambiente devo usar?

**Resposta:** Para iniciar o aplicativo Spark no YARN, as variáveis de ambiente devem ser especificadas como `spark.yarn.appMasterEnv.[EnvironmentVariableName]` . Veja abaixo como exemplo usando `spark-submit` :

```powershell
spark-submit \
--class org.apache.spark.deploy.dotnet.DotnetRunner \
--master yarn \
--deploy-mode cluster \
--conf spark.yarn.appMasterEnv.DOTNET_WORKER_DIR=./worker/Microsoft.Spark.Worker-<version> \
--conf spark.yarn.appMasterEnv.DOTNET_ASSEMBLY_SEARCH_PATHS=./udfs \
--archives hdfs://<path to your files>/Microsoft.Spark.Worker.net461.win-x64-<version>.zip#worker,hdfs://<path to your files>/mySparkApp.zip#udfs \
hdfs://<path to jar file>/microsoft-spark-2.4.x-<version>.jar \
hdfs://<path to your files>/mySparkApp.zip mySparkApp
```

## <a name="next-steps"></a>Próximas etapas

* [Introdução ao .NET para Apache Spark](../tutorials/get-started.md)
* [Depurar um aplicativo .NET para Apache Spark no Windows](debug.md)
