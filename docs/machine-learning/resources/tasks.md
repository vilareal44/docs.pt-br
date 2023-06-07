---
title: Tarefas de aprendizado de máquina
description: Explore as diferentes tarefas de aprendizado de máquina e as tarefas associadas compatíveis com o ML.NET.
ms.date: 12/23/2019
ms.openlocfilehash: e6e36bd65dbadb8cb7b8edbf9e2e82071c208378
ms.sourcegitcommit: ee5b798427f81237a3c23d1fd81fff7fdc21e8d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84144442"
---
# <a name="machine-learning-tasks-in-mlnet"></a>Tarefas de aprendizado de máquina no ML.NET

Uma tarefa de aprendizado de máquina é o tipo de previsão ou inferência que está sendo feita, com base no problema ou na pergunta que está sendo solicitada e nos dados disponíveis. Por exemplo, a tarefa de classificação atribui dados a categorias e a tarefa de clustering agrupa os dados de acordo com a similaridade.

As tarefas de aprendizado de máquina dependem de padrões nos dados em vez de serem programadas explicitamente.

Este artigo descreve as diferentes tarefas de aprendizado de máquina que você pode escolher em ML.NET e alguns casos de uso comuns.

Depois de decidir a tarefa ideal para seu cenário, será preciso escolher o melhor algoritmo para treinar seu modelo. Os algoritmos disponíveis são listados na seção para cada tarefa.

## <a name="binary-classification"></a>Classificação binária

