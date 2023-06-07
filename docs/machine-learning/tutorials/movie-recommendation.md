---
title: 'Tutorial: criar um recomendador de filmes-fatoração de matriz'
description: Este tutorial mostra como criar uma recomendação de filme do ML.NET em um aplicativo de console do .NET Core. AS etapas usam C# e o Visual Studio 2019.
author: briacht
ms.date: 06/30/2020
ms.custom: mvc, title-hack-0516
ms.topic: tutorial
ms.openlocfilehash: 39c4aeef0b02a6bf47d78e6bf53cd42b4f592946
ms.sourcegitcommit: 97ce5363efa88179dd76e09de0103a500ca9b659
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2020
ms.locfileid: "86282093"
---
# <a name="tutorial-build-a-movie-recommender-using-matrix-factorization-with-mlnet"></a>Tutorial: criar um recomendador de filmes usando a fatoração de matriz com o ML.NET

Este tutorial mostra como criar uma recomendação de filme do ML.NET em um aplicativo de console do .NET Core. AS etapas usam C# e o Visual Studio 2019.

Neste tutorial, você aprenderá como:
> [!div class="checklist"]
>
> * Selecionar um algoritmo de aprendizado de máquina
> * Preparar e carregar os dados
> * Criar e treinar um modelo
> * Avaliar um modelo
> * Implantar e consumir um modelo

Você pode encontrar o código-fonte para este tutorial no repositório [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/MovieRecommendation).

## <a name="machine-learning-workflow"></a>Fluxo de trabalho de aprendizado de máquina

Você usará as seguintes etapas para realizar sua tarefa, bem como qualquer outra tarefa do ML.NET:

