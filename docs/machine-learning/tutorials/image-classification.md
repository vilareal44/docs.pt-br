---
title: 'Tutorial: modelo de classificação de imagem ML.NET de TensorFlow'
description: Saiba como transferir o conhecimento de um modelo TensorFlow existente para um novo modelo de classificação de imagem ML.NET. O modelo TensorFlow foi treinado para classificar imagens em milhares de categorias. O modelo ML.NET usa o aprendizado de transferência para classificar imagens em menos categorias mais amplas.
ms.date: 06/30/2020
ms.topic: tutorial
ms.custom: mvc, title-hack-0612
ms.openlocfilehash: a4c671816dce1fe2abdf77f81da0f27236136536
ms.sourcegitcommit: 97ce5363efa88179dd76e09de0103a500ca9b659
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2020
ms.locfileid: "86282106"
---
# <a name="tutorial-generate-an-mlnet-image-classification-model-from-a-pre-trained-tensorflow-model"></a>Tutorial: gerar um modelo de classificação de imagem ML.NET de um modelo de TensorFlow pré-treinado

Saiba como transferir o conhecimento de um modelo TensorFlow existente para um novo modelo de classificação de imagem ML.NET.

O modelo TensorFlow foi treinado para classificar imagens em milhares de categorias. O modelo ML.NET faz uso de parte do modelo TensorFlow em seu pipeline para treinar um modelo para classificar imagens em três categorias.

