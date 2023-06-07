---
title: 'Tutorial: analisar as opiniões de revisão usando um modelo TensorFlow'
description: Este tutorial mostra como usar um modelo de TensorFlow pré-treinado para classificar as opiniões nos comentários do site. O classificador de sentimentos binário é um aplicativo de console em C# desenvolvido usando o Visual Studio.
ms.date: 06/30/2020
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 9a2e7f72d59e31cfd7db5b89bfad55bccb063cea
ms.sourcegitcommit: 97ce5363efa88179dd76e09de0103a500ca9b659
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2020
ms.locfileid: "86281401"
---
# <a name="tutorial-analyze-sentiment-of-movie-reviews-using-a-pre-trained-tensorflow-model-in-mlnet"></a>Tutorial: analisar o sentimentos das análises de filmes usando um modelo de TensorFlow pré-treinado no ML.NET

Este tutorial mostra como usar um modelo de TensorFlow pré-treinado para classificar as opiniões nos comentários do site. O classificador de sentimentos binário é um aplicativo de console em C# desenvolvido usando o Visual Studio.

O modelo TensorFlow usado neste tutorial foi treinado usando revisões de filme do banco de dados do IMDB. Depois de concluir o desenvolvimento do aplicativo, você poderá fornecer o texto de revisão do filme e o aplicativo informará se a revisão tem uma opinião positiva ou negativa.

Neste tutorial, você aprenderá como:
> [!div class="checklist"]
>
> * Carregar um modelo de TensorFlow pré-treinado
> * Transformar o texto de comentário do site em recursos adequados para o modelo
> * Usar o modelo para fazer uma previsão

Você pode encontrar o código-fonte para este tutorial no repositório [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/TextClassificationTF).

## <a name="prerequisites"></a>Pré-requisitos

* [Visual Studio 2017 versão 15,6 ou posterior](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) com a carga de trabalho "desenvolvimento de plataforma cruzada do .NET Core" instalada.

## <a name="setup"></a>Configuração

### <a name="create-the-application"></a>Criar o aplicativo

1. Crie um **aplicativo de console do .NET Core** chamado "TextClassificationTF".

2. Crie um diretório chamado *dados* em seu projeto para salvar os arquivos do conjunto de dados.

3. Instalar o **Pacote NuGet Microsoft.ML**:

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    No Gerenciador de Soluções, clique com o botão direito do mouse no seu projeto e selecione **Gerenciar Pacotes NuGet**. Escolha "nuget.org" como a origem do pacote e, em seguida, selecione a guia **procurar** . procure **Microsoft.ml**, selecione o pacote desejado e, em seguida, selecione o botão **instalar** . Prossiga com a instalação concordando com os termos de licença do pacote que você escolher. Repita essas etapas para **Microsoft. ml. TensorFlow**, **Microsoft. ml. SampleUtils** e **SciSharp. TensorFlow. Redist**.

### <a name="add-the-tensorflow-model-to-the-project"></a>Adicionar o modelo TensorFlow ao projeto

