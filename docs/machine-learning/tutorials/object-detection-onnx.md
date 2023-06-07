---
title: 'Tutorial: detectar objetos usando um modelo de aprendizado profundo do ONNX'
description: Este tutorial mostra como usar um modelo de aprendizado profundo ONNX pré-treinado no ML.NET para detectar objetos em imagens.
author: luisquintanilla
ms.author: luquinta
ms.date: 06/30/2020
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 4759a661646b08ea6a93cab030a19af2cfeaca16
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85803398"
---
# <a name="tutorial-detect-objects-using-onnx-in-mlnet"></a>Tutorial: detectar objetos usando ONNX no ML.NET

Saiba como usar um modelo ONNX pré-treinado no ML.NET para detectar objetos em imagens.

Treinar um modelo de detecção de objetos do zero requer a configuração de milhões de parâmetros, uma grande quantidade de dados de treinamento rotulados e uma grande quantidade de recursos de computação (centenas de horas de GPU). Usar um modelo pré-treinado permite que você reduza o processo de treinamento.

Neste tutorial, você aprenderá como:
> [!div class="checklist"]
>
> - Compreender o problema
> - Saiba o que é o ONNX e como ele funciona com o ML.NET
> - Entender o modelo
> - Reutilizar o modelo pré-treinado
> - Detectar objetos com um modelo carregado

## <a name="pre-requisites"></a>Pré-requisitos

