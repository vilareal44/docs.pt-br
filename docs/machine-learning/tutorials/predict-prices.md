---
title: 'Tutorial: prever os preços usando a regressão'
description: Este tutorial mostra como criar um modelo de regressão usando do ML.NET para prever os preços, especificamente, as tarifas de táxi de Nova York.
ms.date: 06/30/2020
ms.topic: tutorial
ms.custom: mvc, title-hack-0516
ms.openlocfilehash: beb48c9252b83cd693c351d39882b7ac9d08d882
ms.sourcegitcommit: 0fa2b7b658bf137e813a7f4d09589d64c148ebf5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/14/2020
ms.locfileid: "86309710"
---
# <a name="tutorial-predict-prices-using-regression-with-mlnet"></a>Tutorial: prever preços usando regressão com ML.NET

Este tutorial mostra como criar um [modelo de regressão](../resources/glossary.md#regression) usando do ML.NET para prever os preços, especificamente, as tarifas de táxi de Nova York.

Neste tutorial, você aprenderá como:
> [!div class="checklist"]
>
> * Preparar e compreender os dados
> * Carregar e transformar os dados
> * Escolher um algoritmo de aprendizado
> * Treinar o modelo
> * Avaliar o modelo
> * Usar o modelo para previsões

## <a name="prerequisites"></a>Pré-requisitos

* [Visual studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) ou posterior ou visual Studio 2017 versão 15,6 ou posterior com a carga de trabalho "desenvolvimento de plataforma cruzada do .NET Core" instalada.

## <a name="create-a-console-application"></a>Criar um aplicativo de console

1. Crie um **Aplicativo de Console do .NET Core** chamado "TaxiFarePrediction".

1. Crie um diretório chamado *Dados* em seu projeto para armazenar o conjunto de dados e arquivos de modelo.

1. Instale o pacote NuGet **Microsoft.ml** e **Microsoft. ml. FastTree** :

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    No **Gerenciador de Soluções**, clique com o botão direito no projeto e escolha **Gerenciar Pacotes NuGet**. Escolha "nuget.org" como a origem do pacote, selecione a guia **Procurar**, pesquise por **Microsoft.ML**, selecione o pacote na lista e selecione o botão **Instalar**. Selecione o botão **OK** na caixa de diálogo **Visualizar Alterações** e selecione o botão **Aceito** na caixa de diálogo **Aceitação da Licença**, se concordar com o termos de licença para os pacotes listados. Faça o mesmo para o pacote NuGet **Microsoft. ml. FastTree** .

## <a name="prepare-and-understand-the-data"></a>Preparar e compreender os dados

1. Baixe os conjuntos de dados [taxi-fare-train.csv](https://github.com/dotnet/machinelearning/blob/master/test/data/taxi-fare-train.csv) e [taxi-fare-test.csv](https://github.com/dotnet/machinelearning/blob/master/test/data/taxi-fare-test.csv) e salve-os na pasta *Dados* criada na etapa anterior. Esses conjuntos de dados são usados para treinar o modelo de aprendizado de máquina e, em seguida, avaliar a precisão dele. Esses conjuntos de dados são originalmente do [Conjunto de dados NYC TLC Taxi Trip](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page).

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse em cada um dos \* arquivos. csv e selecione **Propriedades**. Em **avançado**, altere o valor de **copiar para diretório de saída** para **copiar se mais recente**.

1. Abra o conjunto de dados **taxi-fare-train.csv** e observe os cabeçalhos de coluna na primeira linha. Dê uma olhada em cada uma das colunas. Compreenda os dados e decida quais colunas são **recursos** e qual é o **rótulo**.

O `label` é a coluna que você deseja prever. O `Features`identificado são as entradas que você atribui ao modelo para prever o `Label`.

O conjunto de dados fornecido contém as seguintes colunas:

* **vendor_id:** o ID do taxista é um recurso.
* **rate_code:** o tipo de tarifa da viagem de táxi é um recurso.
* **passenger_count:** o número de passageiros na viagem é um recurso.
* **trip_time_in_secs:** a quantidade de tempo que a viagem levou. Você deseja prever a tarifa da viagem antes de sua conclusão. Nesse momento, você não sabe por quanto tempo levaria a duração. Portanto, o tempo da viagem não é um recurso e você excluirá essa coluna do modelo.
* **trip_distance:** a distância da viagem é um recurso.
* **payment_type:** o método de pagamento (dinheiro ou cartão de crédito) é um recurso.
* **fare_amount:** a tarifa total de táxi paga é o rótulo.

## <a name="create-data-classes"></a>Criar classes de dados

Crie classes para os dados de entrada e as previsões:

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **Novo Item**.
1. Na caixa de diálogo **Adicionar Novo Item**, selecione **Classe** e altere o campo **Nome** para *TaxiTrip.cs*. Em seguida, selecione o botão **Adicionar**.
1. Adicione as seguintes diretivas `using` ao novo arquivo:

   [!code-csharp[AddUsings](./snippets/predict-prices/csharp/TaxiTrip.cs#1 "Add necessary usings")]

Remova a definição de classe existente e adicione o seguinte código, que tem duas classes `TaxiTrip` e `TaxiTripFarePrediction`, ao arquivo *TaxiTrip.cs*:

[!code-csharp[DefineTaxiTrip](./snippets/predict-prices/csharp/TaxiTrip.cs#2 "Define the taxi trip and fare predictions classes")]

`TaxiTrip` é a classe de dados de entrada e tem definições para cada uma das colunas do conjunto de dados. Use o atributo <xref:Microsoft.ML.Data.LoadColumnAttribute> para especificar os índices das colunas de origem no conjunto de dados.

A classe `TaxiTripFarePrediction` representa os resultados previstos. Ele tem um único campo float, `FareAmount` , com um `Score` <xref:Microsoft.ML.Data.ColumnNameAttribute> atributo aplicado. No caso da tarefa de regressão, a coluna de **Pontuação** contém valores de rótulo previstos.

> [!NOTE]
> Use o tipo `float` para representar valores de ponto flutuante nas classes de dados de entrada e previsão.

### <a name="define-data-and-model-paths"></a>Definir dados e caminhos de modelo

Adicione as seguintes instruções `using` adicionais ao início do arquivo *Program.cs*: 

[!code-csharp[AddUsings](./snippets/predict-prices/csharp/Program.cs#1 "Add necessary usings")]

Você precisa criar três campos para conter os caminhos para os arquivos com conjuntos de dados e o arquivo para salvar o modelo:

* `_trainDataPath` contém o caminho para o arquivo com o conjunto de dados usado para treinar o modelo.
* `_testDataPath` contém o caminho para o arquivo com o conjunto de dados usado para avaliar o modelo.
* `_modelPath` contém o caminho para o arquivo em que o modelo treinado está armazenado.

Adicione o seguinte código logo acima do método `Main` para especificar estes caminhos e para a variável `_textLoader`:

[!code-csharp[InitializePaths](./snippets/predict-prices/csharp/Program.cs#2 "Define variables to store the data file paths")]

Todas as operações do ML.NET iniciam na [classe MLContext](xref:Microsoft.ML.MLContext). Inicializar `mlContext` cria um novo ambiente do ML.NET que pode ser compartilhado entre os objetos do fluxo de trabalho de criação de modelo. Ele é semelhante, conceitualmente, a `DBContext` no Entity Framework.

### <a name="initialize-variables-in-main"></a>Inicializar variáveis em Main

Substitua a linha `Console.WriteLine("Hello World!")` no método `Main` pelo seguinte código para declarar e inicializar a variável `mlContext`:

[!code-csharp[CreateMLContext](./snippets/predict-prices/csharp/Program.cs#3 "Create the ML Context")]

Adicione o seguinte como a linha seguinte de código no método `Main` para chamar o método `Train`:

[!code-csharp[Train](./snippets/predict-prices/csharp/Program.cs#5 "Train your model")]

O método `Train()` executa as seguintes tarefas:

* Carrega os dados.
* Extrai e transforma os dados.
* Treina o modelo.
* Retorna o modelo.

O método `Train` treina o modelo. Crie esse método logo abaixo de `Main`, usando o seguinte código:

```csharp
public static ITransformer Train(MLContext mlContext, string dataPath)
{

}
```

## <a name="load-and-transform-data"></a>Carregar e transformar dados

O ML.NET usa a [classe IDataView](xref:Microsoft.ML.IDataView) como uma maneira flexível e eficiente de descrever dados tabulares de texto ou numéricos. O `IDataView` pode carregar arquivos de texto ou em tempo real (por exemplo, banco de dados SQL ou arquivos de log). Adicione o seguinte código como a primeira linha do método `Train()`:

[!code-csharp[LoadTrainData](./snippets/predict-prices/csharp/Program.cs#6 "loading training dataset")]

Como você deseja prever a tarifa de percurso de táxi, a `FareAmount` coluna é a `Label` que você irá prever (a saída do modelo). Use a `CopyColumnsEstimator` classe de transformação para copiar `FareAmount` e adicione o seguinte código:

[!code-csharp[CopyColumnsEstimator](./snippets/predict-prices/csharp/Program.cs#7 "Use the CopyColumnsEstimator")]

O algoritmo que treina o modelo requer recursos **numéricos** , portanto, você precisa transformar os valores de dados categóricos ( `VendorId` , `RateCode` e `PaymentType` ) em números ( `VendorIdEncoded` , `RateCodeEncoded` e `PaymentTypeEncoded` ). Para fazer isso, use a classe de transformação [OneHotEncodingTransformer](xref:Microsoft.ML.Transforms.OneHotEncodingTransformer), que atribui valores de chave numérica diferentes para os diferentes valores em cada uma das colunas e adicione o seguinte código:

[!code-csharp[OneHotEncodingEstimator](./snippets/predict-prices/csharp/Program.cs#8 "Use the OneHotEncodingEstimator")]

A última etapa na preparação de dados combina todas as colunas de recursos na coluna **Features** usando a classe de transformação `mlContext.Transforms.Concatenate`. Por padrão, um algoritmo de aprendizado processa apenas os recursos da coluna **Features**. Adicione o seguinte código:

[!code-csharp[ColumnConcatenatingEstimator](./snippets/predict-prices/csharp/Program.cs#9 "Use the ColumnConcatenatingEstimator")]

## <a name="choose-a-learning-algorithm"></a>Escolher um algoritmo de aprendizado

Esse problema é sobre prever uma tarifa de corrida de táxi em Nova York. À primeira vista, pode parecer que depende simplesmente da distância percorrida. No entanto, os taxistas em Nova York cobram valores variados por outros fatores, como passageiros adicionais ou pagamento por cartão de crédito em vez de dinheiro. Você deseja prever o valor de preço, que é um valor real, com base em outros fatores no conjunto de dados. Para fazer isso, você deve escolher uma tarefa de aprendizado de máquina de [regressão](../resources/glossary.md#regression).

Acrescente a tarefa de aprendizado de máquina [FastTreeRegressionTrainer](xref:Microsoft.ML.Trainers.FastTree.FastTreeRegressionTrainer) às definições de transformação de dados adicionando o seguinte como a próxima linha de código em `Train()`:

[!code-csharp[FastTreeRegressionTrainer](./snippets/predict-prices/csharp/Program.cs#10 "Add the FastTreeRegressionTrainer")]

## <a name="train-the-model"></a>Treinar o modelo

Ajuste o modelo à `dataview` do treinamento e retorne o modelo treinado adicionando a seguinte linha de código ao método `Train()`:

[!code-csharp[TrainModel](./snippets/predict-prices/csharp/Program.cs#11 "Train the model")]

O método [Fit()](xref:Microsoft.ML.Trainers.FastTree.FastTreeRegressionTrainer.Fit%28Microsoft.ML.IDataView,Microsoft.ML.IDataView%29) treina o modelo transformando o conjunto de dados e aplicando o treinamento.

Retorne o modelo treinado com a seguinte linha de código no método `Train()`:

[!code-csharp[ReturnModel](./snippets/predict-prices/csharp/Program.cs#12 "Return the model")]

## <a name="evaluate-the-model"></a>Avaliar o modelo

Em seguida, avalie o desempenho do modelo com seus dados de teste para garantia de qualidade e validação. Crie o método `Evaluate()`, logo após `Train()`, com o código a seguir:

```csharp
private static void Evaluate(MLContext mlContext, ITransformer model)
{

}
```

O método `Evaluate` executa as seguintes tarefas:

* Carrega o conjunto de dados de teste.
* Cria o avaliador de regressão.
* Avalia o modelo e cria métricas.
* Exibe as métricas.

Adicione uma chamada ao novo método a partir do método `Main`, logo abaixo da chamada do método `Train`, usando o seguinte código:

[!code-csharp[CallEvaluate](./snippets/predict-prices/csharp/Program.cs#14 "Call the Evaluate method")]

Carregue o conjunto de dados de teste usando o método [LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%2A). Avalie o modelo usando esse conjunto de dados como uma verificação de qualidade adicionando o seguinte código ao método `Evaluate`:

[!code-csharp[LoadTestDataset](./snippets/predict-prices/csharp/Program.cs#15 "Load the test dataset")]

Em seguida, transforme os dados `Test` adicionando o seguinte código a `Evaluate()`:

[!code-csharp[PredictWithTransformer](./snippets/predict-prices/csharp/Program.cs#16 "Predict using the Transformer")]

O método [Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) faz previsões para testar as linhas de entrada do conjunto de dados.

O método `RegressionContext.Evaluate` calcula as métricas de qualidade para o `PredictionModel` usando o conjunto de dados especificado. Ele retorna um objeto <xref:Microsoft.ML.Data.RegressionMetrics> que contém as métricas gerais computadas pelos avaliadores de regressão.

Para exibi-los a fim de determinar a qualidade do modelo, é preciso primeiro obter as métricas. Adicione o seguinte código ao método `Evaluate` como a linha seguinte:

[!code-csharp[ComputeMetrics](./snippets/predict-prices/csharp/Program.cs#17 "Compute Metrics")]

Depois que você define a previsão, o método [Evaluate()](xref:Microsoft.ML.RegressionCatalog.Evaluate%2A) avalia o modelo, que compara os valores previstos com os `Labels` reais no conjunto de dados de teste e retorna métricas sobre o desempenho do modelo.

Adicione o seguinte código para avaliar o modelo e produzir as métricas de avaliação:

```csharp
Console.WriteLine();
Console.WriteLine($"*************************************************");
Console.WriteLine($"*       Model quality metrics evaluation         ");
Console.WriteLine($"*------------------------------------------------");
```

[RSquared](../resources/glossary.md#coefficient-of-determination) é outra métrica de avaliação dos modelos de regressão. RSquared aceita valores entre 0 e 1. Quanto mais perto de 1 estiver o valor, melhor o modelo será. Adicione o seguinte código ao método `Evaluate` para exibir o valor RSquared:

[!code-csharp[DisplayRSquared](./snippets/predict-prices/csharp/Program.cs#18 "Display the RSquared metric.")]

[RMS](../resources/glossary.md#root-of-mean-squared-error-rmse) é uma das métricas de avaliação do modelo de regressão. Quanto menor, melhor o modelo. Adicione o seguinte código ao método `Evaluate` para exibir o valor RMS:

[!code-csharp[DisplayRMS](./snippets/predict-prices/csharp/Program.cs#19 "Display the RMS metric.")]

## <a name="use-the-model-for-predictions"></a>Usar o modelo para previsões

Crie o método `TestSinglePrediction`, logo após o método `Evaluate`, usando o seguinte código:

```csharp
private static void TestSinglePrediction(MLContext mlContext, ITransformer model)
{

}
```

O método `TestSinglePrediction` executa as seguintes tarefas:

* Cria um único comentário dos dados de teste.
* Prevê o valor da tarifa com base nos dados de teste.
* Combina dados de teste e previsões para relatórios.
* Exibe os resultados previstos.

Adicione uma chamada ao novo método a partir do método `Main`, logo abaixo da chamada do método `Evaluate`, usando o seguinte código:

[!code-csharp[CallTestSinglePrediction](./snippets/predict-prices/csharp/Program.cs#20 "Call the TestSinglePrediction method")]

Use o `PredictionEngine` para prever a tarifa adicionando o seguinte código a `TestSinglePrediction()`:

[!code-csharp[MakePredictionEngine](./snippets/predict-prices/csharp/Program.cs#22 "Create the PredictionFunction")]

O [PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) é uma API de conveniência, que permite que você execute uma previsão em uma única instância de dados. [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602)Não é thread-safe. É aceitável usar em ambientes de protótipo ou de thread único. Para melhorar o desempenho e a segurança de thread em ambientes de produção, use o `PredictionEnginePool` serviço, que cria um [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601) dos [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) objetos para uso em todo o aplicativo. Consulte este guia sobre como [usar o `PredictionEnginePool` em uma API Web do ASP.NET Core](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application).

> [!NOTE]
> A extensão de serviço `PredictionEnginePool` está atualmente em versão prévia.

Este tutorial usa uma viagem de teste dentro dessa classe. Posteriormente, você pode adicionar outros cenários para fazer experiências com o modelo. Adicione uma corrida de teste à previsão do modelo treinado no método `TestSinglePrediction()` ao criar uma instância de `TaxiTrip`:

[!code-csharp[PredictionData](./snippets/predict-prices/csharp/Program.cs#23 "Create test data for single prediction")]

Em seguida, preveja a tarifa com base em uma única instância dos dados de corrida táxi e passe-a para o `PredictionEngine` adicionando o seguinte como as próximas linhas do código no método `TestSinglePrediction()`:

[!code-csharp[Predict](./snippets/predict-prices/csharp/Program.cs#24 "Create a prediction of taxi fare")]

A função [Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) faz uma previsão em uma única instância de dados.

Para exibir a tarifa prevista da corrida especificada, adicione o código a seguir ao método `TestSinglePrediction`:

[!code-csharp[Predict](./snippets/predict-prices/csharp/Program.cs#25 "Display the prediction.")]

Execute o programa para ver a tarifa de táxi prevista para o seu caso de teste.

Parabéns! Agora você criou com sucesso um modelo de aprendizado de máquina para prever tarifas de táxi, avaliou sua precisão e o usou para fazer previsões. Encontre o código-fonte deste tutorial no repositório do GitHub [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/TaxiFarePrediction).

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a:

> [!div class="checklist"]
>
> * Preparar e compreender os dados
> * Criar um pipeline de aprendizado
> * Carregar e transformar os dados
> * Escolher um algoritmo de aprendizado
> * Treinar o modelo
> * Avaliar o modelo
> * Usar o modelo para previsões

Avance para o próximo tutorial para saber mais.

> [!div class="nextstepaction"]
> [Clustering de Iris](iris-clustering.md)