> [!NOTE]
> O modelo para este tutorial é do repositório GitHub [dotnet/MachineLearning-TestData](https://github.com/dotnet/machinelearning-testdata/tree/master/Microsoft.ML.TensorFlow.TestModels/sentiment_model) . O modelo está no formato TensorFlow SavedModel.

1. Baixe o [arquivo zip sentiment_model](https://github.com/dotnet/samples/blob/master/machine-learning/models/textclassificationtf/sentiment_model.zip?raw=true)e descompacte.

    O arquivo zip contém:

    * `saved_model.pb`: o próprio modelo de TensorFlow. O modelo usa uma matriz de inteiros de tamanho fixo (tamanho 600) de recursos que representam o texto em uma cadeia de caracteres de revisão do IMDB e gera duas probabilidades que somam 1: a probabilidade de que a revisão de entrada tenha uma opinião positiva e a probabilidade de que a revisão de entrada tenha uma opinião negativa.
    * `imdb_word_index.csv`: um mapeamento de palavras individuais para um valor inteiro. O mapeamento é usado para gerar os recursos de entrada para o modelo TensorFlow.

2. Copie o conteúdo do diretório mais interno `sentiment_model` em seu diretório de projeto do *TextClassificationTF* `sentiment_model` . Este diretório contém o modelo e os arquivos de suporte adicionais necessários para este tutorial, conforme mostrado na imagem a seguir:

   ![conteúdo do sentiment_model Directory](./media/text-classification-tf/sentiment-model-files.png)

3. Em Gerenciador de Soluções, clique com o botão direito do mouse em cada um dos arquivos no `sentiment_model` diretório e no subdiretório e selecione **Propriedades**. Em **avançado**, altere o valor de **copiar para diretório de saída** para **copiar se mais recente**.

### <a name="add-using-statements-and-global-variables"></a>Adicionar instruções using e variáveis globais

1. Adicione as seguintes instruções `using` adicionais ao início do arquivo *Program.cs*: 

   [!code-csharp[AddUsings](./snippets/text-classification-tf/csharp/Program.cs#AddUsings "Add necessary usings")]

1. Crie duas variáveis globais logo acima do `Main` método para manter o caminho do arquivo de modelo salvo e o comprimento do vetor de recurso.

   [!code-csharp[DeclareGlobalVariables](./snippets/text-classification-tf/csharp/Program.cs#DeclareGlobalVariables "Declare global variables")]

    * `_modelPath`é o caminho do arquivo do modelo treinado.
    * `FeatureLength`é o comprimento da matriz de recursos de inteiro que o modelo espera.

### <a name="model-the-data"></a>Modelar os dados

As revisões de filme são texto de forma livre. Seu aplicativo converte o texto no formato de entrada esperado pelo modelo em uma série de estágios discretos.

A primeira é dividir o texto em palavras separadas e usar o arquivo de mapeamento fornecido para mapear cada palavra em uma codificação de número inteiro. O resultado dessa transformação é uma matriz de inteiro de comprimento variável com um comprimento correspondente ao número de palavras na sentença.

|Propriedade| Valor|Tipo|
|-------------|-----------------------|------|
|ReviewText|Esse filme é realmente bom|string|
|VariableLengthFeatures|14, 22, 9, 66, 78,... |int []|

Em seguida, a matriz de recursos de comprimento variável é redimensionada para um comprimento fixo de 600. Esse é o comprimento esperado pelo modelo TensorFlow.

|Propriedade| Valor|Tipo|
|-------------|-----------------------|------|
|ReviewText|Esse filme é realmente bom|string|
|VariableLengthFeatures|14, 22, 9, 66, 78,... |int []|
|Recursos|14, 22, 9, 66, 78,... |int [600]|

1. Crie uma classe para os dados de entrada, após o `Main` método:

    [!code-csharp[MovieReviewClass](./snippets/text-classification-tf/csharp/Program.cs#MovieReviewClass "Declare movie review type")]

    A classe de dados de entrada, `MovieReview` , tem um `string` para comentários do usuário ( `ReviewText` ).

1. Crie uma classe para os recursos de comprimento variável, após o `Main` método:

    [!code-csharp[VariableLengthFeatures](./snippets/text-classification-tf/csharp/Program.cs#VariableLengthFeatures "Declare variable length features type")]

    A `VariableLengthFeatures` propriedade tem um atributo [vectortype](xref:Microsoft.ML.Data.VectorTypeAttribute.%23ctor%2A) para designá-lo como um vetor.  Todos os elementos de vetor devem ser do mesmo tipo. Em conjuntos de dados com um grande número de colunas, carregar várias colunas como um único vetor reduz o número de passagens de dados quando você aplica transformações de dados.

    Essa classe é usada na `ResizeFeatures` ação. Os nomes de suas propriedades (neste caso, apenas um) são usados para indicar quais colunas no DataView podem ser usadas como _entrada_ para a ação de mapeamento personalizado.

1. Crie uma classe para os recursos de comprimento fixo, após o `Main` método:

    [!code-csharp[FixedLengthFeatures](./snippets/text-classification-tf/csharp/Program.cs#FixedLengthFeatures)]

    Essa classe é usada na `ResizeFeatures` ação. Os nomes de suas propriedades (neste caso, apenas um) são usados para indicar quais colunas no DataView podem ser usadas como a _saída_ da ação de mapeamento personalizado.

    Observe que o nome da propriedade `Features` é determinado pelo modelo TensorFlow. Você não pode alterar esse nome de propriedade.

1. Crie uma classe para a previsão após o `Main` método:

    [!code-csharp[Prediction](./snippets/text-classification-tf/csharp/Program.cs#Prediction "Declare prediction class")]

    `MovieReviewSentimentPrediction` é a classe de previsão usada após o treinamento do modelo. `MovieReviewSentimentPrediction`tem uma única `float` matriz ( `Prediction` ) e um `VectorType` atributo.

### <a name="create-the-mlcontext-lookup-dictionary-and-action-to-resize-features"></a>Criar o MLContext, o dicionário de pesquisa e a ação para redimensionar recursos

A [classe MLContext](xref:Microsoft.ML.MLContext) é um ponto de partida para todas as operações do ML.NET. Inicializar `mlContext` cria um novo ambiente do ML.NET que pode ser compartilhado entre os objetos do fluxo de trabalho de criação de modelo. Ele é semelhante, conceitualmente, a `DBContext` no Entity Framework.

1. Substitua a linha `Console.WriteLine("Hello World!")` no método `Main` pelo seguinte código para declarar e inicializar a variável mlContext:

   [!code-csharp[CreateMLContext](./snippets/text-classification-tf/csharp/Program.cs#CreateMLContext "Create the ML Context")]

1. Crie um dicionário para codificar palavras como inteiros usando o [`LoadFromTextFile`](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%2A) método para carregar dados de mapeamento de um arquivo, como mostrado na tabela a seguir:

    |de palavras     |Índice    |
    |---------|---------|
    |Kids     |  362    |
    |desejar     |  181    |
    |errado    |  355    |
    |effects  |  302    |
    |sensação  |  547    |

    Adicione o código abaixo para criar o mapa de pesquisa:

    [!code-csharp[CreateLookupMap](./snippets/text-classification-tf/csharp/Program.cs#CreateLookupMap)]

1. Adicione um `Action` para redimensionar a matriz de comprimento variável do Word para uma matriz de número inteiro de tamanho fixo, com as próximas linhas de código:

   [!code-csharp[ResizeFeatures](./snippets/text-classification-tf/csharp/Program.cs#ResizeFeatures)]

## <a name="load-the-pre-trained-tensorflow-model"></a>Carregar o modelo de TensorFlow pré-treinado

1. Adicione o código para carregar o modelo TensorFlow:

    [!code-csharp[LoadTensorFlowModel](./snippets/text-classification-tf/csharp/Program.cs#LoadTensorFlowModel)]

    Depois que o modelo for carregado, você poderá extrair seu esquema de entrada e saída. Os esquemas são exibidos apenas para fins de interesse e aprendizado. Você não precisa desse código para que o aplicativo final funcione:

    [!code-csharp[GetModelSchema](./snippets/text-classification-tf/csharp/Program.cs#GetModelSchema)]

    O esquema de entrada é a matriz de comprimento fixo de palavras codificadas por inteiro. O esquema de saída é uma matriz float de probabilidades que indica se a opinião de uma revisão é negativa ou positiva. Esses valores somam 1, uma vez que a probabilidade de ser positiva é o complemento da probabilidade de que o sentimentos seja negativo.

## <a name="create-the-mlnet-pipeline"></a>Criar o pipeline ML.NET

1. Crie o pipeline e divida o texto de entrada em palavras usando [TokenizeIntoWords](xref:Microsoft.ML.TextCatalog.TokenizeIntoWords%2A) Transform para quebrar o texto em palavras como a próxima linha de código:

   [!code-csharp[TokenizeIntoWords](./snippets/text-classification-tf/csharp/Program.cs#TokenizeIntoWords)]

   A transformação [TokenizeIntoWords](xref:Microsoft.ML.TextCatalog.TokenizeIntoWords%2A) usa espaços para analisar o texto/cadeia de caracteres em palavras. Ele cria uma nova coluna e divide cada cadeia de caracteres de entrada em um vetor de subcadeias de caracteres com base no separador definido pelo usuário.

1. Mapeie as palavras em sua codificação de número inteiro usando a tabela de pesquisa declarada acima:

    [!code-csharp[MapValue](./snippets/text-classification-tf/csharp/Program.cs#MapValue)]

1. Redimensione as codificações de inteiro de comprimento variável para o comprimento fixo exigido pelo modelo:

    [!code-csharp[CustomMapping](./snippets/text-classification-tf/csharp/Program.cs#CustomMapping)]

1. Classifique a entrada com o modelo TensorFlow carregado:

    [!code-csharp[ScoreTensorFlowModel](./snippets/text-classification-tf/csharp/Program.cs#ScoreTensorFlowModel)]

    A saída do modelo TensorFlow é chamada `Prediction/Softmax` . Observe que o nome `Prediction/Softmax` é determinado pelo modelo TensorFlow. Você não pode alterar esse nome.

1. Crie uma nova coluna para a previsão de saída:

    [!code-csharp[SnippetCopyColumns](./snippets/text-classification-tf/csharp/Program.cs#SnippetCopyColumns)]

    Você precisa copiar a `Prediction/Softmax` coluna em uma com um nome que possa ser usado como uma propriedade em uma classe C#: `Prediction` . O `/` caractere não é permitido em um nome de propriedade C#.

## <a name="create-the-mlnet-model-from-the-pipeline"></a>Criar o modelo ML.NET a partir do pipeline

1. Adicione o código para criar o modelo a partir do pipeline:

    [!code-csharp[SnippetCreateModel](./snippets/text-classification-tf/csharp/Program.cs#SnippetCreateModel)]

    Um modelo ML.NET é criado a partir da cadeia de estimadores no pipeline chamando o `Fit` método. Nesse caso, não estamos ajustando nenhum dado para criar o modelo, pois o modelo TensorFlow já foi treinado anteriormente. Fornecemos um objeto de exibição de dados vazio para atender aos requisitos do `Fit` método.

## <a name="use-the-model-to-make-a-prediction"></a>Usar o modelo para fazer uma previsão

1. Adicione o `PredictSentiment` método abaixo do `Main` método:

    ```csharp
    public static void PredictSentiment(MLContext mlContext, ITransformer model)
    {

    }
    ```

1. Adicione o seguinte código para criar o `PredictionEngine` como a primeira linha no `PredictSentiment()` método:

    [!code-csharp[CreatePredictionEngine](./snippets/text-classification-tf/csharp/Program.cs#CreatePredictionEngine)]

    O [PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) é uma API de conveniência, que permite que você execute uma previsão em uma única instância de dados. [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602)Não é thread-safe. É aceitável usar em ambientes de protótipo ou de thread único. Para melhorar o desempenho e a segurança de thread em ambientes de produção, use o `PredictionEnginePool` serviço, que cria um [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601) dos [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) objetos para uso em todo o aplicativo. Consulte este guia sobre como [usar o `PredictionEnginePool` em uma API Web do ASP.NET Core](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application).

    > [!NOTE]
    > A extensão de serviço `PredictionEnginePool` está atualmente em versão prévia.

1. Adicione um comentário para testar as previsões do modelo treinado no método `Predict()` ao criar uma instância de `MovieReview`:

    [!code-csharp[CreateTestData](./snippets/text-classification-tf/csharp/Program.cs#CreateTestData)]

1. Passe os dados de comentário de teste para o `Prediction Engine` adicionando as próximas linhas de código no `PredictSentiment()` método:

    [!code-csharp[Predict](./snippets/text-classification-tf/csharp/Program.cs#Predict)]

1. A função [Predict ()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) faz uma previsão em uma única linha de dados:

    |Propriedade| Valor|Tipo|
    |-------------|-----------------------|------|
    |Previsão|[0,5459937, 0,454006255]|float []|

1. Exiba a previsão de sentimentos usando o seguinte código:

    [!code-csharp[DisplayPredictions](./snippets/text-classification-tf/csharp/Program.cs#DisplayPredictions)]

1. Adicione uma chamada ao `PredictSentiment` no final do `Main` método:

    [!code-csharp[CallPredictSentiment](./snippets/text-classification-tf/csharp/Program.cs#CallPredictSentiment)]

## <a name="results"></a>Resultados

Compile e execute seu aplicativo.

Seus resultados devem ser semelhantes aos seguintes. Durante o processamento, as mensagens são exibidas. Você pode ver avisos ou mensagens de processamento. Essas mensagens foram removidas dos resultados a seguir para ficar mais claro.

```console
Number of classes: 2
Is sentiment/review positive ? Yes
```

Parabéns! Agora você criou com êxito um modelo de aprendizado de máquina para classificar e prever os sentimentos de mensagens reutilizando um modelo pretreinado `TensorFlow` no ml.net.

Você pode encontrar o código-fonte para este tutorial no repositório [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/TextClassificationTF).

Neste tutorial, você aprendeu a:
> [!div class="checklist"]
>
> * Carregar um modelo de TensorFlow pré-treinado
> * Transformar o texto de comentário do site em recursos adequados para o modelo
> * Usar o modelo para fazer uma previsão
