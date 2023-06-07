---
title: 'Tutorial: analisar comentários do site-classificação binária'
description: Este tutorial mostra como criar um aplicativo de console do .NET Core que classifica sentimentos de comentários de um site e executa a ação adequada. O classificador binário de sentimento usa C# no Visual Studio.
ms.date: 06/30/2020
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: da972d793570a8dd6b906762640bd6bfe5531a5b
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87557158"
---
# <a name="tutorial-analyze-sentiment-of-website-comments-with-binary-classification-in-mlnet"></a>Tutorial: analisar o sentimentos dos comentários do site com a classificação binária no ML.NET

Este tutorial mostra como criar um aplicativo de console do .NET Core que classifica sentimentos de comentários de um site e executa a ação adequada. O classificador binário de sentimento usa C# no Visual Studio 2017.

Neste tutorial, você aprenderá como:
> [!div class="checklist"]
>
> - Criar um aplicativo de console
> - Preparar dados
> - Carregar os dados
> - Criar e treinar o modelo
> - Avaliar o modelo
> - Usar o modelo para fazer uma previsão
> - Confira os resultados

Você pode encontrar o código-fonte para este tutorial no repositório [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/SentimentAnalysis).

## <a name="prerequisites"></a>Pré-requisitos

- [Visual Studio 2017 versão 15,6 ou posterior](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) com a carga de trabalho "desenvolvimento de plataforma cruzada do .NET Core" instalada

