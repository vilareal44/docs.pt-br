---
title: 'Tutorial: Previsão de pressão de bicicletas – série temporal'
description: Este tutorial mostra como prever a demanda por um serviço de aluguel de bicicletas usando análise de série temporal monovariável e ML.NET.
ms.date: 06/30/2020
ms.topic: tutorial
ms.custom: mvc
ms.author: luquinta
author: luisquintanilla
ms.openlocfilehash: d93bdee8d5a057be0f405fe4334d7edbdc0649ec
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174400"
---
# <a name="tutorial-forecast-bike-rental-service-demand-with-time-series-analysis-and-mlnet"></a>Tutorial: prever a demanda de serviço de aluguel de bicicletas com análise de série temporal e ML.NET

Saiba como prever a demanda por um serviço de aluguel de bicicletas usando a análise de série temporal monovariável nos dados armazenados em um banco de dados SQL Server com ML.NET.

Neste tutorial, você aprenderá como:
> [!div class="checklist"]
>
> * Compreender o problema
> * Carregar dados de um banco
> * Criar um modelo de previsão
> * Avaliar modelo de previsão
> * Salvar um modelo de previsão
> * Usar um modelo de previsão

## <a name="prerequisites"></a>Pré-requisitos

- [Visual studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) ou posterior ou visual Studio 2017 versão 15,6 ou posterior com a carga de trabalho "desenvolvimento de plataforma cruzada do .NET Core" instalada.

## <a name="time-series-forecasting-sample-overview"></a>Visão geral de exemplo de previsão de série temporal

Este exemplo é um **aplicativo de console C# .NET Core** que prevê a demanda por locações de bicicletas usando um algoritmo de análise de série temporal monovariável conhecido como análise de espectro singular. O código para este exemplo pode ser encontrado no repositório [dotnet/MachineLearning-Samples](https://github.com/dotnet/machinelearning-samples/tree/master/samples/csharp/getting-started/Forecasting_BikeSharingDemand) no github.

## <a name="understand-the-problem"></a>Compreender o problema

Para executar uma operação eficiente, o gerenciamento de inventário desempenha um papel fundamental. Ter muitos produtos em estoque significa que produtos não vendidos nos bastidores não geram receita. Ter um produto muito pequeno leva a uma perda de vendas e clientes comprando de concorrentes. Portanto, a questão constante é, qual é a quantidade ideal de inventário a ser mantido disponível? A análise de série temporal ajuda a fornecer uma resposta a essas perguntas, examinando os dados históricos, identificando padrões e usando essas informações para prever valores em algum momento no futuro.

A técnica para analisar dados usados neste tutorial é a análise de série temporal monovariável. A análise da série temporal monovariável examina uma única observação numérica durante um período de tempo em intervalos específicos, como vendas mensais.