Treinar um modelo de [Classificação de Imagens](https://en.wikipedia.org/wiki/Outline_of_object_recognition) do zero requer a configuração de milhões de parâmetros, uma tonelada de dados de treinamento rotulados e uma grande quantidade de recursos de computação (centenas de horas de GPU). Embora não seja tão eficaz quanto treinar um modelo personalizado do zero, o aprendizado de transferência permite que você ative esse processo trabalhando com milhares de imagens em comparação com milhões de imagens rotuladas e crie um modelo personalizado rapidamente (em uma hora em uma máquina sem GPU). Este tutorial dimensiona o processo ainda mais para cima, usando apenas uma dúzia de imagens de treinamento.

Neste tutorial, você aprenderá como:
> [!div class="checklist"]
>
> * Compreender o problema
> * Incorporar o modelo de TensorFlow pré-treinado ao pipeline do ML.NET
> * Treinar e avaliar o modelo ML.NET
> * Classificar uma imagem de teste

Você pode encontrar o código-fonte para este tutorial no repositório [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/TransferLearningTF). Observe que, por padrão, a configuração de projeto do .NET para este tutorial se destina ao .NET Core 2.2.

## <a name="what-is-transfer-learning"></a>O que é a transferência de aprendizado?

O aprendizado de transferência é o processo de uso do conhecimento obtido ao resolver um problema e aplicá-lo a um problema diferente, mas relacionado.

Para este tutorial, você usa parte de um modelo TensorFlow treinado para classificar imagens em mil categorias – em um modelo ML.NET que classifica imagens em três categorias.

## <a name="prerequisites"></a>Pré-requisitos

* [Visual studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) ou posterior ou visual Studio 2017 versão 15,6 ou posterior com a carga de trabalho "desenvolvimento de plataforma cruzada do .NET Core" instalada.
* [O arquivo .ZIP do diretório de recursos do tutorial](https://github.com/dotnet/samples/blob/master/machine-learning/tutorials/TransferLearningTF/image-classifier-assets.zip)
* [O modelo de machine learning do InceptionV1](https://storage.googleapis.com/download.tensorflow.org/models/inception5h.zip)

## <a name="select-the-right-machine-learning-task"></a>Selecione a tarefa de aprendizado de máquina correta

### <a name="deep-learning"></a>Aprendizado

[Aprendizado profundo](https://en.wikipedia.org/wiki/Deep_learning) é um subconjunto do aprendizado de máquina, que está revolucionando áreas como Pesquisa Visual Computacional e Reconhecimento de Fala.

Os modelos de aprendizado profundo são treinados usando grandes conjuntos de [dados rotulados](https://en.wikipedia.org/wiki/Labeled_data) e [redes neurais](https://en.wikipedia.org/wiki/Artificial_neural_network) que contêm várias camadas de aprendizado. Aprendizado profundo:

* Funciona melhor em algumas tarefas como a Pesquisa Visual Computacional.
* Requer enormes quantidades de dados de treinamento.

A classificação de imagem é uma tarefa Machine Learning comum que nos permite classificar automaticamente as imagens em categorias como:

* Detectar uma face humana em uma imagem ou não.
* Detectando gatos vs. cachorros.

 Ou como nas imagens a seguir, determinando se uma imagem é um (n) comida, Toy ou dispositivo:

![imagem de pizza](./media/image-classification/220px-Pepperoni_pizza.jpg)
![imagem de ursinho de pelúcia](./media/image-classification/119px-Nalle_-_a_small_brown_teddy_bear.jpg)
![imagem de torradeira](./media/image-classification/193px-Broodrooster.jpg)

>[!Note]
> As imagens anteriores pertencem ao Wikimedia Commons e são atribuídas da seguinte forma:
>
> * "220px-Pepperoni_pizza.jpg" Public Domain, <https://commons.wikimedia.org/w/index.php?curid=79505>,
> * "119px-Nalle_-_a_small_brown_teddy_bear.jpg" By [Jonik](https://commons.wikimedia.org/wiki/User:Jonik) - Self-photographed, CC BY-SA 2.0, <https://commons.wikimedia.org/w/index.php?curid=48166>.
> * "193px-Broodrooster.jpg" By [M.Minderhoud](https://nl.wikipedia.org/wiki/Gebruiker:Michiel1972) - Own work, CC BY-SA 3.0, <https://commons.wikimedia.org/w/index.php?curid=27403>

O `Inception model` é treinado para classificar imagens em milhares de categorias, mas, para este tutorial, você precisa classificar imagens em um conjunto de categorias menor e apenas essas categorias. Digite a parte `transfer` de `transfer learning`. Você pode transferir a capacidade de `Inception model` de reconhecer e classificar imagens para as novas categorias limitadas do seu classificador de imagens personalizadas.

* Alimentos
* Brinquedos
* Dispositivo

Este tutorial usa o modelo de aprendizado profundo de [modelo](https://storage.googleapis.com/download.tensorflow.org/models/inception5h.zip) de TensorFlow, um modelo de reconhecimento de imagem popular treinado no conjunto de informações `ImageNet` . O modelo TensorFlow classifica imagens inteiras em milhares de classes, como "abrangência", "Jersey" e "dishwasher".

Como o `Inception model` já foi pré-treinado em milhares de imagens diferentes, internamente ele contém os [recursos de imagem](https://en.wikipedia.org/wiki/Feature_(computer_vision)) necessários para a identificação da imagem. Podemos fazer uso desses recursos de imagem interna no modelo para treinar um novo modelo com muito menos classes.

Conforme mostrado no diagrama a seguir, você adiciona uma referência aos pacotes ML.NET NuGet em seus aplicativos .NET Core ou .NET Framework. Nos bastidores, ML.NET inclui e faz referência à `TensorFlow` biblioteca nativa que permite que você escreva o código que carrega um `TensorFlow` arquivo de modelo treinado existente.

![Diagrama de arco da transformação do TensorFlow do ML.NET](./media/image-classification/tensorflow-mlnet.png)

### <a name="multiclass-classification"></a>Classificação multiclasse

Depois de usar o modelo de criação de TensorFlow para extrair recursos adequados como entrada para um algoritmo de aprendizado de máquina clássico, adicionamos um [classificador de várias classes](../resources/tasks.md#multiclass-classification)ml.net.

O treinador específico usado nesse caso é o [algoritmo de regressão logística multinomial](https://en.wikipedia.org/wiki/Multinomial_logistic_regression).

O algoritmo implementado por esse instrutor tem um bom desempenho em problemas com um grande número de recursos, que é o caso de um modelo de aprendizado profundo operando em dados de imagem.

### <a name="data"></a>Dados

Há duas fontes de dados: o arquivo `.tsv` e os arquivos de imagem.  O arquivo `tags.tsv` contém duas colunas: a primeira é definida como `ImagePath` e a segunda é a `Label` correspondente à imagem. O arquivo de exemplo a seguir não possui uma linha de cabeçalho e tem a seguinte aparência:

<!-- markdownlint-disable MD010 -->
```tsv
broccoli.jpg    food
pizza.jpg   food
pizza2.jpg  food
teddy2.jpg  toy
teddy3.jpg  toy
teddy4.jpg  toy
toaster.jpg appliance
toaster2.png    appliance
```
<!-- markdownlint-enable MD010 -->

As imagens de treinamento e teste estão localizadas nas pastas de recursos que você baixará em um arquivo zip. Essas imagens pertencem ao Wikimedia Commons.
> *[Wikimedia Commons](https://commons.wikimedia.org/w/index.php?title=Main_Page&oldid=313158208), o repositório de mídia gratuito.* Recuperado 10:48, 17 de outubro de 2018 de:<https://commons.wikimedia.org/wiki/Pizza>
> <https://commons.wikimedia.org/wiki/Toaster>
> <https://commons.wikimedia.org/wiki/Teddy_bear>

## <a name="setup"></a>Configuração

### <a name="create-a-project"></a>Criar um projeto

1. Criar um **Aplicativo de Console .NET Core** chamado "TransferLearningTF".

1. Instalar o **Pacote NuGet Microsoft.ML**:

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    * No Gerenciador de Soluções, clique com o botão direito do mouse no seu projeto e selecione **Gerenciar Pacotes NuGet**.
    * Escolha "nuget.org" como a fonte do pacote, selecione a guia Browse, procure por **Microsoft.ML**.
    * Selecione o botão **Instalar**.
    * Selecione o botão **OK** na caixa de diálogo **Visualizar alterações** .
    * Selecione o botão **aceito** na caixa de diálogo de **aceitação da licença** se você concordar com os termos de licença dos pacotes listados.
    * Repita essas etapas para **Microsoft. ml. ImageAnalytics**, **SciSharp. TensorFlow. Redist** e **Microsoft. ml. TensorFlow**.

### <a name="download-assets"></a>Baixar ativos

1. Baixe [o arquivo zip do diretório de ativos do projeto](https://github.com/dotnet/samples/blob/master/machine-learning/tutorials/TransferLearningTF/image-classifier-assets.zip)e descompacte.

1. Copie o diretório`assets` no diretório do projeto *TransferLearningTF*. Este diretório e seus subdiretórios contêm os arquivos de dados e de suporte (exceto o modelo Concepção, que você irá baixar e adicionar na próxima etapa) necessários para este tutorial.

1. Baixe o [modelo Concepção](https://storage.googleapis.com/download.tensorflow.org/models/inception5h.zip) e descompacte-o.

1. Copie o conteúdo do diretório `inception5h` apenas descompactado em seu diretório de projeto *TransferLearningTF*`assets/inception`. Este diretório contém o modelo e os arquivos de suporte adicionais necessários para este tutorial, conforme mostrado na imagem a seguir:

   ![Conteúdo do diretório de concepção](./media/image-classification/inception-files.png)

1. No Gerenciador de Soluções, clique com o botão direito do mouse em cada um dos arquivos no diretório e nos subdiretórios do ativo e selecione**Propriedades**. Em **avançado**, altere o valor de **copiar para diretório de saída** para **copiar se mais recente**.

### <a name="create-classes-and-define-paths"></a>Criar classes e definir demarcadores

1. Adicione as seguintes instruções `using` adicionais ao início do arquivo *Program.cs*: 

    [!code-csharp[AddUsings](./snippets/image-classification/csharp/Program.cs#AddUsings)]

1. Adicione o seguinte código à linha à direita acima do `Main` método para especificar os caminhos de ativo:

    [!code-csharp[DeclareGlobalVariables](./snippets/image-classification/csharp/Program.cs#DeclareGlobalVariables)]

1. Crie classes para os dados de entrada e previsões.

    [!code-csharp[DeclareImageData](./snippets/image-classification/csharp/Program.cs#DeclareImageData)]

    `ImageData` é a classe de conjunto de dados de entrada e tem os seguintes campos <xref:System.String>:

    * `ImagePath` contém o nome do arquivo de imagem.
    * `Label` contém um valor para o rótulo da imagem.

1. Adicione uma nova classe ao seu projeto para `ImagePrediction`:

    [!code-csharp[DeclareImagePrediction](./snippets/image-classification/csharp/Program.cs#DeclareImagePrediction)]

    `ImagePrediction` é a classe de previsão de imagem e possui os seguintes campos:

    * `Score` contém a porcentagem de confiança para uma determinada classificação de imagem.
    * `PredictedLabelValue` contém um valor para o rótulo de classificação de imagem previsto.

    `ImagePrediction` é a classe usada para previsão depois que o modelo foi treinado. Tem um `string` (`ImagePath`) para o demarcador da imagem. O `Label` é usado para reutilizar e treinar o modelo. O `PredictedLabelValue` é usado durante a previsão e avaliação. Para avaliação, uma entrada com dados de treinamento, os valores previstos e o modelo são usados.

### <a name="initialize-variables-in-main"></a>Inicializar variáveis em Main

1. Inicialize a variável `mlContext` com uma nova instância de `MLContext`.  Substitua a linha `Console.WriteLine("Hello World!")` pelo seguinte código no método `Main`:

    [!code-csharp[CreateMLContext](./snippets/image-classification/csharp/Program.cs#CreateMLContext)]

    A [classe MLContext](xref:Microsoft.ML.MLContext) é um ponto de partida para todas as operações do ML.NET e a inicialização do `mlContext` cria um ambiente do ML.NET que pode ser compartilhado entre os objetos de fluxo de trabalho da criação de modelo. Ele é semelhante, conceitualmente, a `DBContext` no Entity Framework.

### <a name="create-a-struct-for-inception-model-parameters"></a>Criar uma estrutura para parâmetros de modelo de criação

1. O modelo de início tem vários parâmetros que você precisa passar. Crie um struct para mapear os valores de parâmetro para nomes amigáveis com o seguinte código, logo após o `Main()` método:

    [!code-csharp[InceptionSettings](./snippets/image-classification/csharp/Program.cs#InceptionSettings)]

### <a name="create-a-display-utility-method"></a>Criar um método de utilitário de exibição

Uma vez que você exibirá os dados de imagem e as previsões relacionadas mais de uma vez, crie um método de utilitário de exibição para lidar com a exibição dos resultados de imagem e previsão.

1. Crie o método `DisplayResults()`, logo após o struct `InceptionSettings`, usando o seguinte código:

    ```csharp
    private static void DisplayResults(IEnumerable<ImagePrediction> imagePredictionData)
    {

    }
    ```

1. Preencha o corpo do `DisplayResults` método:

    [!code-csharp[DisplayPredictions](./snippets/image-classification/csharp/Program.cs#DisplayPredictions)]

### <a name="create-a-tsv-file-utility-method"></a>Criar um método de utilitário de arquivo .tsv

1. Crie o método `ReadFromTsv()`, logo após o método `DisplayResults()`, usando o seguinte código:

    ```csharp
    public static IEnumerable<ImageData> ReadFromTsv(string file, string folder)
    {

    }
    ```

1. Preencha o corpo do `ReadFromTsv` método:

    [!code-csharp[ReadFromTsv](./snippets/image-classification/csharp/Program.cs#ReadFromTsv)]

    O código analisa o `tags.tsv` arquivo para adicionar o caminho do arquivo ao nome do arquivo de imagem da `ImagePath` propriedade e carregá-lo e `Label` em um `ImageData` objeto.

### <a name="create-a-method-to-make-a-prediction"></a>Criar um método para fazer uma previsão

1. Crie o método `ClassifySingleImage()`, logo antes do método `DisplayResults()`, usando o seguinte código:

    ```csharp
    public static void ClassifySingleImage(MLContext mlContext, ITransformer model)
    {

    }
    ```

1. Crie um `ImageData` objeto que contenha o caminho totalmente qualificado e o nome do arquivo de imagem para o único `ImagePath` . Adicione o seguinte código como as próximas linhas no método `ClassifySingleImage()`:

    [!code-csharp[LoadImageData](./snippets/image-classification/csharp/Program.cs#LoadImageData)]

1. Faça uma única previsão adicionando o seguinte código como a próxima linha do `ClassifySingleImage` método:

    [!code-csharp[PredictSingle](./snippets/image-classification/csharp/Program.cs#PredictSingle)]

    Para obter a previsão, use o método [Predict ()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) . O [PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) é uma API de conveniência, que permite que você execute uma previsão em uma única instância de dados. [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602)Não é thread-safe. É aceitável usar em ambientes de protótipo ou de thread único. Para melhorar o desempenho e a segurança de thread em ambientes de produção, use o `PredictionEnginePool` serviço, que cria um [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601) dos [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) objetos para uso em todo o aplicativo. Consulte este guia sobre como [usar o `PredictionEnginePool` em uma API Web do ASP.NET Core](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application).

    > [!NOTE]
    > A extensão de serviço `PredictionEnginePool` está atualmente em versão prévia.

1. Exiba o resultado da previsão como a próxima linha de código no método `ClassifySingleImage()`:

   [!code-csharp[DisplayPrediction](./snippets/image-classification/csharp/Program.cs#DisplayPrediction)]

## <a name="construct-the-mlnet-model-pipeline"></a>Construir o pipeline de modelo ML.NET

Um pipeline de modelo ML.NET é uma cadeia de estimadores. Observe que nenhuma execução ocorre durante a construção do pipeline. Os objetos do estimador são criados, mas não são executados.

1. Adicionar um método para gerar o modelo

    Esse método é o coração do tutorial. Ele cria um pipeline para o modelo e treina o pipeline para produzir o modelo ML.NET. Ele também avalia o modelo em relação a alguns dados de teste não vistos anteriormente.

    Crie o método `GenerateModel()` logo após o struct `InceptionSettings` e antes do método `DisplayResults()`, usando o seguinte código:

    ```csharp
    public static ITransformer GenerateModel(MLContext mlContext)
    {

    }
    ```

1. Adicione os estimadores para carregar, redimensionar e extrair os pixels dos dados da imagem:

    [!code-csharp[ImageTransforms](./snippets/image-classification/csharp/Program.cs#ImageTransforms)]

    Os dados da imagem precisam ser processados no formato esperado pelo modelo TensorFlow. Nesse caso, as imagens são carregadas na memória, redimensionadas para um tamanho consistente e os pixels são extraídos em um vetor numérico.

1. Adicione o estimador para carregar o modelo TensorFlow e pontua-lo:

    [!code-csharp[ScoreTensorFlowModel](./snippets/image-classification/csharp/Program.cs#ScoreTensorFlowModel)]

    Esse estágio no pipeline carrega o modelo TensorFlow na memória e, em seguida, processa o vetor de valores de pixel por meio da rede do modelo TensorFlow. Aplicar entradas a um modelo de aprendizado profundo e gerar uma saída usando o modelo é conhecido como **Pontuação**. Ao usar o modelo em sua totalidade, a pontuação faz uma inferência ou previsão.

    Nesse caso, você usa todo o modelo TensorFlow, exceto a última camada, que é a camada que faz a inferência. A saída da camada penúltima é rotulada `softmax_2_preactivation` . A saída dessa camada é efetivamente um vetor de recursos que caracteriza as imagens de entrada originais.

    Esse vetor de recurso gerado pelo modelo TensorFlow será usado como entrada para um algoritmo de treinamento ML.NET.

1. Adicione o estimador para mapear os rótulos de cadeia de caracteres nos dados de treinamento para valores de chave de inteiro:

    [!code-csharp[MapValueToKey](./snippets/image-classification/csharp/Program.cs#MapValueToKey)]

    O treinador ML.NET que é acrescentado em seguida requer que seus rótulos estejam no `key` formato em vez de cadeias de caracteres arbitrárias. Uma chave é um número que tem um mapeamento de um para um para um valor de cadeia de caracteres.

1. Adicione o algoritmo de treinamento ML.NET:

    [!code-csharp[AddTrainer](./snippets/image-classification/csharp/Program.cs#AddTrainer)]

1. Adicione o estimador para mapear o valor de chave previsto de volta para uma cadeia de caracteres:

    [!code-csharp[MapKeyToValue](./snippets/image-classification/csharp/Program.cs#MapKeyToValue)]

## <a name="train-the-model"></a>Treinar o modelo

1. Carregue os dados de treinamento usando o wrapper [LoadFromTextFile](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile(Microsoft.ML.DataOperationsCatalog,System.String,Microsoft.ML.Data.TextLoader.Options)) . Adicione o seguinte código ao método `GenerateModel()` como a linha seguinte:

    [!code-csharp[LoadData](./snippets/image-classification/csharp/Program.cs#LoadData "Load the data")]

    Os dados do ML.NET são representados como uma [classe IDataView](xref:Microsoft.ML.IDataView). `IDataView` é uma maneira flexível e eficiente de descrever dados tabulares (numéricos e texto). Os dados podem ser carregados de um arquivo de texto ou em tempo real (por exemplo, banco de dados SQL ou arquivos de log) para um objeto `IDataView`.

1. Treine o modelo com os dados carregados acima:

    [!code-csharp[TrainModel](./snippets/image-classification/csharp/Program.cs#TrainModel)]

    O `Fit()` método treina seu modelo aplicando o conjunto de os de treinamento ao pipeline.

## <a name="evaluate-the-accuracy-of-the-model"></a>Avaliar a precisão do modelo

1. Carregue e transforme os dados de teste, adicionando o seguinte código à próxima linha do `GenerateModel` método:

    [!code-csharp[LoadAndTransformTestData](./snippets/image-classification/csharp/Program.cs#LoadAndTransformTestData "Load and transform test data")]

    Há algumas imagens de exemplo que você pode usar para avaliar o modelo. Assim como os dados de treinamento, eles precisam ser carregados em um `IDataView` , para que possam ser transformados pelo modelo.

1. Adicione o seguinte código ao `GenerateModel()` método para avaliar o modelo:

    [!code-csharp[Evaluate](./snippets/image-classification/csharp/Program.cs#Evaluate)]

    Depois de determinar a previsão, o método [Evaluate ()](xref:Microsoft.ML.RecommendationCatalog.Evaluate%2A):

    * Avalia o modelo (compara os valores previstos com o conjunto de testes `labels` ).
    * Retorna as métricas de desempenho do modelo.

1. Exibir as métricas de precisão do modelo

    Use o código a seguir para exibir as métricas, compartilhar os resultados e, em seguida, agir com relação a eles:

    [!code-csharp[DisplayMetrics](./snippets/image-classification/csharp/Program.cs#DisplayMetrics)]

    As seguintes métricas são avaliadas para classificação de imagem:

    * `Log-loss` - confira [Perda de log](../resources/glossary.md#log-loss). Convém que a Perda de log seja tão próxima de zero quanto possível.
    * `Per class Log-loss`. Convém que a Perda de log por classe seja tão próxima de zero quanto possível.

1. Adicione o seguinte código para retornar o modelo treinado como a próxima linha:

    [!code-csharp[SaveModel](./snippets/image-classification/csharp/Program.cs#ReturnModel)]

## <a name="run-the-application"></a>Execute o aplicativo!

1. Adicione a chamada para `GenerateModel` no `Main` método após a criação da classe MLContext:

    [!code-csharp[CallGenerateModel](./snippets/image-classification/csharp/Program.cs#CallGenerateModel)]

1. Adicione a chamada ao `ClassifySingleImage()` método como a próxima linha de código no `Main` método:

    [!code-csharp[CallClassifySingleImage](./snippets/image-classification/csharp/Program.cs#CallClassifySingleImage)]

1. Execute o aplicativo de console (Ctrl + F5). O resultado deverá ser semelhante à seguinte saída.  Você poderá ver avisos ou mensagens de processamento, mas essas mensagens foram removidas dos resultados a seguir para maior clareza.

    ```console
    =============== Training classification model ===============
    Image: broccoli2.jpg predicted as: food with score: 0.8955513
    Image: pizza3.jpg predicted as: food with score: 0.9667718
    Image: teddy6.jpg predicted as: toy with score: 0.9797683
    =============== Classification metrics ===============
    LogLoss is: 0.0653774699265059
    PerClassLogLoss is: 0.110315812569315 , 0.0204391272836966 , 0
    =============== Making single image classification ===============
    Image: toaster3.jpg predicted as: appliance with score: 0.9646884
    ```

Parabéns! Agora você criou com êxito um modelo de aprendizado de máquina para classificação de imagem aplicando o aprendizado de transferência a um `TensorFlow` modelo no ml.net.

Você pode encontrar o código-fonte para este tutorial no repositório [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/TransferLearningTF).

Neste tutorial, você aprendeu a:
> [!div class="checklist"]
>
> * Compreender o problema
> * Incorporar o modelo de TensorFlow pré-treinado ao pipeline do ML.NET
> * Treinar e avaliar o modelo ML.NET
> * Classificar uma imagem de teste

Conferir o repositório GitHub de Amostras de Aprendizado de Máquina para explorar uma amostra de classificação de imagem expandida.
> [!div class="nextstepaction"]
> [dotnet/machinelearning-samples GitHub repository](https://github.com/dotnet/machinelearning-samples/)