1. [Carregar os dados](#load-your-data)
2. [Criar e treinar o modelo](#build-and-train-your-model)
3. [Avaliar o modelo](#evaluate-your-model)
4. [Usar o modelo](#use-your-model)

## <a name="prerequisites"></a>Pré-requisitos

* [Visual studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) ou posterior ou visual Studio 2017 versão 15,6 ou posterior com a carga de trabalho "desenvolvimento de plataforma cruzada do .NET Core" instalada.

## <a name="select-the-appropriate-machine-learning-task"></a>Selecionar a tarefa de aprendizado de máquina apropriada

Há várias maneiras de abordar problemas de recomendação, como recomendar uma lista de filmes ou uma lista de produtos relacionados, mas, nesse caso, você preverá qual classificação (1 a 5) um usuário dará a um filme específico e recomendará esse filme se ele for superior a um limite definido (quanto maior a classificação, maior a probabilidade de um usuário gostar de um filme específico).

## <a name="create-a-console-application"></a>Criar um aplicativo de console

### <a name="create-a-project"></a>Criar um projeto

1. Abra o Visual Studio 2017. Selecione **arquivo**  >  **novo**  >  **projeto** na barra de menus. Na caixa de diálogo **Novo Projeto**, selecione o nó **Visual C#** seguido pelo nó **.NET Core**. Em seguida, selecione o modelo de projeto **Aplicativo de console (.NET Core)**. Na caixa de texto **Nome**, digite "MovieRecommender" e, em seguida, selecione o botão **OK**.

2. Crie um diretório chamado *Data* no projeto para armazenar o conjunto de dados:

    No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **Nova Pasta**. Digite "Dados" e pressione Enter.

3. Instale os pacotes NuGet **Microsoft.ML** e **Microsoft.ML.Recommender**:

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    No **Gerenciador de Soluções**, clique com o botão direito no projeto e escolha **Gerenciar Pacotes NuGet**. Escolha "nuget.org" como a origem do pacote, selecione a guia **Procurar**, pesquise por **Microsoft.ML**, selecione o pacote na lista e selecione o botão **Instalar**. Selecione o botão **OK** na caixa de diálogo **Visualizar Alterações** e selecione o botão **Aceito** na caixa de diálogo **Aceitação da Licença**, se concordar com o termos de licença para os pacotes listados. Repita essas etapas para o **Microsoft.ML.Recommender**.

4. Adicione as seguintes instruções `using` à parte superior do arquivo *Program.cs*:

    [!code-csharp[UsingStatements](./snippets/movie-recommendation/csharp/Program.cs#UsingStatements "Add necessary usings")]

### <a name="download-your-data"></a>Baixar seus dados

1. Baixe os dois conjuntos de dados e salve-os na pasta *Data* criada anteriormente:

   * Clique com o botão direito do mouse em [*recommendation-ratings-train.csv*](https://raw.githubusercontent.com/dotnet/machinelearning-samples/master/samples/csharp/getting-started/MatrixFactorization_MovieRecommendation/Data/recommendation-ratings-train.csv) e selecione "Salvar Link (ou Destino) como..."
   * Clique com o botão direito do mouse em [*recommendation-ratings-test.csv*](https://raw.githubusercontent.com/dotnet/machinelearning-samples/master/samples/csharp/getting-started/MatrixFactorization_MovieRecommendation/Data/recommendation-ratings-test.csv) e selecione "Salvar Link (ou Destino) como..."

     Salve os arquivos \*.csv na pasta *Data* ou, depois de salvá-los em outro lugar, mova os arquivos \*.csv para a pasta *Data*.

2. No Gerenciador de Soluções, clique com o botão direito do mouse em cada um dos arquivos \*.csv e selecione **Propriedades**. Em **avançado**, altere o valor de **copiar para diretório de saída** para **copiar se mais recente**.

   ![GIF de um usuário selecionando copiar se mais recente no VS.](./media/movie-recommendation/copy-to-output-if-newer.gif)

## <a name="load-your-data"></a>Carregar os dados

A primeira etapa no processo do ML.NET é preparar e carregar os dados de treinamento e teste de modelo.

Os dados de classificações de recomendação são divididos nos conjuntos de dados `Train` e `Test`. Os dados `Train` são usados para ajustar o modelo. Os dados `Test` são usados para fazer previsões com o modelo treinado e avaliar o desempenho do modelo. É comum ter uma divisão 80/20 com os dados `Train` e `Test`.

Segue abaixo uma visualização dos dados nos arquivos \*.csv:

![Captura de tela da visualização do conjunto de uma do CVS.](./media/movie-recommendation/csv-file-dataset-preview.png)

Nos arquivos \*.csv, há quatro colunas:

* `userId`
* `movieId`
* `rating`
* `timestamp`

No aprendizado de máquina, as colunas que são usadas para fazer uma previsão são chamadas [Recursos](../resources/glossary.md#feature) e a coluna com a previsão retornada é chamada o [Rótulo](../resources/glossary.md#label).

Você deseja prever as classificações de filmes, portanto, a coluna de classificação é o `Label`. As outras três colunas, `userId`, `movieId` e `timestamp`, são todos `Features` usados para prever o `Label`.

| Recursos      | Rotular         |
| ------------- |:-------------:|
| `userId`        |    `rating`     |
| `movieId`      |               |
| `timestamp`     |               |

Cabe a você decidir quais `Features` são usados para prever o `Label`. Você também pode usar métodos como a [importância do recurso de permutação](../how-to-guides/explain-machine-learning-model-permutation-feature-importance-ml-net.md) para ajudar a selecionar o melhor `Features` .

Nesse caso, você deve eliminar a coluna `timestamp` como um `Feature` porque o carimbo de data/hora não afeta como um usuário classifica determinado filme e, portanto, não contribui para uma previsão mais precisa:

| Recursos      | Rotular         |
| ------------- |:-------------:|
| `userId`        |    `rating`     |
| `movieId`      |               |

Em seguida, é necessário definir a estrutura de dados para a classe de entrada.

Adicione uma nova classe ao seu projeto:

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto e, em seguida, selecione **Adicionar > Novo Item**.

2. Na **caixa de diálogo Adicionar Novo Item**, selecione **Classe** e altere o campo **Nome** para *MovieRatingData.cs*. Em seguida, selecione o botão **Adicionar**.

O arquivo *MovieRatingData.cs* será aberto no editor de códigos. Adicione a seguinte instrução `using` acima de *MovieRatingData.cs*:

```csharp
using Microsoft.ML.Data;
```

Crie uma classe chamada `MovieRating` removendo a definição de classe existente e adicionando o seguinte código a *MovieRatingData.cs*:

[!code-csharp[MovieRatingClass](./snippets/movie-recommendation/csharp/MovieRatingData.cs#MovieRatingClass "Add the Movie Rating class")]

`MovieRating` especifica uma classe de dados de entrada. O atributo [LoadColumn](xref:Microsoft.ML.Data.LoadColumnAttribute.%23ctor%28System.Int32%29) especifica quais colunas (por índice de coluna) no conjunto de dados devem ser carregadas. As colunas `userId` e `movieId` são os `Features` (as entradas que você fornecerá ao modelo para prever o `Label`), e a coluna de classificação é o `Label`, que você preverá (a saída do modelo).

Crie outra classe, `MovieRatingPrediction`, para representar os resultados previstos adicionando o seguinte código após a classe `MovieRating` em *MovieRatingData.cs*:

[!code-csharp[PredictionClass](./snippets/movie-recommendation/csharp/MovieRatingData.cs#PredictionClass "Add the Movie Prediction Class")]

Em *Program.cs*, substitua o `Console.WriteLine("Hello World!")` pelo seguinte código dentro de `Main()`:

[!code-csharp[MLContext](./snippets/movie-recommendation/csharp/Program.cs#MLContext "Add MLContext")]

A [classe MLContext](xref:Microsoft.ML.MLContext) é um ponto de partida para todas as operações do ML.NET e a inicialização do `mlContext` cria um ambiente do ML.NET que pode ser compartilhado entre os objetos de fluxo de trabalho da criação de modelo. Ele é semelhante, conceitualmente, a `DBContext` no Entity Framework.

Após `Main()`, crie um método chamado `LoadData()`:

```csharp
public static (IDataView training, IDataView test) LoadData(MLContext mlContext)
{

}
```

> [!NOTE]
> Esse método apresentará um erro até que você adicione uma instrução de retorno nas etapas a seguir.

Inicialize as variáveis do caminho de dados, carregue os dados dos arquivos \*.csv e retorne os dados `Train` e `Test` como objetos `IDataView` adicionando o seguinte como a próxima linha de código em `LoadData()`:

[!code-csharp[LoadData](./snippets/movie-recommendation/csharp/Program.cs#LoadData "Load data from data paths")]

Os dados do ML.NET são representados como uma [classe IDataView](xref:Microsoft.ML.IDataView). `IDataView` é uma maneira flexível e eficiente de descrever dados tabulares (numéricos e texto). Os dados podem ser carregados de um arquivo de texto ou em tempo real (por exemplo, banco de dados SQL ou arquivos de log) para um objeto `IDataView`.

O [LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29) define o esquema de dados e lê o arquivo. Ele usa as variáveis de caminho de dados e retorna uma `IDataView`. Nesse caso, você fornece o caminho para os arquivos `Test` e `Train` e indica o cabeçalho do arquivo de texto (para que ele possa usar os nomes de coluna corretamente) e o separador de dados de caractere de vírgula (o separador padrão é uma guia).

Adicione o seguinte código ao método `Main()` para chamar o método `LoadData()` e retornar os dados `Train` e `Test`:

[!code-csharp[LoadDataMain](./snippets/movie-recommendation/csharp/Program.cs#LoadDataMain "Add LoadData method to Main")]

## <a name="build-and-train-your-model"></a>Criar e treinar o modelo

Há três conceitos principais em ML.NET: [Data](../resources/glossary.md#data), [transformadores](../resources/glossary.md#transformer)e [estimadores](../resources/glossary.md#estimator).

Os algoritmos de treinamento de aprendizado de máquina exigem que os dados estejam em determinado formato. `Transformers` são usados para transformar dados de tabela em um formato compatível.

![Diagrama da Dataflow do transformador.](./media/movie-recommendation/data-transformer-transformed.png)

Crie `Transformers` no ML.NET criando `Estimators`. `Estimators` usa dados e retorna `Transformers`.

![Diagrama do Dataflow do estimador.](./media/movie-recommendation/data-estimator-transformer.png)

O algoritmo de treinamento de recomendação que você usará para treinar seu modelo é um exemplo de `Estimator`.

Crie um `Estimator` com as seguintes etapas:

Crie o método `BuildAndTrainModel()`, logo após o método `LoadData()`, usando o seguinte código:

```csharp
public static ITransformer BuildAndTrainModel(MLContext mlContext, IDataView trainingDataView)
{

}
```

> [!NOTE]
> Esse método apresentará um erro até que você adicione uma instrução de retorno nas etapas a seguir.

Defina as transformações de dados adicionando o seguinte código a `BuildAndTrainModel()`:

[!code-csharp[DataTransformations](./snippets/movie-recommendation/csharp/Program.cs#DataTransformations "Define data transformations")]

Como `userId` e `movieId` representam usuários e títulos de filmes, não valores reais, use o método [MapValueToKey()](xref:Microsoft.ML.ConversionsExtensionsCatalog.MapValueToKey%2A) para transformar cada `userId` e cada `movieId` em uma coluna `Feature` de tipo de chave numérica (um formato aceito pelos algoritmos de recomendação) e adicioná-los como novas colunas de conjunto de dados:

| userId | movieId | Rotular | userIdEncoded | movieIdEncoded |
| ------------- |:-------------:| -----:|-----:|-----:|
| 1 | 1 | 4 | userKey1 | movieKey1 |
| 1 | 3 | 4 | userKey1 | movieKey2 |
| 1 | 6 | 4 | userKey1 | movieKey3 |

Escolha o algoritmo de aprendizado de máquina e acrescente-o às definições de transformação de dados adicionando o seguinte como a próxima linha de código em `BuildAndTrainModel()`:

[!code-csharp[AddAlgorithm](./snippets/movie-recommendation/csharp/Program.cs#AddAlgorithm "Add the training algorithm with options")]

O [MatrixFactorizationTrainer](xref:Microsoft.ML.RecommendationCatalog.RecommendationTrainers.MatrixFactorization%28Microsoft.ML.Trainers.MatrixFactorizationTrainer.Options%29) é o algoritmo de treinamento de recomendação.  A [Fatoração de Matriz](https://en.wikipedia.org/wiki/Matrix_factorization_(recommender_systems)) é uma abordagem comum de recomendação quando você tem dados sobre como os usuários classificaram produtos no passado, que é o caso para os conjuntos de dados deste tutorial. Há outros algoritmos de recomendação para quando você tem diferentes dados disponíveis (confira a seção [Outros algoritmos de recomendação](#other-recommendation-algorithms) abaixo para saber mais).

Nesse caso, o algoritmo `Matrix Factorization` usa um método chamado "filtragem colaborativa", que supõe que, se o Usuário 1 tem a mesma opinião do Usuário 2 sobre determinada questão, é mais provável que o Usuário 1 sinta-se da mesma forma que o Usuário 2 sobre outra questão.

Por exemplo, se o Usuário 1 e o Usuário 2 classificam filmes de forma semelhante, é mais provável que o Usuário 2 goste de um filme que o Usuário 1 assistiu e forneceu uma classificação alta:

| | `Incredibles 2 (2018)` | `The Avengers (2012)` | `Guardians of the Galaxy (2014)` |
| -------------:|-------------:| -----:|-----:|
| Usuário 1 | Assistiu e gostou do filme | Assistiu e gostou do filme | Assistiu e gostou do filme |
| Usuário 2 | Assistiu e gostou do filme | Assistiu e gostou do filme | Não assistiu – RECOMENDAR o filme |

O treinador `Matrix Factorization` tem várias [Opções](xref:Microsoft.ML.Trainers.MatrixFactorizationTrainer.Options), sobre as quais você pode ler mais na seção [Hiperparâmetros de algoritmo](#algorithm-hyperparameters) abaixo.

Ajuste o modelo aos dados `Train` e retorne o modelo treinado adicionando o seguinte como a próxima linha de código no método `BuildAndTrainModel()`:

[!code-csharp[FitModel](./snippets/movie-recommendation/csharp/Program.cs#FitModel "Call the Fit method and return back the trained model")]

O método [Fit()](xref:Microsoft.ML.Trainers.MatrixFactorizationTrainer.Fit%28Microsoft.ML.IDataView,Microsoft.ML.IDataView%29) treina o modelo com o conjunto de dados de treinamento fornecido. Tecnicamente, ele executa as definições `Estimator` transformando os dados e aplicando o treinamento e retorna o modelo treinado, que é um `Transformer`.

Adicione o seguinte como a próxima linha de código no método `Main()` para chamar o método `BuildAndTrainModel()` e retornar o modelo treinado:

[!code-csharp[BuildTrainModelMain](./snippets/movie-recommendation/csharp/Program.cs#BuildTrainModelMain "Add BuildAndTrainModel method in Main")]

## <a name="evaluate-your-model"></a>Avaliar o modelo

Depois de treinar o modelo, use os dados de teste para avaliar o desempenho do modelo.

Crie o método `EvaluateModel()`, logo após o método `BuildAndTrainModel()`, usando o seguinte código:

```csharp
public static void EvaluateModel(MLContext mlContext, IDataView testDataView, ITransformer model)
{

}
```

Transforme os dados `Test` adicionando o seguinte código a `EvaluateModel()`:

[!code-csharp[Transform](./snippets/movie-recommendation/csharp/Program.cs#Transform "Transform the test data")]

O método [Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) faz previsões para várias linhas de entrada fornecidas de um conjunto de dados de teste.

Avalie o modelo adicionando o seguinte como a próxima linha de código no método `EvaluateModel()`:

[!code-csharp[Evaluate](./snippets/movie-recommendation/csharp/Program.cs#Evaluate "Evaluate the model using predictions from the test data")]

Depois que você define a previsão, o método [Evaluate()](xref:Microsoft.ML.RecommendationCatalog.Evaluate%2A) avalia o modelo, que compara os valores previstos com os `Labels` reais no conjunto de dados de teste e retorna métricas sobre o desempenho do modelo.

Imprima as métricas de avaliação no console adicionando o seguinte como a próxima linha de código no método `EvaluateModel()`:

[!code-csharp[PrintMetrics](./snippets/movie-recommendation/csharp/Program.cs#PrintMetrics "Print the evaluation metrics")]

Adicione o seguinte como a próxima linha de código no método `Main()` para chamar o método `EvaluateModel()`:

[!code-csharp[EvaluateModelMain](./snippets/movie-recommendation/csharp/Program.cs#EvaluateModelMain "Add EvaluateModel method in Main")]

Até agora, a saída deve ser semelhante ao seguinte texto:

```console
=============== Training the model ===============
iter      tr_rmse          obj
   0       1.5403   3.1262e+05
   1       0.9221   1.6030e+05
   2       0.8687   1.5046e+05
   3       0.8416   1.4584e+05
   4       0.8142   1.4209e+05
   5       0.7849   1.3907e+05
   6       0.7544   1.3594e+05
   7       0.7266   1.3361e+05
   8       0.6987   1.3110e+05
   9       0.6751   1.2948e+05
  10       0.6530   1.2766e+05
  11       0.6350   1.2644e+05
  12       0.6197   1.2541e+05
  13       0.6067   1.2470e+05
  14       0.5953   1.2382e+05
  15       0.5871   1.2342e+05
  16       0.5781   1.2279e+05
  17       0.5713   1.2240e+05
  18       0.5660   1.2230e+05
  19       0.5592   1.2179e+05
=============== Evaluating the model ===============
Rms: 0.994051469730769
RSquared: 0.412556298844873
```

Nessa saída, há 20 iterações. Em cada iteração, a medida de erro diminui e é convergida para mais próximo de 0.

A `root of mean squared error` (RMS ou RMSE) é usada para medir as diferenças entre os valores previstos do modelo e os valores observados do conjunto de dados de teste. Tecnicamente, ela é a raiz quadrada da média dos quadrados dos erros. Quanto menor, melhor o modelo.

`R Squared` indica o quanto os dados se ajustam a um modelo. Varia de 0 a 1. Um valor de 0 significa que os dados são aleatórios ou não podem ser ajustados ao modelo. Um valor de 1 significa que o modelo corresponde exatamente aos dados. Você deseja que a pontuação `R Squared` esteja o mais próximo possível de 1.

A criação de modelos bem-sucedidos é um processo iterativo. Esse modelo tem qualidade inicial inferior, pois o tutorial usa conjuntos de dados pequenos para fornecer um treinamento rápido do modelo. Se você não estiver satisfeito com a qualidade do modelo, tente melhorá-lo fornecendo conjuntos de dados de treinamento maiores ou escolhendo diferentes algoritmos de treinamento com diferentes hiperparâmetros para cada algoritmo. Para obter mais informações, confira a seção [Melhorar o modelo](#improve-your-model) abaixo.

## <a name="use-your-model"></a>Usar o modelo

Agora você pode usar o modelo treinado para fazer previsões sobre novos dados.

Crie o método `UseModelForSinglePrediction()`, logo após o método `EvaluateModel()`, usando o seguinte código:

```csharp
public static void UseModelForSinglePrediction(MLContext mlContext, ITransformer model)
{

}
```

Use o `PredictionEngine` para prever a classificação adicionando o seguinte código à `UseModelForSinglePrediction()`:

[!code-csharp[PredictionEngine](./snippets/movie-recommendation/csharp/Program.cs#PredictionEngine "Create Prediction Engine")]

O [PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) é uma API de conveniência, que permite que você execute uma previsão em uma única instância de dados. [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602)Não é thread-safe. É aceitável usar em ambientes de protótipo ou de thread único. Para melhorar o desempenho e a segurança de thread em ambientes de produção, use o `PredictionEnginePool` serviço, que cria um [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601) dos [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) objetos para uso em todo o aplicativo. Consulte este guia sobre como [usar o `PredictionEnginePool` em uma API Web do ASP.NET Core](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application).

> [!NOTE]
> A extensão de serviço `PredictionEnginePool` está atualmente em versão prévia.

Crie uma instância de `MovieRating` chamada `testInput` e passe-a para o Mecanismo de Previsão adicionando o seguinte como as próximas linhas do código no método `UseModelForSinglePrediction()`:

[!code-csharp[MakeSinglePrediction](./snippets/movie-recommendation/csharp/Program.cs#MakeSinglePrediction "Make a single prediction with the Prediction Engine")]

A função [Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) faz uma previsão sobre uma única coluna de dados.

Em seguida, use a `Score`, ou a classificação prevista, para determinar se deseja recomendar o filme com a movieId 10 para o usuário 6. Quanto mais alta a `Score`, maior a probabilidade de um usuário gostar de um filme específico. Nesse caso, digamos que você recomende filmes com uma classificação prevista de > 3,5.

Para imprimir os resultados, adicione o seguinte como as próximas linhas de código no método `UseModelForSinglePrediction()`:

[!code-csharp[PrintResults](./snippets/movie-recommendation/csharp/Program.cs#PrintResults "Print the recommendation prediction results")]

Adicione o seguinte como a próxima linha de código no método `Main()` para chamar o método `UseModelForSinglePrediction()`:

[!code-csharp[UseModelMain](./snippets/movie-recommendation/csharp/Program.cs#UseModelMain "Add UseModelForSinglePrediction method in Main")]

A saída desse método deve ser semelhante ao seguinte texto:

```console
=============== Making a prediction ===============
Movie 10 is recommended for user 6
```

### <a name="save-your-model"></a>Salve seu modelo

Para usar o modelo para fazer previsões em aplicativos de usuário final, primeiro é necessário salvar o modelo.

Crie o método `SaveModel()`, logo após o método `UseModelForSinglePrediction()`, usando o seguinte código:

```csharp
public static void SaveModel(MLContext mlContext, DataViewSchema trainingDataViewSchema, ITransformer model)
{

}
```

Salve o modelo treinado adicionando o seguinte código ao método `SaveModel()`:

[!code-csharp[SaveModel](./snippets/movie-recommendation/csharp/Program.cs#SaveModel "Save the model to a zip file")]

Esse método salva o modelo treinado em um arquivo .zip (na pasta "Data"), que pode ser usado em outros aplicativos .NET para fazer previsões.

Adicione o seguinte como a próxima linha de código no método `Main()` para chamar o método `SaveModel()`:

[!code-csharp[SaveModelMain](./snippets/movie-recommendation/csharp/Program.cs#SaveModelMain "Create SaveModel method in Main")]

### <a name="use-your-saved-model"></a>Usar o modelo salvo

Depois de salvar seu modelo treinado, você pode consumir o modelo em ambientes diferentes. Consulte [salvar e carregar modelos treinados](../how-to-guides/save-load-machine-learning-models-ml-net.md) para saber como colocar em operação um modelo de aprendizado de máquina treinado em aplicativos.

## <a name="results"></a>Resultados

Depois de seguir as etapas acima, execute o aplicativo de console (Ctrl + F5). Os resultados da previsão individual acima deverão ser semelhantes ao mostrado a seguir. Você poderá ver avisos ou mensagens de processamento, mas essas mensagens foram removidas dos resultados a seguir para maior clareza.

```console
=============== Training the model ===============
iter      tr_rmse          obj
   0       1.5382   3.1213e+05
   1       0.9223   1.6051e+05
   2       0.8691   1.5050e+05
   3       0.8413   1.4576e+05
   4       0.8145   1.4208e+05
   5       0.7848   1.3895e+05
   6       0.7552   1.3613e+05
   7       0.7259   1.3357e+05
   8       0.6987   1.3121e+05
   9       0.6747   1.2949e+05
  10       0.6533   1.2766e+05
  11       0.6353   1.2636e+05
  12       0.6209   1.2561e+05
  13       0.6072   1.2462e+05
  14       0.5965   1.2394e+05
  15       0.5868   1.2352e+05
  16       0.5782   1.2279e+05
  17       0.5713   1.2227e+05
  18       0.5637   1.2190e+05
  19       0.5604   1.2178e+05
=============== Evaluating the model ===============
Rms: 0.977175077487166
RSquared: 0.43233349213192
=============== Making a prediction ===============
Movie 10 is recommended for user 6
=============== Saving the model to a file ===============
```

Parabéns! Você agora criou um modelo de machine learning para recomendação de filmes. Você pode encontrar o código-fonte para este tutorial no repositório [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/MovieRecommendation).

## <a name="improve-your-model"></a>Melhorar o modelo

Há várias maneiras de melhorar o desempenho do modelo para obter previsões mais precisas.

### <a name="data"></a>Dados

A adição de mais dados de treinamento que tenham amostras suficientes para cada usuário e a ID de filme pode ajudar a melhorar a qualidade do modelo de recomendação.

A [validação cruzada](../how-to-guides/train-machine-learning-model-cross-validation-ml-net.md) é uma técnica para avaliação de modelos que divide dados aleatoriamente em subconjuntos (em vez de extrair os dados de teste do conjunto de dados como você fez neste tutorial) e usa alguns dos grupos como dados de treinamento e alguns dos grupos como dados de teste. Esse método tem um melhor desempenho do que a divisão treinamento/teste em termos de qualidade do modelo.

### <a name="features"></a>Recursos

Neste tutorial, você só usará os três `Features` (`user id`, `movie id` e `rating`) fornecidos pelo conjunto de dados.

Embora esse seja um bom começo, na verdade, o ideal será adicionar outros atributos ou `Features` (por exemplo, idade, sexo, localização geográfica etc.) se eles forem incluídos no conjunto de dados. A adição de `Features` mais relevantes pode ajudar a melhorar o desempenho do modelo de recomendação.

Se você não tiver certeza sobre o que `Features` pode ser o mais relevante para sua tarefa de aprendizado de máquina, também poderá usar o cálculo de contribuição de recurso (FCC) e a [importância do recurso de permuta](../how-to-guides/explain-machine-learning-model-permutation-feature-importance-ml-net.md), que o ml.NET fornece para descobrir os mais influentes `Features` .

### <a name="algorithm-hyperparameters"></a>Hiperparâmetros de algoritmo

Embora o ML.NET forneça bons algoritmos de treinamento padrão, você pode ajustar ainda mais o desempenho alterando os [hiperparâmetros](../resources/glossary.md#hyperparameter) do algoritmo.

Para a `Matrix Factorization`, você pode experimentar com hiperparâmetros, como [NumberOfIterations](xref:Microsoft.ML.Trainers.MatrixFactorizationTrainer.Options.NumberOfIterations) e [ApproximationRank](xref:Microsoft.ML.Trainers.MatrixFactorizationTrainer.Options.ApproximationRank) para ver se ele oferece melhores resultados.

Por exemplo, neste tutorial, as opções de algoritmo são:

```csharp
var options = new MatrixFactorizationTrainer.Options
{
    MatrixColumnIndexColumnName = "userIdEncoded",
    MatrixRowIndexColumnName = "movieIdEncoded",
    LabelColumnName = "Label",
    NumberOfIterations = 20,
    ApproximationRank = 100
};
```

### <a name="other-recommendation-algorithms"></a>Outros algoritmos de recomendação

O algoritmo de fatoração de matriz com a filtragem colaborativa é apenas uma abordagem para fazer recomendações de filmes. Em muitos casos, talvez você não tenha os dados de classificações disponíveis e tenha apenas o histórico de filmes disponível dos usuários. Em outros casos, você pode ter mais do que apenas os dados de classificação do usuário.

| Algoritmo       | Cenário           | Amostra  |
| ------------- |:-------------:| -----:|
| Fatoração de Matriz de uma Classe | Use isso quando você tiver apenas userId e movieId. Esse estilo de recomendação é baseado no cenário de cocompra, ou produtos frequentemente comprados juntos, o que significa que ele recomendará aos clientes um conjunto de produtos de acordo com seu próprio histórico de ordens de compra. | [>experimentar](https://github.com/dotnet/machinelearning-samples/tree/master/samples/csharp/getting-started/MatrixFactorization_ProductRecommendation) |
| Computadores de Fatoração com Reconhecimento de Campo | Use isso para fazer recomendações quando você tiver mais Recursos do que userId, productId e a classificação (como descrição ou preço do produto). Esse método também usa uma abordagem de filtragem colaborativa. | [>experimentar](https://github.com/dotnet/machinelearning-samples/tree/master/samples/csharp/end-to-end-apps/Recommendation-MovieRecommender) |

### <a name="new-user-scenario"></a>Novo cenário de usuário

Um problema comum na filtragem colaborativa é o problema de inicialização a frio, que é quando você tem um novo usuário sem nenhum dado anterior do qual fazer inferências. Esse problema costuma ser resolvido com a solicitação da criação de um perfil aos novos usuários e, por exemplo, da classificação de filmes que assistiram no passado. Embora esse método seja um fardo para o usuário, ele fornece alguns dados iniciais para novos usuários sem nenhum histórico de classificação.

## <a name="resources"></a>Recursos

Os dados usados neste tutorial foram obtidos do [Conjunto de dados MovieLens](http://files.grouplens.org/datasets/movielens/).

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a:

> [!div class="checklist"]
>
> * Selecionar um algoritmo de aprendizado de máquina
> * Preparar e carregar os dados
> * Criar e treinar um modelo
> * Avaliar um modelo
> * Implantar e consumir um modelo

Avançar para o próximo tutorial para saber mais
> [!div class="nextstepaction"]
> [Análise de Sentimento](sentiment-analysis.md)