- [Conjunto de dados de Sentenças de sentimentos rotuladas da UCI](http://archive.ics.uci.edu/ml/machine-learning-databases/00331/sentiment%20labelled%20sentences.zip) (arquivo zip)

## <a name="create-a-console-application"></a>Criar um aplicativo de console

1. Crie um **Aplicativo de console do .NET Core** chamado "SentimentAnalysis".

2. Crie um diretório chamado *dados* em seu projeto para salvar os arquivos do conjunto de dados.

3. Instalar o **Pacote NuGet Microsoft.ML**:

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    No Gerenciador de Soluções, clique com o botão direito do mouse no seu projeto e selecione **Gerenciar Pacotes NuGet**. Escolha "nuget.org" como a origem do pacote e, em seguida, selecione a guia **procurar** . procure **Microsoft.ml**, selecione o pacote desejado e, em seguida, selecione o botão **instalar** . Prossiga com a instalação concordando com os termos de licença do pacote que você escolher.

## <a name="prepare-your-data"></a>Preparar seus dados

> [!NOTE]
> Os conjuntos de dados deste tutorial são de "From Group to Individual Labels using Deep Features”, Kotzias et. al,. KDD 2015 e hospedado na UCI Machine Learning Repository-Dua, D. e Karra Taniskidou, E. (2017). UCI Machine Learning Repository [http://archive.ics.uci.edu/ml]. Irvine, CA: University of California, School of Information and Computer Science.

1. Baixe o [arquivo zip do conjunto de dados de Sentenças de sentimentos rotuladas da UCI](http://archive.ics.uci.edu/ml/machine-learning-databases/00331/sentiment%20labelled%20sentences.zip) e descompacte-o.

2. Copie o arquivo `yelp_labelled.txt` para o diretório *Dados* que você criou.

3. No Gerenciador de Soluções, clique com o botão direito do mouse no arquivo `yelp_labeled.txt` e selecione **Propriedades**. Em **avançado**, altere o valor de **copiar para diretório de saída** para **copiar se mais recente**.

### <a name="create-classes-and-define-paths"></a>Criar classes e definir demarcadores

1. Adicione as seguintes instruções `using` adicionais ao início do arquivo *Program.cs*: 

    [!code-csharp[AddUsings](./snippets/sentiment-analysis/csharp/Program.cs#AddUsings "Add necessary usings")]

1. Adicione o seguinte código à linha à direita acima do `Main` método, para criar um campo para manter o caminho do arquivo do conjunto de código baixado recentemente:

    [!code-csharp[Declare global variables](./snippets/sentiment-analysis/csharp/Program.cs#DeclareGlobalVariables "Declare global variables")]

1. Em seguida, crie classes para os dados de entrada e as previsões. Adicione uma nova classe ao seu projeto:

    - No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **Novo Item**.

    - Na caixa de diálogo **Adicionar Novo Item**, selecione **Classe** e altere o campo **Nome** para *SentimentData.cs*. Em seguida, selecione o botão **Adicionar**.

1. O arquivo *SentimentData.cs* é aberto no editor de códigos. Adicione a seguinte instrução `using` acima de *SentimentData.cs*:

    [!code-csharp[AddUsings](./snippets/sentiment-analysis/csharp/SentimentData.cs#AddUsings "Add necessary usings")]

1. Remova a definição de classe existente e adicione o seguinte código, que possui duas classes `SentimentData` e `SentimentPrediction`, ao arquivo *SentimentData.cs*:

    [!code-csharp[DeclareTypes](./snippets/sentiment-analysis/csharp/SentimentData.cs#DeclareTypes "Declare data record types")]

### <a name="how-the-data-was-prepared"></a>Como os dados foram preparados

A classe de conjunto de dados de entrada, `SentimentData`, tem um `string` para comentários do usuário (`SentimentText`) e um valor `bool` (`Sentiment`) de 1 (positivo) ou 0 (negativo) para sentimento. Ambos os campos têm atributos [LoadColumn](xref:Microsoft.ML.Data.LoadColumnAttribute.%23ctor%28System.Int32%29) anexados a eles, que descrevem a ordem do arquivo de dados de cada campo.  Além disso, a propriedade `Sentiment` tem um atributo [ColumnName](xref:Microsoft.ML.Data.ColumnNameAttribute.%23ctor%2A) para designá-lo como o campo `Label`. O arquivo de exemplo a seguir não possui uma linha de cabeçalho e tem a seguinte aparência:

|SentimentText                         |Sentimento (rótulo) |
|--------------------------------------|----------|
|A garçonete foi um pouco lenta no serviço.|    0     |
|A torrada não é boa.                    |    0     |
|Wow... Adorei este lugar.              |    1     |
|O serviço era muito rápido.              |    1     |

`SentimentPrediction` é a classe de previsão usada após o treinamento do modelo. Ela herda `SentimentData` de modo que a entrada `SentimentText` possa ser exibida junto com a previsão de saída. O booliano `Prediction` é o valor que o modelo prevê quando fornecido com a nova entrada `SentimentText`.

A classe de saída `SentimentPrediction` contém duas outras propriedades calculadas pelo modelo: `Score` – a pontuação bruta calculada pelo modelo e `Probability` – a pontuação calibrada para a probabilidade de o texto ter sentimento positivo.

Para este tutorial, a propriedade mais importante é `Prediction`.

## <a name="load-the-data"></a>Carregar os dados

Os dados do ML.NET são representados como uma [classe IDataView](xref:Microsoft.ML.IDataView). `IDataView` é uma maneira flexível e eficiente de descrever dados tabulares (numéricos e texto). Os dados podem ser carregados de um arquivo de texto ou em tempo real (por exemplo, banco de dados SQL ou arquivos de log) para um objeto `IDataView`.

A [classe MLContext](xref:Microsoft.ML.MLContext) é um ponto de partida para todas as operações do ML.NET. Inicializar `mlContext` cria um novo ambiente do ML.NET que pode ser compartilhado entre os objetos do fluxo de trabalho de criação de modelo. Ele é semelhante, conceitualmente, a `DBContext` no Entity Framework.

Você prepara o aplicativo e, em seguida, carrega os dados:

1. Substitua a linha `Console.WriteLine("Hello World!")` no método `Main` pelo seguinte código para declarar e inicializar a variável mlContext:

    [!code-csharp[CreateMLContext](./snippets/sentiment-analysis/csharp/Program.cs#CreateMLContext "Create the ML Context")]

2. Adicione o seguinte como a linha seguinte de código no método `Main()`:

    [!code-csharp[CallLoadData](./snippets/sentiment-analysis/csharp/Program.cs#CallLoadData)]

3. Crie o método `LoadData()`, logo após o método `Main()`, usando o seguinte código:

    ```csharp
    public static TrainTestData LoadData(MLContext mlContext)
    {

    }
    ```

    O método `LoadData()` executa as seguintes tarefas:

    - Carrega os dados.
    - Divide o conjunto de dados carregado em conjuntos de treinamento e teste.
    - Retorna os conjuntos de treinamento e teste.

4. Adicione o seguinte código como a primeira linha do método `LoadData()`:

    [!code-csharp[LoadData](./snippets/sentiment-analysis/csharp/Program.cs#LoadData "loading dataset")]

    O método [LoadFromTextFile ()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29) define o esquema de dados e lê o arquivo. Ele usa as variáveis de caminho de dados e retorna uma `IDataView`.

### <a name="split-the-dataset-for-model-training-and-testing"></a>Dividir o conjunto de dados para o modelo de treinamento e de teste

Ao preparar um modelo, você usa parte do conjunto de dados para treiná-lo e parte do conjunto de dados para testar a precisão do modelo.

1. Para dividir os dados carregados nos conjuntos de dados necessários, adicione o seguinte código como a próxima linha no método `LoadData()`:

    [!code-csharp[SplitData](./snippets/sentiment-analysis/csharp/Program.cs#SplitData "Split the Data")]

    O código anterior usa o método [TrainTestSplit ()](xref:Microsoft.ML.DataOperationsCatalog.TrainTestSplit%2A) para dividir o DataSet carregado em conjuntos de valores de teste e de treinamento e retorná-los na <xref:Microsoft.ML.DataOperationsCatalog.TrainTestData> classe. Especifique o percentual de dados do conjunto de teste com o parâmetro `testFraction`. O padrão é 10% e, nesse caso, você usa 20% para avaliar mais dados.

2. Retorne o `splitDataView` no final do método `LoadData()`:

    [!code-csharp[ReturnSplitData](./snippets/sentiment-analysis/csharp/Program.cs#ReturnSplitData)]

## <a name="build-and-train-the-model"></a>Criar e treinar o modelo

1. Adicione a seguinte chamada ao método `BuildAndTrainModel` como a próxima linha de código no método `Main()`:

    [!code-csharp[CallBuildAndTrainModel](./snippets/sentiment-analysis/csharp/Program.cs#CallBuildAndTrainModel)]

    O método `BuildAndTrainModel()` executa as seguintes tarefas:

    - Extrai e transforma os dados.
    - Treina o modelo.
    - Prevê o sentimento com base nos dados de teste.
    - Retorna o modelo.

2. Crie o método `BuildAndTrainModel()`, logo após o método `Main()`, usando o seguinte código:

    ```csharp
    public static ITransformer BuildAndTrainModel(MLContext mlContext, IDataView splitTrainSet)
    {

    }
    ```

### <a name="extract-and-transform-the-data"></a>Extrair e transformar os dados

1. Chame `FeaturizeText` como a próxima linha de código:

    [!code-csharp[FeaturizeText](./snippets/sentiment-analysis/csharp/Program.cs#FeaturizeText "Featurize the text")]

    O método `FeaturizeText()` no código anterior converte a coluna de texto (`SentimentText`) em uma coluna `Features` do tipo chave numérica usada pelo algoritmo de aprendizado de máquina, adicionando-a como uma nova coluna do conjunto de dados:

    |SentimentText                         |Sentimento |Recursos              |
    |--------------------------------------|----------|----------------------|
    |A garçonete foi um pouco lenta no serviço.|    0     |[0,76, 0,65, 0,44, …] |
    |A torrada não é boa.                    |    0     |[0,98, 0,43, 0,54, …] |
    |Wow... Adorei este lugar.              |    1     |[0,35, 0,73, 0,46, …] |
    |O serviço era muito rápido.              |    1     |[0,39, 0, 0,75, …]    |

### <a name="add-a-learning-algorithm"></a>Adicionar um algoritmo de aprendizado

Este aplicativo usa um algoritmo de classificação que categoriza itens ou linhas de dados. O aplicativo categoriza comentários de site como positivos ou negativos, portanto, use a tarefa de classificação binária.

Acrescente a tarefa de aprendizado de máquina às definições de transformação de dados adicionando o seguinte como a próxima linha de código em `BuildAndTrainModel()`:

[!code-csharp[SdcaLogisticRegressionBinaryTrainer](./snippets/sentiment-analysis/csharp/Program.cs#AddTrainer "Add a SdcaLogisticRegressionBinaryTrainer")]

O [SdcaLogisticRegressionBinaryTrainer](xref:Microsoft.ML.Trainers.SdcaLogisticRegressionBinaryTrainer) é o algoritmo de treinamento de classificação. Ele é acrescentado ao `estimator` e aceita o parâmetro `SentimentText` (`Features`) personalizado e o parâmetro de entrada `Label` para aprender com os dados históricos.

### <a name="train-the-model"></a>Treinar o modelo

Ajuste o modelo aos dados `splitTrainSet` e retorne o modelo treinado adicionando o seguinte como a próxima linha de código no método `BuildAndTrainModel()`:

[!code-csharp[TrainModel](./snippets/sentiment-analysis/csharp/Program.cs#TrainModel "Train the model")]

O método [Fit()](xref:Microsoft.ML.Trainers.MatrixFactorizationTrainer.Fit%28Microsoft.ML.IDataView,Microsoft.ML.IDataView%29) treina o modelo transformando o conjunto de dados e aplicando o treinamento.

### <a name="return-the-model-trained-to-use-for-evaluation"></a>Retornar o modelo treinado a ser usado para avaliação

 Retorne o modelo no final do método `BuildAndTrainModel()`:

[!code-csharp[ReturnModel](./snippets/sentiment-analysis/csharp/Program.cs#ReturnModel "Return the model")]

## <a name="evaluate-the-model"></a>Avaliar o modelo

Depois que o modelo for treinado, use seus dados de teste para validar o desempenho do modelo.

1. Crie o método `Evaluate()`, logo após `BuildAndTrainModel()`, com o código a seguir:

    ```csharp
    public static void Evaluate(MLContext mlContext, ITransformer model, IDataView splitTestSet)
    {

    }
    ```

    O método `Evaluate()` executa as seguintes tarefas:

    - Carrega o conjunto de dados de teste.
    - Cria o avaliador BinaryClassification.
    - Avalia o modelo e cria métricas.
    - Exibe as métricas.

2. Adicione uma chamada ao novo método a partir do método `Main()`, logo abaixo da chamada do método `BuildAndTrainModel()`, usando o seguinte código:

    [!code-csharp[CallEvaluate](./snippets/sentiment-analysis/csharp/Program.cs#CallEvaluate "Call the Evaluate method")]

3. Transforme os dados `splitTestSet` adicionando o seguinte código a `Evaluate()`:

    [!code-csharp[PredictWithTransformer](./snippets/sentiment-analysis/csharp/Program.cs#TransformData "Predict using the Transformer")]

    O código anterior usa o método [Transform](xref:Microsoft.ML.ITransformer.Transform%2A) para fazer previsões para várias linhas de entrada fornecidas por um conjunto de dados de teste.

4. Avalie o modelo adicionando o seguinte como a próxima linha de código no método `Evaluate()`:

    [!code-csharp[ComputeMetrics](./snippets/sentiment-analysis/csharp/Program.cs#Evaluate "Compute Metrics")]

Uma vez que você tem o conjunto de previsão (`predictions`), o método [Evaluate()](xref:Microsoft.ML.BinaryClassificationCatalog.Evaluate%2A) avalia o modelo, que compara os valores previstos com os `Labels` reais no conjunto de dados de teste e retorna um objeto [CalibratedBinaryClassificationMetrics](xref:Microsoft.ML.Data.CalibratedBinaryClassificationMetrics) sobre o desempenho do modelo.

### <a name="displaying-the-metrics-for-model-validation"></a>Exibir as métricas para validação de modelo

Use o código a seguir para exibir as métricas:

[!code-csharp[DisplayMetrics](./snippets/sentiment-analysis/csharp/Program.cs#DisplayMetrics "Display selected metrics")]

- A métrica `Accuracy` obtém a precisão de um modelo, que é a proporção de previsões corretas no conjunto de teste.

- A métrica `AreaUnderRocCurve` indica a confiabilidade do modelo ao classificar corretamente as classes positivas e negativas. Você deseja que o `AreaUnderRocCurve` seja o mais próximo possível de um.

- A métrica `F1Score` obtém a pontuação F1 do modelo, que é uma medida do equilíbrio entre [precisão](../resources/glossary.md#precision) e [recall](../resources/glossary.md#recall).  Você deseja que o `F1Score` seja o mais próximo possível de um.

### <a name="predict-the-test-data-outcome"></a>Prever o resultado dos dados de teste

1. Crie o método `UseModelWithSingleItem()`, logo após o método `Evaluate()`, usando o seguinte código:

    ```csharp
    private static void UseModelWithSingleItem(MLContext mlContext, ITransformer model)
    {

    }
    ```

    O método `UseModelWithSingleItem()` executa as seguintes tarefas:

    - Cria um único comentário dos dados de teste.
    - Prevê o sentimento com base nos dados de teste.
    - Combina dados de teste e previsões para relatórios.
    - Exibe os resultados previstos.

2. Adicione uma chamada ao novo método a partir do método `Main()`, logo abaixo da chamada do método `Evaluate()`, usando o seguinte código:

    [!code-csharp[CallUseModelWithSingleItem](./snippets/sentiment-analysis/csharp/Program.cs#CallUseModelWithSingleItem "Call the UseModelWithSingleItem method")]

3. Adicione o seguinte código para criar a primeira linha no método `UseModelWithSingleItem()`:

    [!code-csharp[CreatePredictionEngine](./snippets/sentiment-analysis/csharp/Program.cs#CreatePredictionEngine1 "Create the PredictionEngine")]

    O [PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) é uma API de conveniência, que permite que você execute uma previsão em uma única instância de dados. [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602)Não é thread-safe. É aceitável usar em ambientes de protótipo ou de thread único. Para melhorar o desempenho e a segurança de thread em ambientes de produção, use o `PredictionEnginePool` serviço, que cria um [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601) dos [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) objetos para uso em todo o aplicativo. Consulte este guia sobre como [usar o `PredictionEnginePool` em uma API Web do ASP.NET Core](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application).

    > [!NOTE]
    > A extensão de serviço `PredictionEnginePool` está atualmente em versão prévia.

4. Adicione um comentário para testar as previsões do modelo treinado no método `UseModelWithSingleItem()` ao criar uma instância de `SentimentData`:

    [!code-csharp[PredictionData](./snippets/sentiment-analysis/csharp/Program.cs#CreateTestIssue1 "Create test data for single prediction")]

5. Passe os dados do comentário de teste para o [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) adicionando o seguinte como as próximas linhas de código no `UseModelWithSingleItem()` método:

    [!code-csharp[Predict](./snippets/sentiment-analysis/csharp/Program.cs#Predict "Create a prediction of sentiment")]

    A função [Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) faz uma previsão em uma única coluna de dados.

6. Exiba `SentimentText` e a previsão de sentimento correspondente usando o código a seguir:

    [!code-csharp[OutputPrediction](./snippets/sentiment-analysis/csharp/Program.cs#OutputPrediction "Display prediction output")]

## <a name="use-the-model-for-prediction"></a>Usar o modelo para previsão

### <a name="deploy-and-predict-batch-items"></a>Implantar e prever itens em lotes

1. Crie o método `UseModelWithBatchItems()`, logo após o método `UseModelWithSingleItem()`, usando o seguinte código:

    ```csharp
    public static void UseModelWithBatchItems(MLContext mlContext, ITransformer model)
    {

    }
    ```

    O método `UseModelWithBatchItems()` executa as seguintes tarefas:

    - Cria dados de teste em lote.
    - Prevê o sentimento com base nos dados de teste.
    - Combina dados de teste e previsões para relatórios.
    - Exibe os resultados previstos.

2. Adicione uma chamada ao novo método a partir do método `Main`, logo abaixo da chamada do método `UseModelWithSingleItem()`, usando o seguinte código:

    [!code-csharp[CallPredictModelBatchItems](./snippets/sentiment-analysis/csharp/Program.cs#CallUseModelWithBatchItems "Call the CallUseModelWithBatchItems method")]

3. Adicione alguns comentários para testar as previsões do modelo treinado no método `UseModelWithBatchItems()`:

    [!code-csharp[PredictionData](./snippets/sentiment-analysis/csharp/Program.cs#CreateTestIssues "Create test data for predictions")]

### <a name="predict-comment-sentiment"></a>Prever sentimento de comentário

Use o modelo para prever o sentimento dos dados de comentários usando o método [Transform](xref:Microsoft.ML.ITransformer.Transform%2A):

[!code-csharp[Predict](./snippets/sentiment-analysis/csharp/Program.cs#Prediction "Create predictions of sentiments")]

### <a name="combine-and-display-the-predictions"></a>Combinar e exibir as previsões

Crie um cabeçalho para as previsões usando o seguinte código:

[!code-csharp[OutputHeaders](./snippets/sentiment-analysis/csharp/Program.cs#AddInfoMessage "Display prediction outputs")]

Como `SentimentPrediction` é herdado de `SentimentData`, o método `Transform()` preencheu `SentimentText` com os campos previstos. Conforme o processo do ML.NET processa, cada componente adiciona colunas e isso facilita a exibição dos resultados:

[!code-csharp[DisplayPredictions](./snippets/sentiment-analysis/csharp/Program.cs#DisplayResults "Display the predictions")]

## <a name="results"></a>Resultados

Seus resultados devem ser semelhantes aos seguintes. Durante o processamento, as mensagens são exibidas. Você pode ver avisos ou mensagens de processamento. Eles foram removidos dos seguintes resultados para maior clareza.

```console
Model quality metrics evaluation
--------------------------------
Accuracy: 83.96%
Auc: 90.51%
F1Score: 84.04%

=============== End of model evaluation ===============

=============== Prediction Test of model with a single sample and test dataset ===============

Sentiment: This was a very bad steak | Prediction: Negative | Probability: 0.1027377
=============== End of Predictions ===============

=============== Prediction Test of loaded model with a multiple samples ===============

Sentiment: This was a horrible meal | Prediction: Negative | Probability: 0.1369192
Sentiment: I love this spaghetti. | Prediction: Positive | Probability: 0.9960636
=============== End of predictions ===============

=============== End of process ===============
Press any key to continue . . .

```

Parabéns! Agora você criou com sucesso um modelo de aprendizado de máquina para classificar e prever o sentimento das mensagens.

A criação de modelos bem-sucedidos é um processo iterativo. Esse modelo tem qualidade inicial inferior, pois o tutorial usa conjuntos de dados pequenos para fornecer um treinamento rápido do modelo. Se você não estiver satisfeito com a qualidade do modelo, poderá tentar melhorá-la fornecendo um conjunto de maiores conjuntos de grandes ou escolhendo algoritmos de treinamento diferentes com [hiperparâmetros](../resources/glossary.md#hyperparameter) diferentes para cada algoritmo.

Você pode encontrar o código-fonte para este tutorial no repositório [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/SentimentAnalysis).

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a:
> [!div class="checklist"]
>
> - Criar um aplicativo de console
> - Preparar dados
> - Carregar os dados
> - Criar e treinar o modelo
> - Avaliar o modelo
> - Usar o modelo para fazer uma previsão
> - Confira os resultados

Avançar para o próximo tutorial para saber mais
> [!div class="nextstepaction"]
> [Classificação de problema](github-issue-classification.md)