O algoritmo usado neste tutorial é [análise de espectro singular (SSA)](http://ssa.cf.ac.uk/zhigljavsky/pdfs/SSA/SSA_encyclopedia.pdf). O SSA funciona decompondo uma série temporal em um conjunto de componentes principais. Esses componentes podem ser interpretados como as partes de um sinal que correspondem a tendências, ruídos, sazonalidade e muitos outros fatores. Em seguida, esses componentes são reconstruídos e usados para prever valores por algum tempo no futuro.

## <a name="create-console-application"></a>Criar um aplicativo de console

1. Crie um novo **aplicativo de console C# .NET Core** chamado "BikeDemandForecasting".
1. Instalar o pacote NuGet da versão **Microsoft.ml**

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    1. No Gerenciador de Soluções, clique com o botão direito do mouse no seu projeto e selecione **Gerenciar Pacotes NuGet**.
    1. Escolha "nuget.org" como a origem do pacote, selecione a guia **procurar** , procure **Microsoft.ml**.
    1. Marque a caixa de seleção **incluir pré-lançamento** .
    1. Selecione o botão **Instalar**.
    1. Selecione o botão **OK** na caixa de diálogo **Visualizar Alterações** e selecione o botão **Aceito** na caixa de diálogo Aceitação da Licença, se concordar com o termos de licença para os pacotes listados.
    1. Repita essas etapas para **System. Data. SqlClient** e **Microsoft. ml. timeseries**.

### <a name="prepare-and-understand-the-data"></a>Preparar e compreender os dados

1. Crie um diretório chamado *Data*.
1. Baixe o arquivo de banco de dados [ *DailyDemand. MDF* ](https://github.com/dotnet/machinelearning-samples/raw/master/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Data/DailyDemand.mdf) e salve-o no diretório *Data* .

> [!NOTE]
> Os dados usados neste tutorial são provenientes do [DataSet de compartilhamento de bicicletas de UCI](http://archive.ics.uci.edu/ml/datasets/bike+sharing+dataset). Fanaee-T, Hadi e gama, Joao, ' rotulamento de evento combinando detectores de Ensemble e conhecimento em segundo plano ', progresso em inteligência artificial (2013): PP. 1-15, Springer Berlim Heidelberg, [link da Web](https://link.springer.com/article/10.1007%2Fs13748-013-0040-3).

O conjunto de conteúdo original contém várias colunas correspondentes ao sazonalidade e ao clima. Para resumir e como o algoritmo usado neste tutorial requer apenas os valores de uma única coluna numérica, o conjunto de um original foi condensado para incluir apenas as seguintes colunas:

- **dteday**: a data da observação.
- **year**: o ano codificado da observação (0 = 2011, 1 = 2012).
- **CNT**: o número total de locações de bicicletas para esse dia.

O DataSet original é mapeado para uma tabela de banco de dados com o esquema a seguir em um banco de dados SQL Server.

```SQL
CREATE TABLE [Rentals] (
    [RentalDate] DATE NOT NULL,
    [Year] INT NOT NULL,
    [TotalRentals] INT NOT NULL
);
```

Veja a seguir um exemplo dos dados:

| RentalDate | Ano | TotalRentals |
| --- | --- | --- |
|1/1/2011|0|985|
|1/2/2011|0|801|
|1/3/2011|0|1349|

### <a name="create-input-and-output-classes"></a>Criar classes de entrada e saída

1. Abra o arquivo *Program.cs* e substitua as `using` instruções existentes pelo seguinte:

    [!code-csharp [ProgramUsings](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L1-L8)]

1. Crie a classe `ModelInput`. Abaixo da `Program` classe, adicione o código a seguir.

    [!code-csharp [ModelInputClass](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L120-L127)]

    A `ModelInput` classe contém as seguintes colunas:

    - **RentalDate**: a data da observação.
    - **Year**: o ano codificado da observação (0 = 2011, 1 = 2012).
    - **TotalRentals**: o número total de locações de bicicletas para esse dia.

1. Crie `ModelOutput` a classe abaixo da `ModelInput` classe recém-criada.

    [!code-csharp [ModelOutputClass](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L129-L136)]

    A `ModelOutput` classe contém as seguintes colunas:

    - **ForecastedRentals**: os valores previstos para o período previsto.
    - **LowerBoundRentals**: os valores mínimos previstos para o período previsto.
    - **UpperBoundRentals**: os valores máximos previstos para o período previsto.

### <a name="define-paths-and-initialize-variables"></a>Definir caminhos e inicializar variáveis

1. Dentro do `Main` método, defina variáveis para armazenar o local dos dados, a cadeia de conexão e onde salvar o modelo treinado.

    [!code-csharp [DefinePaths](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L16-L19)]

1. Inicialize a `mlContext` variável com uma nova instância do [`MLContext`](xref:Microsoft.ML.MLContext) adicionando a linha a seguir ao `Main` método.

    [!code-csharp [MLContext](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L21)]

    A [`MLContext`](xref:Microsoft.ML.MLContext) classe é um ponto de partida para todas as operações de ml.net e a inicialização de mlContext cria um novo ambiente de ml.NET que pode ser compartilhado entre os objetos de fluxo de trabalho de criação de modelo. Ele é semelhante, conceitualmente, a `DBContext` no Entity Framework.

## <a name="load-the-data"></a>Carregar os dados

1. Create `DatabaseLoader` que carrega registros do tipo `ModelInput` .

    [!code-csharp [CreateDBLoader](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L23)]

1. Defina a consulta para carregar os dados do banco de dado.

    [!code-csharp [DefineSQLQuery](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L25)]

    Os algoritmos ML.NET esperam que os dados sejam do tipo [`Single`](xref:System.Single) . Portanto, os valores numéricos provenientes do banco de dados que não são do tipo [`Real`](xref:System.Data.SqlDbType) , um valor de ponto flutuante de precisão simples, precisam ser convertidos em [`Real`](xref:System.Data.SqlDbType) .

    As `Year` `TotalRental` colunas e são ambos tipos de inteiro no banco de dados. Usando a `CAST` função interna, ambas são convertidas para `Real` .

1. Crie um `DatabaseSource` para se conectar ao banco de dados e executar a consulta.

    [!code-csharp [CreateDBSource](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L27-L29)]

1. Carregar os dados em um `IDataView`.

    [!code-csharp [LoadData](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L31)]

1. O DataSet contém dois anos de dados. Somente os dados do primeiro ano são usados para treinamento, o segundo ano é mantido para comparar os valores reais com a previsão produzida pelo modelo. Filtre os dados usando a [`FilterRowsByColumn`](xref:Microsoft.ML.DataOperationsCatalog.FilterRowsByColumn*) transformação.

    [!code-csharp [SplitData](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L33-L34)]

    No primeiro ano, somente os valores na `Year` coluna menor que 1 são selecionados definindo o `upperBound` parâmetro como 1. Por outro lado, no segundo ano, valores maiores ou iguais a 1 são selecionados definindo o `lowerBound` parâmetro como 1.

## <a name="define-time-series-analysis-pipeline"></a>Definir pipeline de análise de série temporal

1. Defina um pipeline que usa o [SsaForecastingEstimator](xref:Microsoft.ML.Transforms.TimeSeries.SsaForecastingEstimator) para prever valores em um conjunto de tempo de série temporal.

    [!code-csharp [DefinePipeline](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L36-L45)]

    O `forecastingPipeline` leva 365 pontos de dados para o primeiro ano e os exemplos ou divide o conjunto de tempo da série temporal em intervalos de 30 dias (mensais), conforme especificado pelo `seriesLength` parâmetro. Cada um desses exemplos é analisado por meio de uma janela semanal ou de 7 dias. Ao determinar qual é o valor previsto para o próximo período, os valores dos sete dias anteriores são usados para fazer uma previsão. O modelo é definido para prever sete períodos no futuro, conforme definido pelo `horizon` parâmetro. Como uma previsão é uma estimativa informada, nem sempre é mais 100% precisa. Portanto, é bom saber o intervalo de valores nos melhores e piores cenários, conforme definido pelos limites superior e inferior. Nesse caso, o nível de confiança para os limites inferior e superior é definido como 95%. O nível de confiança pode ser aumentado ou diminuído adequadamente. Quanto maior o valor, mais largo o intervalo é entre os limites superior e inferior para obter o nível de confiança desejado.

1. Use o [`Fit`](xref:Microsoft.ML.Transforms.TimeSeries.SsaForecastingEstimator.Fit*) método para treinar o modelo e ajustar os dados para o definido anteriormente `forecastingPipeline` .

    [!code-csharp [TrainModel](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L47)]

## <a name="evaluate-the-model"></a>Avaliar o modelo

Avalie como o modelo é executado prevendo os dados do próximo ano e comparando-os com os valores reais.

1. Abaixo do `Main` método, crie um novo método de utilitário chamado `Evaluate` .

    ```csharp
    static void Evaluate(IDataView testData, ITransformer model, MLContext mlContext)
    {

    }
    ```

1. Dentro do `Evaluate` método, preveja os dados do segundo ano usando o [`Transform`](xref:Microsoft.ML.ITransformer.Transform*) método com o modelo treinado.

    [!code-csharp [EvaluateForecast](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L62)]

1. Obtenha os valores reais dos dados usando o [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable*) método.

    [!code-csharp [GetActualRentals](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L65-L67)]

1. Obtenha os valores de previsão usando o [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable*) método.

    [!code-csharp [GetForecastRentals](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L70-L72)]

1. Calcule a diferença entre os valores real e de previsão, normalmente chamados de erro.

    [!code-csharp [CalculateError](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L75)]

1. Meça o desempenho computando os valores de erro médio de média e de erro no quadrado da média de raiz.

    [!code-csharp [CalculateMetrics](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L78-L79)]

    Para avaliar o desempenho, as seguintes métricas são usadas:

    - **Erro de média absoluta**: mede como as previsões fechadas são para o valor real. Esse valor varia entre 0 e infinito. Quanto mais próximo de 0, melhor a qualidade do modelo.
    - **Erro de raiz quadrada média**: resume o erro no modelo. Esse valor varia entre 0 e infinito. Quanto mais próximo de 0, melhor a qualidade do modelo.

1. Gere as métricas para o console.

    [!code-csharp [OutputMetrics](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L82-L85)]

1. Use o `Evaluate` método dentro do `Main` método.

    [!code-csharp [EvaluateModel](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L49)]

## <a name="save-the-model"></a>Salvar o modelo

Se você estiver satisfeito com seu modelo, salve-o para uso posterior em outros aplicativos.

1. No `Main` método, crie um [`TimeSeriesPredictionEngine`](xref:Microsoft.ML.Transforms.TimeSeries.TimeSeriesPredictionEngine%602) . [`TimeSeriesPredictionEngine`](xref:Microsoft.ML.Transforms.TimeSeries.TimeSeriesPredictionEngine%602)é um método de conveniência para fazer previsões únicas.

    [!code-csharp [CreateTimeSeriesEngine](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L51)]

1. Salve o modelo em um arquivo chamado `MLModel.zip` conforme especificado pela variável definida anteriormente `modelPath` . Use o [`Checkpoint`](xref:Microsoft.ML.Transforms.TimeSeries.TimeSeriesPredictionEngine%602.CheckPoint*) método para salvar o modelo.

    [!code-csharp [SaveModel](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L52)]

## <a name="use-the-model-to-forecast-demand"></a>Usar o modelo para prever a demanda

1. Abaixo do `Evaluate` método, crie um novo método de utilitário chamado `Forecast` .

    ```csharp
    static void Forecast(IDataView testData, int horizon, TimeSeriesPredictionEngine<ModelInput, ModelOutput> forecaster, MLContext mlContext)
    {

    }
    ```

1. Dentro do `Forecast` método, use o [`Predict`](xref:Microsoft.ML.Transforms.TimeSeries.TimeSeriesPredictionEngine%602.Predict*) método para prever locações para os próximos sete dias.

    [!code-csharp [SingleForecast](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L91)]

1. Alinhe os valores reais e de previsão por sete períodos.

    [!code-csharp [GetForecastOutput](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L93-L108)]

1. Itere na saída da previsão e exiba-a no console.

    [!code-csharp [DisplayForecast](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L111-L116)]

## <a name="run-the-application"></a>Executar o aplicativo

1. Dentro do `Main` método, chame o `Forecast` método.

    [!code-csharp [BuildForecast](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L54)]

1. Execute o aplicativo. Uma saída semelhante à mostrada abaixo deve aparecer no console do. Para resumir, a saída foi condensada.

    ```text
    Evaluation Metrics
    ---------------------
    Mean Absolute Error: 726.416
    Root Mean Squared Error: 987.658

    Rental Forecast
    ---------------------
    Date: 1/1/2012
    Actual Rentals: 2294
    Lower Estimate: 1197.842
    Forecast: 2334.443
    Upper Estimate: 3471.044

    Date: 1/2/2012
    Actual Rentals: 1951
    Lower Estimate: 1148.412
    Forecast: 2360.861
    Upper Estimate: 3573.309
    ```

A inspeção dos valores reais e previstos mostra as seguintes relações:

![Comparação real de previsão vs](./media/time-series-demand-forecasting/forecast.png)

Embora os valores previstos não prevejam o número exato de locações, eles fornecem um intervalo mais estreito de valores que permite que uma operação Otimize o uso dos recursos.

Parabéns! Agora você criou com êxito um modelo de aprendizado de máquina de série temporal para prever a demanda de aluguel de bicicletas.

Você pode encontrar o código-fonte deste tutorial no repositório [dotnet/MachineLearning-Samples](https://github.com/dotnet/machinelearning-samples/tree/master/samples/csharp/getting-started/Forecasting_BikeSharingDemand) .

## <a name="next-steps"></a>Próximas etapas

- [Tarefas de aprendizado de máquina no ML.NET](../resources/tasks.md)
- [Melhorar a precisão do modelo](../resources/improve-machine-learning-model-ml-net.md)
