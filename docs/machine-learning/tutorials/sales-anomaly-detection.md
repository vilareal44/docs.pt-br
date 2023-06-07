---
title: 'Tutorial: detectar anomalias em vendas de produtos'
description: Saiba como criar um aplicativo de detecção de anomalias para dados de vendas do produto. Este tutorial cria um aplicativo de console .NET Core usando C# no Visual Studio 2019.
ms.date: 06/30/2020
ms.topic: tutorial
ms.custom: mvc, title-hack-0612
ms.openlocfilehash: cf61f197e4befebdbb1fbf2ca4cbcdc61c48780a
ms.sourcegitcommit: 97ce5363efa88179dd76e09de0103a500ca9b659
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2020
ms.locfileid: "86281661"
---
# <a name="tutorial-detect-anomalies-in-product-sales-with-mlnet"></a>Tutorial: detectar anomalias em vendas de produtos com o ML.NET

Saiba como criar um aplicativo de detecção de anomalias para dados de vendas do produto. Este tutorial cria um aplicativo de console .NET Core usando C# no Visual Studio.

Neste tutorial, você aprenderá como:
> [!div class="checklist"]
>
> * Carregar os dados
> * Criar uma transformação para detecção de anomalias de pico
> * Detectar anomalias de pico com a transformação
> * Criar uma transformação para detecção de anomalias de ponto de alteração
> * Detectar anomalias de ponto de alteração com a transformação

Você pode encontrar o código-fonte para este tutorial no repositório [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/ProductSalesAnomalyDetection).

## <a name="prerequisites"></a>Pré-requisitos

* [Visual Studio 2017 versão 15,6 ou posterior](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) com a carga de trabalho "desenvolvimento de plataforma cruzada do .NET Core" instalada.