Uma tarefa de [aprendizado de máquina supervisionado](glossary.md#supervised-machine-learning) que é usada para prever a qual das duas classes (categorias) uma instância de dados pertence. A entrada de um algoritmo de classificação é um conjunto de exemplos rotulados, em que cada rótulo é um número inteiro de 0 ou 1. A saída de um algoritmo de classificação binária é um classificador, que pode ser usado para prever a classe de novas instâncias sem rótulo. Exemplos de cenários de classificação binária incluem:

* [Reconhece](../tutorials/sentiment-analysis.md) como "positivo" ou "negativo".
* Diagnosticar se um paciente tem uma determinada doença ou não.
* Tomar a decisão de marcar um email como "spam" ou não.
* Determinar se uma foto contém um item específico ou não, como um cão ou frutas.

Para obter mais informações, consulte o artigo [Classificação binária](https://en.wikipedia.org/wiki/Binary_classification) na Wikipédia.

### <a name="binary-classification-trainers"></a>Treinadores da classificação binária

Você pode treinar um modelo de classificação binária usando os seguintes algoritmos:

* <xref:Microsoft.ML.Trainers.AveragedPerceptronTrainer>
* <xref:Microsoft.ML.Trainers.SdcaLogisticRegressionBinaryTrainer>
* <xref:Microsoft.ML.Trainers.SdcaNonCalibratedBinaryTrainer>
* <xref:Microsoft.ML.Trainers.SymbolicSgdLogisticRegressionBinaryTrainer>
* <xref:Microsoft.ML.Trainers.LbfgsLogisticRegressionBinaryTrainer>
* <xref:Microsoft.ML.Trainers.LightGbm.LightGbmBinaryTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.FastTreeBinaryTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.FastForestBinaryTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.GamBinaryTrainer>
* <xref:Microsoft.ML.Trainers.FieldAwareFactorizationMachineTrainer>
* <xref:Microsoft.ML.Trainers.PriorTrainer>
* <xref:Microsoft.ML.Trainers.LinearSvmTrainer>

### <a name="binary-classification-inputs-and-outputs"></a>Saídas e entradas de classificação binária

Para obter melhores resultados com a classificação binária, os dados de treinamento devem ser equilibrados (ou seja, números iguais de dados de treinamento positivos e negativos). Os valores ausentes devem ser manipulados antes do treinamento.

Os dados da coluna de rótulo de entrada devem ser <xref:System.Boolean>.
Os dados da coluna de recursos de entrada devem ser um vetor de tamanho fixo de <xref:System.Single>.

Esses treinadores geram as seguintes colunas:

| Nome da Coluna de Saída | Tipo de coluna | Descrição|
| -- | -- | -- |
| `Score` | <xref:System.Single> | A pontuação bruta calculada pelo modelo|
| `PredictedLabel` | <xref:System.Boolean> | O rótulo previsto com base no sinal da pontuação. Uma pontuação negativa é mapeada para `false` e uma pontuação positiva é mapeada para `true`.|

## <a name="multiclass-classification"></a>Classificação multiclasse

Uma tarefa de [aprendizado de máquina supervisionado](glossary.md#supervised-machine-learning) que é usada para prever a classe (categoria) de uma instância de dados. A entrada de um algoritmo de classificação é um conjunto de exemplos rotulados. Cada rótulo normalmente começa como texto. Ele é executado por meio de TermTransform, que converte-o para o tipo de Chave (numérico). A saída de um algoritmo de classificação é um classificador, que pode ser usado para prever a classe de novas instâncias sem rótulo. Exemplos de cenários de classificação multiclasse incluem:

* Determinar a raça de um cão como um "Husky Siberiano", "Golden Retriever", "Poodle", etc.
* Entender as resenhas de filmes como "positivas", "neutras" ou "negativas".
* Categorizar as avaliações de hotel como "local", "preço", "limpeza", etc.

Para obter mais informações, consulte o artigo [Classificação multiclasse](https://en.wikipedia.org/wiki/Multiclass_classification) na Wikipédia.

>[!NOTE]
>Uma vs todas as atualizações de [aprendizes de classificação binária](#binary-classification) para atuar em conjuntos de dados multiclasse. Mais informações sobre a [Wikipédia](https://en.wikipedia.org/wiki/Multiclass_classification#One-vs.-rest).

### <a name="multiclass-classification-trainers"></a>Treinadores de classificação multiclasse

Você pode treinar um modelo de classificação multiclasse usando os seguintes algoritmos de treinamento:

* <xref:Microsoft.ML.Trainers.LightGbm.LightGbmMulticlassTrainer>
* <xref:Microsoft.ML.Trainers.SdcaMaximumEntropyMulticlassTrainer>
* <xref:Microsoft.ML.Trainers.SdcaNonCalibratedMulticlassTrainer>
* <xref:Microsoft.ML.Trainers.LbfgsMaximumEntropyMulticlassTrainer>
* <xref:Microsoft.ML.Trainers.NaiveBayesMulticlassTrainer>
* <xref:Microsoft.ML.Trainers.OneVersusAllTrainer>
* <xref:Microsoft.ML.Trainers.PairwiseCouplingTrainer>
* <xref:Microsoft.ML.Vision.ImageClassificationTrainer>

### <a name="multiclass-classification-inputs-and-outputs"></a>Saídas e entradas de classificação multiclasse

Os dados da coluna de rótulo de entrada devem ser do tipo [chave](xref:Microsoft.ML.Data.KeyDataViewType).
A coluna de recursos deve ser um vetor de tamanho fixo de <xref:System.Single>.

Este treinador produz o seguinte:

| Nome de saída | Type | Description|
| -- | -- | -- |
| `Score` | Vetor de <xref:System.Single> | As pontuações de todas as classes. Um valor mais alto significa maior probabilidade de se enquadrar na classe associada. Se o elemento iº elemento tiver o maior valor, o índice de rótulo previsto será i. Observe que i é o índice baseado em zero. |
| `PredictedLabel` | tipo de [chave](xref:Microsoft.ML.Data.KeyDataViewType) | O índice do rótulo previsto. Se seu valor for i, o rótulo real será a iº categoria no tipo de rótulo de entrada com valor de chave. |

## <a name="regression"></a>Regressão

Uma tarefa de [aprendizado de máquina supervisionado](glossary.md#supervised-machine-learning) que é usada para prever o valor do rótulo de um conjunto de recursos relacionados. O rótulo pode ser de qualquer valor real e não pertence a um conjunto finito de valores como nas tarefas de classificação. Os algoritmos de regressão modelam a dependência do rótulo em seus recursos relacionados para determinar como o rótulo será alterado à medida que os valores dos recursos forem variados. A entrada de um algoritmo de regressão é um conjunto de exemplos com rótulos de valores conhecidos. A saída de um algoritmo de regressão é uma função, que você pode usar para prever o valor de rótulo para qualquer novo conjunto de recursos de entrada. Exemplos de cenários de regressão incluem:

* Previsão de preços de casas com base nos atributos da casa, como número de quartos, localização ou tamanho.
* Previsão de preços futuros de ações com base em dados históricos e tendências atuais do mercado.
* Previsão de vendas de um produto com base em orçamentos de publicidade.

### <a name="regression-trainers"></a>Treinadores de regressão

Você pode treinar um modelo de regressão usando os seguintes algoritmos:

* <xref:Microsoft.ML.Trainers.LbfgsPoissonRegressionTrainer>
* <xref:Microsoft.ML.Trainers.LightGbm.LightGbmRegressionTrainer>
* <xref:Microsoft.ML.Trainers.SdcaRegressionTrainer>
* <xref:Microsoft.ML.Trainers.OlsTrainer>
* <xref:Microsoft.ML.Trainers.OnlineGradientDescentTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.FastTreeRegressionTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.FastTreeTweedieTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.FastForestRegressionTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.GamRegressionTrainer>

### <a name="regression-inputs-and-outputs"></a>Saídas e entradas de regressão

Os dados da coluna de rótulo de entrada devem ser <xref:System.Single>.

Os treinadores para esta tarefa produzem a seguinte saída:

| Nome de saída | Type | Description|
| -- | -- | -- |
| `Score` | <xref:System.Single> | A pontuação bruta prevista pelo modelo |

## <a name="clustering"></a>Clustering

Uma tarefa de [aprendizado de máquina não supervisionado](glossary.md#unsupervised-machine-learning) que é usada para agrupar instâncias de dados em clusters que contêm características semelhantes. O clustering também pode ser usado para identificar relacionamentos em um conjunto de dados que você pode não obter logicamente por meio de navegação ou observação simples. As entradas e saídas de um algoritmo de clustering depende a metodologia escolhida. Você pode usar uma abordagem baseada em distribuição, centroide, conectividade ou densidade. Atualmente, o ML.NET oferece suporte a uma abordagem baseada em centroides usando o clustering de K-Means. Exemplos de cenários de clustering incluem:

* Compreender os segmentos de hóspedes do hotel com base nos hábitos e características das opções de hotel.
* Identificar segmentos de clientes e dados demográficos para ajudar a criar campanhas de publicidade segmentadas.
* Categorizar o inventário com base nas métricas de fabricação.

### <a name="clustering-trainer"></a>Treinador de clustering

Você pode treinar um modelo de clustering usando os seguintes algoritmos:

* <xref:Microsoft.ML.Trainers.KMeansTrainer>

### <a name="clustering-inputs-and-outputs"></a>Entradas e saídas de clustering

Os dados de recursos de entrada devem ser <xref:System.Single>. Nenhum rótulo é necessário.

Este treinador produz o seguinte:

| Nome de saída | Type | Descrição|
| -- | -- | -- |
| `Score` | vetor de <xref:System.Single> | As distâncias do ponto de dados fornecido para todos os centroides |
| `PredictedLabel` | tipo de [chave](xref:Microsoft.ML.Data.KeyDataViewType) | O índice do cluster mais próximo previsto pelo modelo. |

## <a name="anomaly-detection"></a>Detecção de anomalias

Esta tarefa cria um modelo de detecção de anomalias usando a Análise de Componente Principal (PCA). A Detecção de Anomalias Baseada em PCA ajuda a criar um modelo em cenários onde é fácil obter dados de treinamento de uma classe, como transações válidas, mas é difícil obter exemplos suficientes das anomalias direcionadas.

O PCA, uma técnica estabelecida no aprendizado de máquina, é frequentemente usado na análise exploratória de dados, pois revela a estrutura interna dos dados e explica sua variação. O PCA trabalha analisando dados que contêm muitas variáveis. Ele procura correlações entre as variáveis e determina a combinação de valores que melhor detecta as diferenças nos resultados. Esses valores de recursos combinados são usados para criar um espaço de recurso mais compacto chamado de componentes principais.

A detecção de anomalias abrange várias tarefas importantes no aprendizado de máquina:

* Identificar transações que são potencialmente fraudulentas.
* Aprender padrões que indicam que ocorreu uma invasão da rede.
* Localizar clusters anormais de pacientes.
* Verificar valores inseridos em um sistema.

Como as anomalias são eventos raros por definição, pode ser difícil coletar um exemplo representativo dos dados para usar na modelagem. Os algoritmos incluídos nesta categoria foram especialmente projetados para abordar os principais desafios de criar e treinar modelos usando conjuntos de dados desequilibrados.

### <a name="anomaly-detection-trainer"></a>Treinador de detecção de anomalias

Você pode treinar um modelo de detecção de anomalias usando os seguintes algoritmos:

* <xref:Microsoft.ML.Trainers.RandomizedPcaTrainer>

### <a name="anomaly-detection-inputs-and-outputs"></a>Saídas e entradas de detecção de anomalias

Os recursos de entrada devem ser um vetor de tamanho fixo de <xref:System.Single>.

Este treinador produz o seguinte:

| Nome de saída | Type | Descrição|
| -- | -- | -- |
| `Score` | <xref:System.Single> | A pontuação não negativa não associada calculada pelo modelo de detecção de anomalias |
| `PredictedLabel` | <xref:System.Boolean> | Um valor true/false que representa se a entrada é uma anomalia (PredictedLabel = true) ou não (PredictedLabel = false) |

## <a name="ranking"></a>Classificação

Uma tarefa de classificação cria um classificador a partir de um conjunto de exemplos rotulados. Este conjunto de exemplo consiste em grupos de instâncias que podem ser marcados com um determinado critério. Os rótulos de classificação são { 0, 1, 2, 3, 4 } para cada instância.  O classificador é treinado para classificar novos grupos de instâncias com pontuações desconhecidas para cada instância. Os aprendizes de classificação do ML.NET são baseados na [classificação de máquinas aprendidas](https://en.wikipedia.org/wiki/Learning_to_rank).

### <a name="ranking-training-algorithms"></a>Algoritmos de treinamento de classificação

Você pode treinar um modelo de classificação usando os seguintes algoritmos:

* <xref:Microsoft.ML.Trainers.LightGbm.LightGbmRankingTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.FastTreeRankingTrainer>

### <a name="ranking-input-and-outputs"></a>Entrada e saídas de classificação

O tipo de dados de rótulo de entrada deve ser do tipo [chave](xref:Microsoft.ML.Data.KeyDataViewType) ou <xref:System.Single>. O valor do rótulo determina a relevância, em que valores mais altos indicam maior relevância. Se o rótulo for um tipo [chave](xref:Microsoft.ML.Data.KeyDataViewType), o índice de chave será o valor de relevância, em que o menor índice é o menos relevante. Se o rótulo for um <xref:System.Single>, valores maiores indicarão maior relevância.

Os dados do recurso devem ser um vetor de tamanho fixo de <xref:System.Single> e a coluna de grupo de linha de entrada deve ser do tipo [chave](xref:Microsoft.ML.Data.KeyDataViewType).

Este treinador produz o seguinte:

| Nome de saída | Type | Descrição|
| -- | -- | -- |
| `Score` | <xref:System.Single> | A pontuação não associada calculada pelo modelo para determinar a previsão |

## <a name="recommendation"></a>Recomendação

Uma tarefa de recomendação permite produzir uma lista de produtos ou serviços recomendados. O ML.NET usa a [Fatoração matricial (MF)](https://en.wikipedia.org/wiki/Matrix_factorization_%28recommender_systems%29), um algoritmo de [filtragem colaborativa](https://en.wikipedia.org/wiki/Collaborative_filtering) para as recomendações quando você tem dados históricos de classificação do produto em seu catálogo. Por exemplo, você tem dados históricos de classificação de filmes para seus usuários e deseja recomendar outros filmes que eles provavelmente assistirão posteriormente.

### <a name="recommendation-training-algorithms"></a>Algoritmos de treinamento de recomendação

Você pode treinar um modelo de recomendação usando os seguintes algoritmos:

* <xref:Microsoft.ML.Trainers.MatrixFactorizationTrainer>

## <a name="forecasting"></a>Previsão

A tarefa de previsão usa dados de série temporal anteriores para fazer previsões sobre o comportamento futuro. Os cenários aplicáveis à previsão incluem previsão do tempo, previsões de vendas sazonais e manutenção preditiva,

### <a name="forecasting-trainers"></a>Prevendo treinadores

Você pode treinar um modelo de previsão com o seguinte algoritmo:

<xref:Microsoft.ML.TimeSeriesCatalog.ForecastBySsa*>