- [Visual studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) ou posterior ou visual Studio 2017 versão 15,6 ou posterior com a carga de trabalho "desenvolvimento de plataforma cruzada do .NET Core" instalada.
- [Pacote NuGet do Microsoft.ML](https://www.nuget.org/packages/Microsoft.ML/)
- [Pacote NuGet Microsoft.ML.ImageAnalytics](https://www.nuget.org/packages/Microsoft.ML.ImageAnalytics/)
- [Pacote NuGet Microsoft.ML.OnnxTransformer](https://www.nuget.org/packages/Microsoft.ML.OnnxTransformer/)
- [Modelo pré-treinado Tiny YOLOv2](https://github.com/onnx/models/tree/master/vision/object_detection_segmentation/tiny-yolov2)
- [Netron](https://github.com/lutzroeder/netron) (opcional)

## <a name="onnx-object-detection-sample-overview"></a>Visão geral do exemplo de detecção de objetos ONNX

Este exemplo cria um aplicativo de console .NET Core que detecta objetos em uma imagem usando um modelo ONNX de aprendizado profundo pré-treinado. O código para este exemplo pode ser encontrado no [repositório dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/master/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx) no GitHub.

## <a name="what-is-object-detection"></a>O que é a detecção de objetos?

A detecção de objetos é um problema da pesquisa visual computacional. Embora esteja bem relacionada à classificação de imagem, a detecção de objetos executa a classificação de imagem em uma escala mais granular. A detecção de objetos localiza _e_ categoriza entidades dentro de imagens. Use a detecção de objetos quando as imagens contiverem vários objetos de tipos diferentes.

![Capturas de tela mostrando classificação de imagem versus classificação de objeto.](./media/object-detection-onnx/img-classification-obj-detection.png)

Alguns casos de uso para detecção de objetos incluem:

- Carros autônomos
- Robótica
- Detecção Facial
- Segurança do local de trabalho
- Contagem de objetos
- Reconhecimento de atividades

## <a name="select-a-deep-learning-model"></a>Selecionar um modelo de aprendizado profundo

O aprendizado profundo é um subconjunto de aprendizado de máquina. Para treinar modelos de aprendizado profundo, grandes quantidades de dados são necessárias. Os padrões nos dados são representados por uma série de camadas. As relações nos dados são codificadas como conexões entre as camadas que contêm pesos. Quanto maior o peso, mais forte a relação. Coletivamente, essa série de camadas e conexões é conhecida como redes neurais artificiais. Quanto mais camadas em uma rede, "mais profunda" ela será, tornando-a uma rede neural profunda.

Há diferentes tipos de redes neurais, as mais comuns são a MLP (Perceptron Multicamadas), a CNN (Rede Neural de Convolução) e a RNN (Rede Neural Recorrente). A mais básica é a MLP, que mapeia um conjunto de entradas para um conjunto de saídas. Essa rede neural é boa quando os dados não têm um componente espacial ou de tempo. A CNN usa camadas de convolução para processar informações espaciais contidas nos dados. Um bom caso de uso para as CNNs é o processamento de imagens para detectar a presença de um recurso em uma região de uma imagem (por exemplo, há um nariz no centro de uma imagem?). Por fim, as RNNs permitem que a persistência do estado ou da memória seja usada como entrada. As RNNs são usadas para análise de série temporal, em que a ordenação sequencial e o contexto dos eventos são importantes.

### <a name="understand-the-model"></a>Entender o modelo

A detecção de objetos é uma tarefa de processamento de imagens. Portanto, os modelos de aprendizado profundo mais treinados para resolver esse problema são as CNNs. O modelo usado neste tutorial é o pequeno modelo de YOLOv2, uma versão mais compacta do modelo YOLOv2 descrito no documento: ["YOLO9000: melhor, mais rápido e mais forte" por Redmon e Farhadi](https://arxiv.org/pdf/1612.08242.pdf). O Tiny YOLOv2 é treinado no conjunto de dados do Pascal VOC e é composto por 15 camadas que podem prever 20 classes diferentes de objetos. Como o Tiny YOLOv2 é uma versão condensada do modelo YOLOv2 original, é feita uma compensação entre velocidade e precisão. As diferentes camadas que compõem o modelo podem ser visualizadas usando ferramentas como o Netron. Inspecionar o modelo produziria um mapeamento das conexões entre todas as camadas que compõem a rede neural, em que cada camada conteria o nome da camada junto com as dimensões da respectiva entrada/saída. As estruturas de dados usadas para descrever as entradas e as saídas do modelo são conhecidas como tensores. Os tensores podem ser entendidos como contêineres que armazenam dados em N dimensões. No caso do Tiny YOLOv2, o nome da camada de entrada é `image` e ele espera um tensor de dimensões `3 x 416 x 416`. O nome da camada de saída é `grid` e gera um tensor de saída de dimensões `125 x 13 x 13`.

![Camada de entrada sendo dividida em camadas ocultas e, em seguida, camada de saída](./media/object-detection-onnx/netron-model-map-layers.png)

O modelo YOLO usa uma imagem `3(RGB) x 416px x 416px`. O modelo usa essa entrada e a passa pelas diferentes camadas para produzir uma saída. A saída divide a imagem de entrada em uma grade `13 x 13`, com cada célula na grade consistindo em `125` valores.

### <a name="what-is-an-onnx-model"></a>O que é um modelo ONNX?

O ONNX (Open Neural Network Exchange) é um formato de software livre para modelos de IA. O ONNX é compatível com a interoperabilidade entre estruturas. Isso significa que você pode treinar um modelo em uma das muitas estruturas de aprendizado de máquina populares, como PyTorch, convertê-la em formato ONNX e consumir o modelo ONNX em uma estrutura diferente, como ML.NET. Para saber mais, visite o [site do ONNX](https://onnx.ai/).

![Diagrama de formatos com suporte ONNX que estão sendo usados.](./media/object-detection-onnx/onnx-supported-formats.png)

O modelo Tiny YOLOv2 pré-treinado é armazenado no formato ONNX, uma representação serializada das camadas e dos padrões aprendidos dessas camadas. No ML.NET, a interoperabilidade com o ONNX é obtida com os [`ImageAnalytics`](xref:Microsoft.ML.Transforms.Image) pacotes do e [`OnnxTransformer`](xref:Microsoft.ML.Transforms.Onnx.OnnxTransformer) NuGet. O [`ImageAnalytics`](xref:Microsoft.ML.Transforms.Image) pacote contém uma série de transformações que pegam uma imagem e a codificam em valores numéricos que podem ser usados como entrada em um pipeline de previsão ou de treinamento. O [`OnnxTransformer`](xref:Microsoft.ML.Transforms.Onnx.OnnxTransformer) pacote aproveita o tempo de execução ONNX para carregar um modelo ONNX e usá-lo para fazer previsões com base na entrada fornecida.

![Fluxo de dados do arquivo ONNX para o tempo de execução do ONNX.](./media/object-detection-onnx/onnx-ml-net-integration.png)

## <a name="set-up-the-net-core-project"></a>Configurar o projeto do .NET Core

Agora que você tem um entendimento geral do que é o ONNX e de como o Tiny YOLOv2 funciona, é hora de criar o aplicativo.

### <a name="create-a-console-application"></a>Criar um aplicativo de console

1. Crie um **Aplicativo de console do .NET Core** chamado "ObjectDetection".

1. Instalar o **Pacote NuGet Microsoft.ML**:

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    - No Gerenciador de Soluções, clique com o botão direito do mouse no seu projeto e selecione **Gerenciar Pacotes NuGet**.
    - Escolha "nuget.org" como a fonte do pacote, selecione a guia Browse, procure por **Microsoft.ML**.
    - Selecione o botão **Instalar**.
    - Selecione o botão **OK** na caixa de diálogo **Visualizar Alterações** e selecione o botão **Aceito** na caixa de diálogo **Aceitação da Licença**, se concordar com o termos de licença para os pacotes listados.
    - Repita essas etapas para **Microsoft. ml. ImageAnalytics**, **Microsoft. ml. OnnxTransformer** e **Microsoft. ml. OnnxRuntime**.

### <a name="prepare-your-data-and-pre-trained-model"></a>Prepare seus dados e um modelo pré-treinado

1. Baixe o [arquivo zip do diretório de ativos do projeto](https://github.com/dotnet/machinelearning-samples/raw/master/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/assets.zip) e descompacte.

1. Copie o diretório `assets` no diretório do projeto *ObjectDetection*. Este diretório e seus subdiretórios contêm os arquivos de imagem (exceto para o modelo Tiny YOLOv2, que você baixará e adicionará na próxima etapa) necessários para este tutorial.

1. Baixe o [modelo Tiny YOLOv2](https://onnxzoo.blob.core.windows.net/models/opset_8/tiny_yolov2/tiny_yolov2.tar.gz) de [ONNX Model Zoo](https://github.com/onnx/models/tree/master/vision/object_detection_segmentation/tiny-yolov2) e descompacte.

    Abra o prompt de comando e insira o seguinte comando:

    ```shell
    tar -xvzf tiny_yolov2.tar.gz
    ```

1. Copie o arquivo `model.onnx` extraído do diretório que acabou de descompactar em seu diretório de projeto *ObjectDetection*`assets\Model` e renomeie-o como `TinyYolo2_model.onnx`. Este diretório contém o modelo necessário para este tutorial.

1. No Gerenciador de Soluções, clique com o botão direito do mouse em cada um dos arquivos no diretório e nos subdiretórios do ativo e selecione**Propriedades**. Em **avançado**, altere o valor de **copiar para diretório de saída** para **copiar se mais recente**.

### <a name="create-classes-and-define-paths"></a>Criar classes e definir demarcadores

Abra o arquivo *Program.cs* e adicione as seguintes instruções `using` complementares à parte superior do arquivo:

[!code-csharp [ProgramUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L1-L7)]

Em seguida, defina os caminhos dos vários ativos.

1. Primeiro, adicione o método `GetAbsolutePath` abaixo do método `Main` na classe `Program`.

    [!code-csharp [GetAbsolutePath](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L66-L74)]

1. Em seguida, dentro do método `Main`, crie campos para armazenar a localização dos ativos.

    [!code-csharp [AssetDefinition](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L17-L21)]

Adicione um novo diretório ao seu projeto para armazenar os dados de entrada e as classes de previsão.

No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **Nova Pasta**. Quando a nova pasta aparecer no Gerenciador de Soluções, nomeie-a como "DataStructures".

Crie sua classe de dados de entrada no diretório *DataStructures* recém-criado.

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no diretório *DataStructures* e, em seguida, selecione **Adicionar** > **Novo Item**.
1. Na caixa de diálogo **Adicionar Novo Item**, selecione **Classe** e altere o campo **Nome** para *ImageNetData.cs*. Em seguida, selecione o botão **Adicionar**.

    O arquivo *ImageNetData.cs* é aberto no editor de códigos. Adicione a seguinte instrução `using` à parte superior do *ImageNetData.cs*:

    [!code-csharp [ImageNetDataUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/DataStructures/ImageNetData.cs#L1-L4)]

    Remova a definição de classe existente e adicione o seguinte código para a classe `ImageNetData` do arquivo *ImageNetData.cs*:

    [!code-csharp [ImageNetDataClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/DataStructures/ImageNetData.cs#L8-L23)]

    `ImageNetData` é a classe de conjunto de dados de entrada e tem os seguintes campos <xref:System.String>:

    - Um `ImagePath` que contém o caminho no qual a imagem é armazenada.
    - Um `Label` que contém o nome do arquivo.

    Além disso, `ImageNetData` o contém um método `ReadFromFile` que carrega vários arquivos de imagem armazenados no `imageFolder` caminho especificado e os retorna como uma coleção de `ImageNetData` objetos.

Crie sua classe de previsão no diretório *DataStructures*.

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no diretório *DataStructures* e, em seguida, selecione **Adicionar** > **Novo Item**.
1. Na caixa de diálogo **Adicionar Novo Item**, selecione **Classe** e altere o campo **Nome** para *ImageNetPrediction.cs*. Em seguida, selecione o botão **Adicionar**.

    O arquivo *ImageNetPrediction.cs* é aberto no editor de códigos. Adicione a seguinte instrução `using` à parte superior do *ImageNetPrediction.cs*:

    [!code-csharp [ImageNetPredictionUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/DataStructures/ImageNetPrediction.cs#L1)]

    Remova a definição de classe existente e adicione o seguinte código para a classe `ImageNetPrediction` do arquivo *ImageNetPrediction.cs*:

    [!code-csharp [ImageNetPredictionClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/DataStructures/ImageNetPrediction.cs#L5-L9)]

    O `ImageNetPrediction` é a classe de dados de previsão e conta com os seguintes `float[]` campos:

    - `PredictedLabel`contém as dimensões, a pontuação de objeções e as probabilidades de classe para cada uma das caixas delimitadas detectadas em uma imagem.

### <a name="initialize-variables-in-main"></a>Inicializar variáveis em Main

A [classe MLContext](xref:Microsoft.ML.MLContext) é um ponto de partida para todas as operações do ML.NET e a inicialização do `mlContext` cria um ambiente do ML.NET que pode ser compartilhado entre os objetos de fluxo de trabalho da criação de modelo. Ele é semelhante, conceitualmente, a `DBContext` no Entity Framework.

Inicialize a variável `mlContext` com uma nova instância de `MLContext` adicionando a linha a seguir ao método `Main` de *Program.cs* abaixo do campo `outputFolder`.

[!code-csharp [InitMLContext](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L24)]

## <a name="create-a-parser-to-post-process-model-outputs"></a>Criar um analisador para saídas de modelo de pós-processamento

O modelo segmenta uma imagem em uma grade `13 x 13`, em que cada célula de grade é `32px x 32px`. Cada célula de grade contém cinco caixas delimitadoras de objetos potenciais. Uma caixa delimitadora tem 25 elementos:

![Exemplo de grade à esquerda e o exemplo de caixa delimitadora à direita](./media/object-detection-onnx/model-output-description.png)

- `x`: a posição x do centro da caixa delimitadora relativa à célula da grade à qual ela está associada.
- `y`: a posição y do centro da caixa delimitadora relativa à célula da grade à qual ela está associada.
- `w`: a largura da caixa delimitadora.
- `h`: a altura da caixa delimitadora.
- `o`: o valor de confiança de que um objeto existe dentro da caixa delimitadora, também conhecido como pontuação de objeções.
- `p1-p20`: probabilidades de classe para cada uma das 20 classes previstas pelo modelo.

No total, os 25 elementos que descrevem cada uma das cinco caixas delimitadoras compõem os 125 elementos contidos em cada célula de grade.

A saída gerada pelo modelo ONNX pré-treinado é uma matriz float de cumprimento `21125`, representando os elementos de um tensor com dimensões `125 x 13 x 13`. Para transformar as previsões geradas pelo modelo em um tensor, é necessário algum trabalho de pós-processamento. Para fazer isso, crie um conjunto de classes para ajudar a analisar a saída.

Adicione um novo diretório ao seu projeto para organizar o conjunto de classes do analisador.

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **Nova Pasta**. Quando a nova pasta aparecer no Gerenciador de Soluções, nomeie-a como "YoloParser".

### <a name="create-bounding-boxes-and-dimensions"></a>Criar caixas delimitadoras e dimensões

A saída de dados pelo modelo contém coordenadas e dimensões das caixas delimitadoras de objetos dentro da imagem. Criar uma classe base para dimensões.

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no diretório *YoloParser* e, em seguida, selecione **Adicionar** > **Novo Item**.
1. Na caixa de diálogo **Adicionar Novo Item**, selecione **Classe** e altere o campo **Nome** para *DimensionsBase.cs*. Em seguida, selecione o botão **Adicionar**.

    O arquivo *DimensionsBase.cs* é aberto no editor de códigos. Remova todas as instruções `using` e a definição de classe existente.

    Adicione o seguinte código à classe `DimensionsBase` ao arquivo *DimensionsBase.cs*:

    [!code-csharp [DimensionsBaseClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/DimensionsBase.cs#L3-L9)]

    `DimensionsBase`tem as seguintes `float` Propriedades:

    - `X` contém a posição do objeto ao longo do eixo x.
    - `Y` contém a posição do objeto ao longo do eixo y.
    - `Height` contém a altura do objeto.
    - `Width` contém a largura do objeto.

Em seguida, crie uma classe para suas caixas delimitadoras.

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no diretório *YoloParser* e, em seguida, selecione **Adicionar** > **Novo Item**.
1. Na caixa de diálogo **Adicionar Novo Item**, selecione **Classe** e altere o campo **Nome** para *YoloBoundingBox.cs*. Em seguida, selecione o botão **Adicionar**.

    O arquivo *YoloBoundingBox.cs* é aberto no editor de códigos. Adicione a seguinte instrução `using` à parte superior do *YoloBoundingBox.cs*:

    [!code-csharp [YoloBoundingBoxUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloBoundingBox.cs#L1)]

    Logo acima da definição de classe existente, adicione uma nova definição de classe chamada `BoundingBoxDimensions` que herda da `DimensionsBase` classe para conter as dimensões da respectiva caixa delimitadora.

    [!code-csharp [BoundingBoxDimClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloBoundingBox.cs#L5)]

    Remova a definição de classe `YoloBoundingBox` existente e adicione o seguinte código para a classe `YoloBoundingBox` do arquivo *YoloBoundingBox.cs*:

    [!code-csharp [YoloBoundingBoxClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloBoundingBox.cs#L7-L21)]

    `YoloBoundingBox`tem as seguintes propriedades:

    - `Dimensions` contém as dimensões da caixa delimitadora.
    - `Label` contém a classe de objeto detectada na caixa delimitadora.
    - `Confidence` contém a confiança da classe.
    - `Rect` contém a representação de retângulo das dimensões da caixa delimitadora.
    - `BoxColor` contém a cor associada à respectiva classe usada para desenhar na imagem.

### <a name="create-the-parser"></a>Criar o analisador

Agora que as classes para dimensões e caixas delimitadoras estão criadas, é hora de criar o analisador.

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no diretório *YoloParser* e, em seguida, selecione **Adicionar** > **Novo Item**.
1. Na caixa de diálogo **Adicionar Novo Item**, selecione **Classe** e altere o campo **Nome** para *YoloOutputParser.cs*. Em seguida, selecione o botão **Adicionar**.

    O arquivo *YoloOutputParser.cs* é aberto no editor de códigos. Adicione a seguinte instrução `using` à parte superior do *YoloOutputParser.cs*:

    [!code-csharp [YoloParserUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L1-L4)]

    Dentro da definição de classe `YoloOutputParser` existente, adicione uma classe aninhada que contém as dimensões de cada uma das células da imagem. Adicione o seguinte código para a `CellDimensions` classe que herda da `DimensionsBase` classe na parte superior da definição de `YoloOutputParser` classe.

    [!code-csharp [YoloParserUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L10)]

1. Dentro da `YoloOutputParser` definição de classe, adicione as constantes e os campos a seguir.

    [!code-csharp [ParserVarDefinitions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L12-L21)]

    - `ROW_COUNT` é o número de linhas na grade em que a imagem é dividida.
    - `COL_COUNT` é o número de colunas na grade em que a imagem é dividida.
    - `CHANNEL_COUNT` é o número total de valores contidos em uma célula da grade.
    - `BOXES_PER_CELL` é o número de caixas delimitadoras em uma célula.
    - `BOX_INFO_FEATURE_COUNT` é o número de recursos contidos em uma caixa (x, y, altura, largura, confiança).
    - `CLASS_COUNT` é o número de previsões de classe contidas em cada caixa delimitadora.
    - `CELL_WIDTH` é a largura de uma célula na grade de imagens.
    - `CELL_HEIGHT` é a altura de uma célula na grade de imagens.
    - `channelStride` é a posição inicial da célula atual na grade.

    Quando o modelo faz uma previsão, também conhecida como pontuação, ele divide a imagem de entrada `416px x 416px` em uma grade de células com o tamanho `13 x 13`. O conteúdo de cada célula é `32px x 32px`. Dentro de cada célula, há cinco caixas delimitadoras que contêm cinco recursos (x, y, largura, altura, confiança). Além disso, cada caixa delimitadora contém a probabilidade de cada uma das classes, que nesse caso é 20. Portanto, cada célula contém 125 partes de informações (cinco recursos + 20 probabilidades de classe).

Crie uma lista de âncoras abaixo de `channelStride` para todas as cinco caixas delimitadoras:

[!code-csharp [ParserAnchors](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L23-L26)]

As âncoras são taxas de altura e largura pré-definidas das caixas delimitadoras. A maioria dos objetos ou classes detectadas por um modelo tem taxas semelhantes. Isso é importante quando se trata de criar caixas delimitadoras. Em vez de prever as caixas delimitadoras, o deslocamento das dimensões pré-definidas é calculado, reduzindo a computação necessária para prever a caixa delimitadora. Normalmente, essas taxas de âncora são calculadas com base no conjunto de dados usado. Nesse caso, como o conjunto de conhecimento é conhecido e os valores foram previamente computados, as âncoras podem ser embutidas em código.

Em seguida, defina os rótulos ou as classes que o modelo preverá. Esse modelo prevê 20 classes, que é um subconjunto do número total de classes previstas pelo modelo YOLOv2 original.

Adicione sua lista de rótulos abaixo de `anchors`.

[!code-csharp [ParserLabels](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L28-L34)]

Há cores associadas a cada uma das classes. Atribua suas cores de classe abaixo de `labels`:

[!code-csharp [ParserColors](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L36-L59)]

### <a name="create-helper-functions"></a>Criar funções auxiliares

Há uma série de etapas envolvidas na fase de pós-processamento. Para ajudar com isso, vários métodos auxiliares podem ser empregados.

Os métodos auxiliares usados pelo analisador são:

- `Sigmoid`: aplica a função sigmoide que gera um número entre 0 e 1.
- `Softmax`: normaliza um vetor de entrada em uma distribuição de probabilidade.
- `GetOffset`: mapeia elementos na saída de um modelo unidimensional para a posição correspondente em um tensor `125 x 13 x 13`.
- `ExtractBoundingBoxes`: extrai as dimensões da caixa delimitadora usando o método `GetOffset` da saída do modelo.
- `GetConfidence`extrai o valor de confiança que indica como o modelo é que ele detectou um objeto e usa a `Sigmoid` função para transformá-lo em uma porcentagem.
- `MapBoundingBoxToCell`: usa as dimensões da caixa delimitadora e as mapeia para sua respectiva célula na imagem.
- `ExtractClasses`: extrai as previsões de classe para a caixa delimitadora da saída do modelo usando o método `GetOffset` e as transforma em uma distribuição de probabilidade usando o método `Softmax`.
- `GetTopResult`: seleciona a classe na lista de classes previstas com a maior probabilidade.
- `IntersectionOverUnion`: filtra as caixas delimitadoras sobrepostas com probabilidades inferiores.

Adicione o código para todos os métodos auxiliares abaixo de sua lista de `classColors`.

[!code-csharp [ParserHelperMethods](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L61-L151)]

Depois de definir todos os métodos auxiliares, é hora de usá-los para processar a saída do modelo.

Abaixo do método `IntersectionOverUnion`, crie o método `ParseOutputs` para processar a saída gerada pelo modelo.

```csharp
public IList<YoloBoundingBox> ParseOutputs(float[] yoloModelOutputs, float threshold = .3F)
{

}
```

Crie uma lista para armazenar suas caixas delimitadoras e defina variáveis dentro do método `ParseOutputs`.

[!code-csharp [BBoxList](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L155)]

Cada imagem é dividida em uma grade de células `13 x 13`. Cada célula contém cinco caixas delimitadoras. Abaixo da variável `boxes`, adicione o código para processar todas as caixas em cada uma das células.

```csharp
for (int row = 0; row < ROW_COUNT; row++)
{
    for (int column = 0; column < COL_COUNT; column++)
    {
        for (int box = 0; box < BOXES_PER_CELL; box++)
        {

        }
    }
}
```

Dentro do loop mais interno, calcule a posição inicial da caixa atual na saída de um modelo unidimensional.

[!code-csharp [ChannelDef](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L163)]

Diretamente abaixo disso, use o método `ExtractBoundingBoxDimensions` para obter as dimensões da caixa delimitadora atual.

[!code-csharp [GetBBoxDims](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L165)]

Em seguida, use o método `GetConfidence` para obter a confiança para a caixa delimitadora atual.

[!code-csharp [GetConfidence](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L167)]

Depois disso, use o método `MapBoundingBoxToCell` para mapear a caixa delimitadora atual para a célula atual que está sendo processada.

[!code-csharp [MapBoundingBoxes](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L169)]

Antes de efetuar qualquer processamento adicional, verifique se seu valor de confiança é maior que o limite fornecido. Se não for, processe a próxima caixa delimitadora.

[!code-csharp [CheckThreshold1](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L171-L172)]

Caso contrário, continue processando a saída. A próxima etapa é obter a distribuição de probabilidade das classes previstas para a caixa delimitadora atual usando o método `ExtractClasses`.

[!code-csharp [ExtractClasses](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L174)]

Em seguida, use o método `GetTopResult` para obter o valor e o índice da classe com a maior probabilidade para a caixa atual e computar sua pontuação.

[!code-csharp [GetTopResult](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L176-L177)]

Use o `topScore` para novamente manter somente as caixas delimitadoras que estão acima do limite especificado.

[!code-csharp [CheckThreshold2](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L179-L180)]

Por fim, se a caixa delimitadora atual exceder o limite, crie um objeto `BoundingBox` e adicione-o à lista `boxes`.

[!code-csharp [AddBBoxToList](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L182-L194)]

Depois que todas as células da imagem forem processadas, retorne à lista `boxes`. Adicione a seguinte instrução de retorno abaixo do loop for mais externo no método `ParseOutputs`.

[!code-csharp [ReturnBoxes](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L198)]

### <a name="filter-overlapping-boxes"></a>Filtrar as caixas sobrepostas

Agora que todas as caixas delimitadoras altamente confiáveis foram extraídas da saída do modelo, é necessário fazer a filtragem adicional para remover imagens sobrepostas. Adicione um método chamado `FilterBoundingBoxes` abaixo do método `ParseOutputs`:

```csharp
public IList<YoloBoundingBox> FilterBoundingBoxes(IList<YoloBoundingBox> boxes, int limit, float threshold)
{

}
```

No método `FilterBoundingBoxes`, comece criando uma matriz igual ao tamanho das caixas detectadas e marcando todos os slots como ativos ou prontos para processamento.

[!code-csharp [InitActiveSlots](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L203-L207)]

Em seguida, classifique a lista que contém as caixas delimitadoras em ordem decrescente com base em confiança.

[!code-csharp [SortBoxes](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L209-L211)]

Depois disso, crie uma lista para manter os resultados filtrados.

[!code-csharp [InitFilterResult](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L213)]

Comece a processar cada caixa delimitadora fazendo a iteração em cada uma das caixas delimitadoras.

```csharp
for (int i = 0; i < boxes.Count; i++)
{

}
```

Dentro desse loop for, verifique se a caixa delimitadora atual pode ser processada.

```csharp
if (isActiveBoxes[i])
{

}
```

Se sim, adicione a caixa delimitadora à lista de resultados. Se os resultados excederem o limite especificado de caixas a serem extraídas, interrompa o loop. Adicione o seguinte código dentro da instrução if.

[!code-csharp [AddFirstBBoxToResults](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L219-L223)]

Caso contrário, examine as caixas delimitadoras adjacentes. Adicione o código a seguir abaixo da verificação de limite de caixa.

```csharp
for (var j = i + 1; j < boxes.Count; j++)
{

}
```

Como na primeira caixa, se a caixa adjacente estiver ativa ou pronta para ser processada, use o método `IntersectionOverUnion` para verificar se a primeira caixa e a segunda caixa excedem o limite especificado. Adicione o código a seguir ao seu loop for interno.

[!code-csharp [IOUBBox](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L227-L239)]

Fora do loop for mais interno que verifica caixas delimitadoras adjacentes, veja se há alguma caixa delimitadora restante a ser processada. Se não houver, interrompa o loop for externo.

[!code-csharp [CheckActiveSlots](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L242-L243)]

Por fim, fora do loop for inicial do método `FilterBoundingBoxes`, retorne os resultados:

[!code-csharp [ReturnFilteredBBox](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L246)]

Ótimo! Agora é hora de usar esse código junto com o modelo de pontuação.

## <a name="use-the-model-for-scoring"></a>Usar o modelo para pontuação

Assim como ocorre com o pós-processamento, há algumas etapas nas etapas de pontuação. Para ajudar com isso, adicione uma classe que conterá a lógica de pontuação ao seu projeto.

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **Novo Item**.
1. Na caixa de diálogo **Adicionar Novo Item**, selecione **Classe** e altere o campo **Nome** para *OnnxModelScorer.cs*. Em seguida, selecione o botão **Adicionar**.

    O arquivo *OnnxModelScorer.cs* é aberto no editor de códigos. Adicione a seguinte instrução `using` à parte superior do *OnnxModelScorer.cs*:

    [!code-csharp [ScorerUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L1-L7)]

    Dentro da definição de classe `OnnxModelScorer`, adicione as variáveis a seguir.

    [!code-csharp [InitScorerVars](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L13-L17)]

    Diretamente abaixo disso, crie um construtor para a classe `OnnxModelScorer` que inicializará as variáveis definidas anteriormente.

    [!code-csharp [ScorerCtor](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L19-L24)]

    Depois de criar o construtor, defina algumas structs que contêm variáveis relacionadas às configurações de imagem e modelo. Crie um struct chamado `ImageNetSettings` para conter a altura e a largura esperadas como entrada para o modelo.

    [!code-csharp [ImageNetSettingStruct](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L26-L30)]

    Depois disso, crie outra struct chamada `TinyYoloModelSettings` que contenha os nomes das camadas de entrada e saída do modelo. Para visualizar o nome das camadas de entrada e saída do modelo, você pode usar uma ferramenta como o [Netron.](https://github.com/lutzroeder/netron)

    [!code-csharp [YoloSettingsStruct](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L32-L43)]

    Em seguida, crie o primeiro conjunto de métodos usado para pontuação. Crie o método `LoadModel` dentro de sua classe `OnnxModelScorer`.

    ```csharp
    private ITransformer LoadModel(string modelLocation)
    {

    }
    ```

    No método `LoadModel`, adicione o código a seguir para registrar em log.

    [!code-csharp [LoadModelLog](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L47-L49)]

    Os pipelines do ML.NET precisam saber o esquema de dados para operar quando o [`Fit`](xref:Microsoft.ML.IEstimator%601.Fit*) método é chamado. Nesse caso, um processo semelhante ao treinamento será usado. No entanto, como nenhum treinamento real está acontecendo, é aceitável usar um vazio [`IDataView`](xref:Microsoft.ML.IDataView) . Crie um novo [`IDataView`](xref:Microsoft.ML.IDataView) para o pipeline a partir de uma lista vazia.

    [!code-csharp [LoadEmptyIDV](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L52)]

    Abaixo disso, defina o pipeline. O pipeline consistirá em quatro transformações.

    - [`LoadImages`](xref:Microsoft.ML.ImageEstimatorsCatalog.LoadImages*)carrega a imagem como um bitmap.
    - [`ResizeImages`](xref:Microsoft.ML.ImageEstimatorsCatalog.ResizeImages*)redimensiona a imagem para o tamanho especificado (nesse caso, `416 x 416` ).
    - [`ExtractPixels`](xref:Microsoft.ML.ImageEstimatorsCatalog.ExtractPixels*)altera a representação de pixel da imagem de um bitmap para um vetor numérico.
    - [`ApplyOnnxModel`](xref:Microsoft.ML.OnnxCatalog.ApplyOnnxModel*)carrega o modelo ONNX e o usa para pontuar os dados fornecidos.

    Defina o pipeline no método `LoadModel` abaixo da variável `data`.

    [!code-csharp [ScoringPipeline](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L55-L58)]

    Agora é hora de instanciar o modelo para pontuação. Chame o [`Fit`](xref:Microsoft.ML.IEstimator%601.Fit*) método no pipeline e retorne-o para processamento adicional.

    [!code-csharp [FitReturnModel](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L61-L63)]

Depois que o modelo for carregado, ele poderá ser usado para fazer previsões. Para facilitar esse processo, crie um método chamado `PredictDataUsingModel` abaixo do método `LoadModel`.

```csharp
private IEnumerable<float[]> PredictDataUsingModel(IDataView testData, ITransformer model)
{

}
```

No `PredictDataUsingModel`, adicione o código a seguir para registrar em log.

[!code-csharp [PredictDataLog](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L68-L71)]

Em seguida, use o [`Transform`](xref:Microsoft.ML.ITransformer.Transform*) método para pontuar os dados.

[!code-csharp [ScoreImages](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L73)]

Extraia as probabilidades previstas e retorne-as para processamento adicional.

[!code-csharp [ReturnModelOutput](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L75-L77)]

Agora que ambas as etapas estão configuradas, combine-as em um único método. Abaixo do método `PredictDataUsingModel`, adicione um novo método chamado `Score`.

[!code-csharp [ScoreMethod](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L80-L85)]

Quase lá! Agora é hora de colocar tudo em uso.

## <a name="detect-objects"></a>Detectar objetos

Agora que toda a configuração foi concluída, é hora de detectar alguns objetos. Comece adicionando referências às pontuações e ao analisador na classe *Program.cs*.

[!code-csharp [ReferenceScorerParser](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L8-L9)]

### <a name="score-and-parse-model-outputs"></a>Pontuar e analisar saídas do modelo

Dentro do método `Main` da classe *Program.cs*, adicione uma instrução try-catch.

```csharp
try
{

}
catch (Exception ex)
{
    Console.WriteLine(ex.ToString());
}
```

Dentro do bloco `try`, comece a implementar a lógica de detecção de objetos. Primeiro, carregue os dados em um [`IDataView`](xref:Microsoft.ML.IDataView) .

[!code-csharp [LoadData](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L29-L30)]

Em seguida, crie uma instância de `OnnxModelScorer` e use-a para pontuar os dados carregados.

[!code-csharp [ScoreData](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L33-L36)]

Agora é hora da etapa de pós-processamento. Crie uma instância de `YoloOutputParser` e use-a para processar a saída do modelo.

[!code-csharp [ParsePredictions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L39-L44)]

Após o processamento da saída do modelo, é hora de desenhar as caixas delimitadoras nas imagens.

### <a name="visualize-predictions"></a>Visualizar previsões

Depois que o modelo pontua as imagens e as saídas são processadas, as caixas delimitadoras precisam ser desenhadas na imagem. Para fazer isso, adicione um método chamado `DrawBoundingBox` abaixo do método `GetAbsolutePath` dentro de *Program.cs*.

```csharp
private static void DrawBoundingBox(string inputImageLocation, string outputImageLocation, string imageName, IList<YoloBoundingBox> filteredBoundingBoxes)
{

}
```

Primeiro, carregue a imagem e obtenha as dimensões de altura e largura no método `DrawBoundingBox`.

[!code-csharp [LoadImage](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L78-L81)]

Em seguida, crie um loop for-each para iterar sobre cada uma das caixas delimitadoras detectadas pelo modelo.

```csharp
foreach (var box in filteredBoundingBoxes)
{

}
```

Dentro do loop for-each, obtenha as dimensões da caixa delimitadora.

[!code-csharp [GetBBoxDimensions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L86-L89)]

Como as dimensões da caixa delimitadora correspondem à entrada do modelo de `416 x 416`, dimensione as dimensões da caixa delimitadora para corresponder ao tamanho real da imagem.

[!code-csharp [ScaleImage](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L92-L95)]

Em seguida, defina um modelo para o texto que será exibido acima de cada caixa delimitadora. O texto conterá a classe do objeto dentro da respectiva caixa delimitadora, bem como a confiança.

[!code-csharp [DefineBBoxText](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L98)]

Para desenhar na imagem, converta-a em um [`Graphics`](xref:System.Drawing.Graphics) objeto.

```csharp
using (Graphics thumbnailGraphic = Graphics.FromImage(image))
{

}
```

Dentro do `using` bloco de código, ajuste as configurações de objeto do gráfico [`Graphics`](xref:System.Drawing.Graphics) .

[!code-csharp [TuneGraphicSettings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L102-L104)]

Abaixo disso, defina as opções de fonte e cor para o texto e a caixa delimitadora.

[!code-csharp [SetColorOptions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L106-L114)]

Crie e preencha um retângulo acima da caixa delimitadora para conter o texto usando o [`FillRectangle`](xref:System.Drawing.Graphics.FillRectangle*) método. Isso ajudará a comparar o texto e a melhorar a legibilidade.

[!code-csharp [DrawTextBackground](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L117)]

Em seguida, desenhe o texto e a caixa delimitadora na imagem usando os [`DrawString`](xref:System.Drawing.Graphics.DrawString*) [`DrawRectangle`](xref:System.Drawing.Graphics.DrawRectangle*) métodos e.

[!code-csharp [DrawClassAndBBox](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L118-L121)]

Fora do loop for-each, adicione o código para salvar as imagens no `outputDirectory`.

[!code-csharp [SaveImage](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L125-L130)]

Para obter comentários adicionais de que o aplicativo está fazendo previsões conforme esperado em runtime, adicione um método chamado `LogDetectedObjects` abaixo do método `DrawBoundingBox` no arquivo *Program.cs* para gerar os objetos detectados no console.

[!code-csharp [LogOutputs](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L133-L143)]

Agora que você tem métodos auxiliares para criar comentários visuais com base nas previsões, adicione um loop for para iterar em cada uma das imagens pontuadas.

```csharp
for (var i = 0; i < images.Count(); i++)
{

}
```

Dentro do loop for, obtenha o nome do arquivo de imagem e as caixas delimitadoras associadas a ele.

[!code-csharp [GetImageFileName](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L49-L50)]

Abaixo disso, use o método `DrawBoundingBox` para desenhar as caixas delimitadoras na imagem.

[!code-csharp [DrawBBoxes](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L52)]

Por fim, use o método `LogDetectedObjects` para gerar previsões no console.

[!code-csharp [LogPredictionsOutput](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L54)]

Após a instrução try-catch, adicione a lógica complementar para indicar que o processo terminou a execução.

[!code-csharp [EndProcessLog](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L62-L63)]

É isso!

## <a name="results"></a>Resultados

Depois de seguir as etapas anteriores, execute o aplicativo de console (Ctrl+F5). O resultado deverá ser semelhante à seguinte saída. Você poderá ver avisos ou mensagens de processamento, mas essas mensagens foram removidas dos resultados a seguir para maior clareza.

```console
=====Identify the objects in the images=====

.....The objects in the image image1.jpg are detected as below....
car and its Confidence score: 0.9697262
car and its Confidence score: 0.6674225
person and its Confidence score: 0.5226039
car and its Confidence score: 0.5224892
car and its Confidence score: 0.4675332

.....The objects in the image image2.jpg are detected as below....
cat and its Confidence score: 0.6461141
cat and its Confidence score: 0.6400049

.....The objects in the image image3.jpg are detected as below....
chair and its Confidence score: 0.840578
chair and its Confidence score: 0.796363
diningtable and its Confidence score: 0.6056048
diningtable and its Confidence score: 0.3737402

.....The objects in the image image4.jpg are detected as below....
dog and its Confidence score: 0.7608147
person and its Confidence score: 0.6321323
dog and its Confidence score: 0.5967442
person and its Confidence score: 0.5730394
person and its Confidence score: 0.5551759

========= End of Process..Hit any Key ========
```

Para ver as imagens com as caixas delimitadoras, navegue até o diretório `assets/images/output/`. Abaixo está um exemplo de uma das imagens processadas.

![Exemplo de imagem processada de uma sala de jantar](./media/object-detection-onnx/dinning-room-table-chairs.png)

Parabéns! Você criou com êxito um modelo de machine learning para detecção de objetos reutilizando um modelo `ONNX` pré-treinado no ML.NET.

Você pode encontrar o código-fonte deste tutorial no repositório [dotnet/MachineLearning-Samples](https://github.com/dotnet/machinelearning-samples/tree/master/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx) .

Neste tutorial, você aprendeu a:
> [!div class="checklist"]
>
> - Compreender o problema
> - Saiba o que é o ONNX e como ele funciona com o ML.NET
> - Entender o modelo
> - Reutilizar o modelo pré-treinado
> - Detectar objetos com um modelo carregado

Confira o repositório GitHub de exemplos de Machine Learning para explorar um exemplo de detecção de objetos expandido.
> [!div class="nextstepaction"]
> [dotnet/machinelearning-samples GitHub repository](https://github.com/dotnet/machinelearning-samples/tree/master/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx)