* [O conjunto de dados product-sales.csv](https://raw.githubusercontent.com/dotnet/machinelearning-samples/master/samples/csharp/getting-started/AnomalyDetection_Sales/SpikeDetection/Data/product-sales.csv)

>[!NOTE]
> Os dados de formato em `product-sales.csv` baseiam-se no conjunto de dados "Vendas de xampu no período de três anos" originalmente adquiridos do DataMarket e fornecidos pela TSDL (Biblioteca de Dados de Série Temporal), criado por Rob Hyndman.
> Conjunto de dados de "Vendas de xampu no período de três anos" licenciado sob a Licença Aberta Padrão do DataMarket.

## <a name="create-a-console-application"></a>Criar um aplicativo de console

1. Crie um **Aplicativo de Console do .NET Core** chamado "ProductSalesAnomalyDetection".

2. Crie um diretório chamado *dados* em seu projeto para salvar os arquivos do conjunto de dados.

3. Instalar o **Pacote NuGet Microsoft.ML**:

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    No Gerenciador de Soluções, clique com o botão direito do mouse no seu projeto e selecione **Gerenciar Pacotes NuGet**. Escolha "nuget.org" como a origem do pacote, selecione a guia procurar, procure **Microsoft.ml** e selecione o botão **instalar** . Selecione o botão **OK** na caixa de diálogo **Visualizar Alterações** e selecione o botão **Aceito** na caixa de diálogo **Aceitação da Licença**, se concordar com o termos de licença para os pacotes listados. Repita essas etapas para **Microsoft. ml. timeseries**.

4. Adicione as seguintes instruções `using` à parte superior do arquivo *Program.cs*:

    [!code-csharp[AddUsings](./snippets/sales-anomaly-detection/csharp/Program.cs#AddUsings "Add necessary usings")]

### <a name="download-your-data"></a>Baixar seus dados

1. Baixe o conjunto de dados e salve-o na pasta *Data* criada anteriormente:

   * Clique com o botão direito do mouse em [*product-sales.csv*](https://raw.githubusercontent.com/dotnet/machinelearning-samples/master/samples/csharp/getting-started/AnomalyDetection_Sales/SpikeDetection/Data/product-sales.csv) e selecione "Salvar Link (ou destino) como…"

     Salve o arquivo \*.csv na pasta *Data* ou, depois de salvá-lo em outro lugar, mova o arquivo \*.csv para a pasta *Data*.

2. No Gerenciador de Soluções, clique com o botão direito do mouse no arquivo \*.csv e selecione **Propriedades**. Em **avançado**, altere o valor de **copiar para diretório de saída** para **copiar se mais recente**.

A tabela a seguir é uma visualização de dados do seu arquivo \*.csv:

|Mês  |ProductSales |
|-------|-------------|
|1-Jan  |    271      |
|2-Jan  |    150.9    |
|.....  |    .....    |
|1-Feb  |    199.3    |
|.....  |    .....    |

### <a name="create-classes-and-define-paths"></a>Criar classes e definir demarcadores

Em seguida, defina suas estruturas de dados de classe de entrada e de previsão.

Adicione uma nova classe ao seu projeto:

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto e, em seguida, selecione **Adicionar > Novo Item**.

2. Na caixa de diálogo **Adicionar Novo Item**, selecione **Classe** e altere o campo **Nome** para *ProductSalesData.cs*. Em seguida, selecione o botão **Adicionar**.

   O arquivo *ProductSalesData.cs* é aberto no editor de códigos.

3. Adicione a seguinte instrução `using` à parte superior do *ProductSalesData.cs*:

   ```csharp
   using Microsoft.ML.Data;
   ```

4. Remova a definição de classe existente e adicione o seguinte código, que tem duas classes, `ProductSalesData` e `ProductSalesPrediction`, ao arquivo *ProductSalesData.cs*:

    [!code-csharp[DeclareTypes](./snippets/sales-anomaly-detection/csharp/ProductSalesData.cs#DeclareTypes "Declare data record types")]

    `ProductSalesData` especifica uma classe de dados de entrada. O atributo [LoadColumn](xref:Microsoft.ML.Data.LoadColumnAttribute.%23ctor%28System.Int32%29) especifica quais colunas (por índice de coluna) no conjunto de dados devem ser carregadas.

    `ProductSalesPrediction` especifica a classe de dados de previsão. Para detecção de anomalias, a previsão consiste em um alerta para indicar se há uma anomalia, uma pontuação bruta e um valor p. Quanto mais próximo o valor p for de 0, maior será a probabilidade de que uma anomalia tenha ocorrido.

5. Crie dois campos globais para armazenar o caminho do arquivo de conjunto de dados baixado recentemente e o caminho do arquivo de modelo salvo:

    * `_dataPath` tem o demarcador para o conjunto de dados usado para treinar o modelo.
    * O `_docsize` tem o número de registros no arquivo de conjunto de dados. Você usará `_docSize` para calcular `pvalueHistoryLength`.

6. Adicione o seguinte código à linha logo acima do método `Main` para especificar estes caminhos:

    [!code-csharp[Declare global variables](./snippets/sales-anomaly-detection/csharp/Program.cs#DeclareGlobalVariables "Declare global variables")]

### <a name="initialize-variables-in-main"></a>Inicializar variáveis em Main

1. Substitua a linha `Console.WriteLine("Hello World!")` no método `Main` pelo seguinte código para declarar e inicializar a variável `mlContext`:

    [!code-csharp[CreateMLContext](./snippets/sales-anomaly-detection/csharp/Program.cs#CreateMLContext "Create the ML Context")]

    A [classe MLContext](xref:Microsoft.ML.MLContext) é um ponto de partida para todas as operações do ML.NET e a inicialização do `mlContext` cria um ambiente do ML.NET que pode ser compartilhado entre os objetos de fluxo de trabalho da criação de modelo. Ele é semelhante, conceitualmente, a `DBContext` no Entity Framework.

### <a name="load-the-data"></a>Carregar os dados

Os dados do ML.NET são representados como uma [classe IDataView](xref:Microsoft.ML.IDataView). `IDataView` é uma maneira flexível e eficiente de descrever dados tabulares (numéricos e texto). Os dados podem ser carregados de um arquivo de texto ou de outras fontes (por exemplo, banco de dados SQL ou arquivos de log) para um objeto `IDataView`.

1. Adicione o seguinte código como a próxima linha do método `Main()`:

    [!code-csharp[LoadData](./snippets/sales-anomaly-detection/csharp/Program.cs#LoadData "loading dataset")]

    O [LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29) define o esquema de dados e lê o arquivo. Ele usa as variáveis de caminho de dados e retorna uma `IDataView`.

## <a name="time-series-anomaly-detection"></a>Detecção de anomalias de série temporal

A detecção de anomalias sinaliza comportamentos ou eventos incomuns ou inesperados. Ela dá dicas de em que local procurar problemas e ajuda a responder à pergunta "Isso é estranho?".

![Exemplo da detecção de anomalias "é estranha".](./media/sales-anomaly-detection/time-series-anomaly-detection.png)

A detecção de anomalias é o processo de detectar exceções de dados de série temporal; pontos em uma série temporal de entrada em que o comportamento não é o esperado ou "estranho".

A detecção de anomalias pode ser útil de várias maneiras. Por exemplo:

Se você tiver um carro, talvez queira saber: Este medidor de óleo está lendo normal ou ter um vazamento?
Se você estiver monitorando o consumo de energia, você desejaria saber: há uma interrupção?

Há dois tipos de anomalias de série temporal que podem ser detectados:

* **Picos** indicam intermitências temporárias de comportamentos anormais no sistema.

* **Pontos de alteração** indicam o início de alterações persistentes ao longo do tempo no sistema.

No ML.NET, os algoritmos de Detecção de Pico IID ou Detecção de Ponto de Alteração IID são adequados para [conjuntos de dados independentes e distribuídos de forma idêntica](https://en.wikipedia.org/wiki/Independent_and_identically_distributed_random_variables).

Ao contrário dos modelos nos outros tutoriais, as transformações do detector de anomalias de série temporal operam diretamente nos dados de entrada. O método `IEstimator.Fit()` não precisa de dados de treinamento para produzir a transformação. No entanto, ele precisa do esquema de dados, que é fornecido por uma exibição de dados gerada de uma lista vazia de `ProductSalesData`.

Você vai analisar os mesmos dados de vendas do produto para detectar picos e pontos de alteração. O processo de criar e treinar o modelo é o mesmo para detecção de pico e detecção de ponto de alteração; a principal diferença é o algoritmo de detecção específico usado.

## <a name="spike-detection"></a>Detecção de pico

A meta da detecção de pico é identificar picos repentinos, mas temporários, que diferem significativamente da maioria dos valores de dados de série temporal. É importante detectar esses itens raros, eventos ou observações suspeitos de maneira oportuna para que sejam minimizados. A abordagem a seguir pode ser usada para detectar uma variedade de anomalias, como: interrupções, ataques cibernéticos ou conteúdo da Web viral. A imagem a seguir é um exemplo de picos em um conjunto de dados de série temporal:

![Captura de tela que mostra duas detecções de pico.](./media/sales-anomaly-detection/two-spike-detections.png)

### <a name="add-the-createemptydataview-method"></a>Adicionar o método CreateEmptyDataView()

Adicione o seguinte método a `Program.cs`:

[!code-csharp[CreateEmptyDataView](./snippets/sales-anomaly-detection/csharp/Program.cs#CreateEmptyDataView)]

O `CreateEmptyDataView()` produz um objeto de exibição de dados vazio com o esquema correto a ser usado como entrada para o método `IEstimator.Fit()`.

### <a name="create-the-detectspike-method"></a>Crie o método DetectSpike()

O método `DetectSpike()`:

* Cria a transformação do estimador.
* Detecta picos com base em dados históricos de vendas.
* Exibe os resultados.

1. Crie o método `DetectSpike()`, logo após o método `Main()`, usando o seguinte código:

    ```csharp
    static void DetectSpike(MLContext mlContext, int docSize, IDataView productSales)
    {

    }
    ```

1. Use [IidSpikeEstimator](xref:Microsoft.ML.Transforms.TimeSeries.IidSpikeEstimator) para treinar o modelo para a detecção de pico. Adicione-o ao método `DetectSpike()` com o seguinte código:

    [!code-csharp[AddSpikeTrainer](./snippets/sales-anomaly-detection/csharp/Program.cs#AddSpikeTrainer)]

1. Crie a transformação detecção de pico adicionando o seguinte como a próxima linha de código no método `DetectSpike()`:

    [!code-csharp[TrainModel1](./snippets/sales-anomaly-detection/csharp/Program.cs#TrainModel1)]

1. Adicione a seguinte linha de código para transformar os dados `productSales` como a próxima linha no método `DetectSpike()`:

    [!code-csharp[TransformData1](./snippets/sales-anomaly-detection/csharp/Program.cs#TransformData1)]

    O código anterior usa o método [Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) para fazer previsões para várias linhas de entrada de um conjunto de dados.

1. Converta seu `transformedData` em um tipo fortemente tipado `IEnumerable` para facilitar a exibição usando o método [createenumerable ()](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) com o seguinte código:

    [!code-csharp[CreateEnumerable1](./snippets/sales-anomaly-detection/csharp/Program.cs#CreateEnumerable1)]

1. Crie uma linha de cabeçalho de exibição usando o seguinte código <xref:System.Console.WriteLine?displayProperty=nameWithType>:

    [!code-csharp[DisplayHeader1](./snippets/sales-anomaly-detection/csharp/Program.cs#DisplayHeader1)]

    Você exibirá as informações a seguir nos resultados da detecção de pico:

    * `Alert` indica um alerta de pico para um ponto de dados específico.
    * `Score` é o valor `ProductSales` para um dado ponto de dados no conjunto de dados.
    * `P-Value` O "P" significa probabilidade. Quanto mais próximo o valor p for de 0, maior será a probabilidade de que o ponto de dados seja uma anomalia.

1. Use o código a seguir para iterar por meio do `predictions` `IEnumerable` e exibir os resultados:

    [!code-csharp[DisplayResults1](./snippets/sales-anomaly-detection/csharp/Program.cs#DisplayResults1)]

1. Adicione a chamada ao método `DetectSpike()` no método `Main()`:

    [!code-csharp[CallDetectSpike](./snippets/sales-anomaly-detection/csharp/Program.cs#CallDetectSpike)]

## <a name="spike-detection-results"></a>Resultados da detecção de pico

Seus resultados devem ser semelhantes aos seguintes. Durante o processamento, as mensagens são exibidas. Você pode ver avisos ou mensagens de processamento. Algumas das mensagens foram removidas dos resultados a seguir para fins de clareza.

```console
Detect temporary changes in pattern
=============== Training the model ===============
=============== End of training process ===============
Alert   Score   P-Value
0       271.00  0.50
0       150.90  0.00
0       188.10  0.41
0       124.30  0.13
0       185.30  0.47
0       173.50  0.47
0       236.80  0.19
0       229.50  0.27
0       197.80  0.48
0       127.90  0.13
1       341.50  0.00 <-- Spike detected
0       190.90  0.48
0       199.30  0.48
0       154.50  0.24
0       215.10  0.42
0       278.30  0.19
0       196.40  0.43
0       292.00  0.17
0       231.00  0.45
0       308.60  0.18
0       294.90  0.19
1       426.60  0.00 <-- Spike detected
0       269.50  0.47
0       347.30  0.21
0       344.70  0.27
0       445.40  0.06
0       320.90  0.49
0       444.30  0.12
0       406.30  0.29
0       442.40  0.21
1       580.50  0.00 <-- Spike detected
0       412.60  0.45
1       687.00  0.01 <-- Spike detected
0       480.30  0.40
0       586.30  0.20
0       651.90  0.14
```

## <a name="change-point-detection"></a>Detecção de ponto de alteração

`Change points` são alterações persistentes em uma distribuição de valores de fluxo de evento de série temporal, como alterações de nível e tendências. Essas alterações persistentes duram muito mais que `spikes` e podem indicar eventos catastróficos. `Change points` normalmente não estão visíveis a olho nu, mas podem ser detectados em seus dados usando abordagens como o método a seguir.  A imagem a seguir é um exemplo de uma detecção de ponto de alteração:

![Captura de tela que mostra uma detecção de ponto de alteração.](./media/sales-anomaly-detection/change-point-detection.png)

### <a name="create-the-detectchangepoint-method"></a>Crie o método DetectChangepoint()

O método `DetectChangepoint()` executa as seguintes tarefas:

* Cria a transformação do estimador.
* Detecta pontos de alteração com base em dados históricos de vendas.
* Exibe os resultados.

1. Crie o método `DetectChangepoint()`, logo após o método `Main()`, usando o seguinte código:

    ```csharp
    static void DetectChangepoint(MLContext mlContext, int docSize, IDataView productSales)
    {

    }
    ```

1. Crie o [iidChangePointEstimator](xref:Microsoft.ML.Transforms.TimeSeries.IidChangePointEstimator) no método `DetectChangepoint()` com o seguinte código:

    [!code-csharp[AddChangepointTrainer](./snippets/sales-anomaly-detection/csharp/Program.cs#AddChangepointTrainer)]

1. Como você fez anteriormente, crie a transformação do estimador adicionando a seguinte linha de código ao método `DetectChangePoint()`:

    [!code-csharp[TrainModel2](./snippets/sales-anomaly-detection/csharp/Program.cs#TrainModel2)]

1. Use o método `Transform()` para transformar os dados adicionando o seguinte código a `DetectChangePoint()`:

    [!code-csharp[TransformData2](./snippets/sales-anomaly-detection/csharp/Program.cs#TransformData2)]

1. Como você fez anteriormente, converta seu `transformedData` em um tipo fortemente tipado `IEnumerable` para facilitar a exibição usando o `CreateEnumerable()` método com o seguinte código:

    [!code-csharp[CreateEnumerable2](./snippets/sales-anomaly-detection/csharp/Program.cs#CreateEnumerable2)]

1. Crie um cabeçalho de exibição com o código a seguir como a próxima linha no método `DetectChangePoint()`:

    [!code-csharp[DisplayHeader2](./snippets/sales-anomaly-detection/csharp/Program.cs#DisplayHeader2)]

    Você exibirá as informações a seguir nos resultados da detecção de ponto de alteração:

    * `Alert` indica um alerta de ponto de alteração para um ponto de dados específico.
    * `Score` é o valor `ProductSales` para um dado ponto de dados no conjunto de dados.
    * `P-Value` O "P" significa probabilidade. Quanto mais próximo o valor P for de 0, maior será a probabilidade de que o ponto de dados seja uma anomalia.
    * `Martingale value` é usado para identificar o quão "estranho" um ponto de dados é com base na sequência de valores de P.

1. Itere por meio do `predictions` `IEnumerable` e exiba os resultados com o seguinte código:

    [!code-csharp[DisplayResults2](./snippets/sales-anomaly-detection/csharp/Program.cs#DisplayResults2)]

1. Adicione a seguinte chamada ao método `DetectChangepoint()` no método `Main()`:

    [!code-csharp[CallDetectChangepoint](./snippets/sales-anomaly-detection/csharp/Program.cs#CallDetectChangepoint)]

## <a name="change-point-detection-results"></a>Resultados da detecção de ponto de alteração

Seus resultados devem ser semelhantes aos seguintes. Durante o processamento, as mensagens são exibidas. Você pode ver avisos ou mensagens de processamento. Algumas mensagens foram removidas dos resultados a seguir para fins de clareza.

```console
Detect Persistent changes in pattern
=============== Training the model Using Change Point Detection Algorithm===============
=============== End of training process ===============
Alert   Score   P-Value Martingale value
0       271.00  0.50    0.00
0       150.90  0.00    2.33
0       188.10  0.41    2.80
0       124.30  0.13    9.16
0       185.30  0.47    9.77
0       173.50  0.47    10.41
0       236.80  0.19    24.46
0       229.50  0.27    42.38
1       197.80  0.48    44.23 <-- alert is on, predicted changepoint
0       127.90  0.13    145.25
0       341.50  0.00    0.01
0       190.90  0.48    0.01
0       199.30  0.48    0.00
0       154.50  0.24    0.00
0       215.10  0.42    0.00
0       278.30  0.19    0.00
0       196.40  0.43    0.00
0       292.00  0.17    0.01
0       231.00  0.45    0.00
0       308.60  0.18    0.00
0       294.90  0.19    0.00
0       426.60  0.00    0.00
0       269.50  0.47    0.00
0       347.30  0.21    0.00
0       344.70  0.27    0.00
0       445.40  0.06    0.02
0       320.90  0.49    0.01
0       444.30  0.12    0.02
0       406.30  0.29    0.01
0       442.40  0.21    0.01
0       580.50  0.00    0.01
0       412.60  0.45    0.01
0       687.00  0.01    0.12
0       480.30  0.40    0.08
0       586.30  0.20    0.03
0       651.90  0.14    0.09
```

Parabéns! Você agora criou com êxito os modelos de machine learning para detectar anomalias de pontos de alteração e picos nos dados de vendas.

Você pode encontrar o código-fonte para este tutorial no repositório [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/ProductSalesAnomalyDetection).

Neste tutorial, você aprendeu a:
> [!div class="checklist"]
>
> * Carregar os dados
> * Treinar o modelo para detecção de anomalias de pico
> * Detectar anomalias de pico com o modelo treinado
> * Treinar o modelo para detecção de anomalias de ponto de alteração
> * Detectar anomalias de ponto de alteração com o modo treinado

## <a name="next-steps"></a>Próximas etapas

Confira o repositório do GitHub de exemplos de Machine Learning para explorar um exemplo de Detecção de Anomalias de Consumo de Energia.
> [!div class="nextstepaction"]
> [dotnet/machinelearning-samples GitHub repository](https://github.com/dotnet/machinelearning-samples/tree/master/samples/csharp/getting-started/AnomalyDetection_PowerMeterReadings)
