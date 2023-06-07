---
title: Automatizar o treinamento de modelos com a CLI do ML.NET
description: Descubra como usar a ferramenta de CLI do ML.NET para treinar automaticamente o melhor modelo da linha de comando.
ms.date: 06/03/2020
ms.custom: how-to, mlnet-tooling
ms.openlocfilehash: d7c6102c2257be1daa613fde0edabce83d04b414
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84589650"
---
# <a name="automate-model-training-with-the-mlnet-cli"></a>Automatizar o treinamento de modelos com a CLI do ML.NET

A CLI do ML.NET automatiza a geração de modelos para desenvolvedores .NET.

Para usar a API do ML.NET por si só (sem a CLI de AutoML do ML.NET), você precisa escolher um treinador (implementação de um algoritmo de aprendizado de máquina para uma tarefa específica) e o conjunto de transformações de dados (engenharia de recursos) para aplicar aos seus dados. O pipeline ideal variará para cada conjunto de dados e selecionar o algoritmo ideal entre todas as opções aumenta a complexidade. Além disso, cada algoritmo tem um conjunto de hiperparâmetros a serem ajustados. Portanto, você pode passar semanas e até meses em otimização de modelos de machine learning na tentativa de encontrar as combinações de engenharia de recursos, algoritmos de aprendizado e hiperparâmetros.

A CLI do ML.NET simplifica esse processo usando o AutoML (Machine Learning automatizado).

> [!NOTE]
> Este tópico refere-se à **CLI** do ML.NET e ao **AutoML** do ML.NET, que estão atualmente em Versão Prévia. O material pode estar sujeito a alterações.

## <a name="what-is-the-mlnet-command-line-interface-cli"></a>O que é a CLI (interface de linha de comando) do ML.NET?

A CLI do ML.NET é uma [ferramenta .NET Core](../core/tools/global-tools.md). Depois de instalado, você dá a ele uma tarefa de aprendizado de máquina e um conjunto de informações de treinamento e gera um modelo ML.NET, bem como o código C# a ser executado para usar o modelo em seu aplicativo.

Conforme mostrado na figura a seguir, é simples gerar um modelo ML.NET de alta qualidade (arquivo. zip de modelo serializado) mais o código C# de exemplo para executar/pontuar esse modelo. Além disso, o código C# para criar/treinar esse modelo também é gerado, de modo que você pode pesquisar e iterar pelo algoritmo e pelas configurações usados para esse "melhor modelo" gerado.

![imagem](media/automate-training-with-cli/cli-high-level-process.png "O mecanismo AutoML está trabalhando dentro da CLI do ML.NET")

Você pode gerar esses ativos de seus próprios conjuntos de dados sem codificação por conta própria, portanto, ele também melhorará a sua produtividade, mesmo se você já conhecer o ML.NET.

Atualmente, as tarefas de ML compatíveis com a CLI do ML.NET são:

- classificação (binário e várias classes)
- regressão
- recomendação
- Futuro: outras tarefas de aprendizado de máquina, como classificação de imagem, classificação, detecção de anomalias, clustering

Exemplo de uso (cenário de classificação):

```console
mlnet classification --dataset "yelp_labelled.txt" --label-col 1 --has-header false --train-time 10
```

![imagem](media/automate-training-with-cli/mlnet-classification-powershell.gif)

Você pode executá-lo da mesma maneira no *Windows PowerShell*, no *MacOS/Linux bash*ou no *Windows cmd*. No entanto, o preenchimento automático de tabela (sugestões de parâmetro) não funcionará no *CMD do Windows*.

## <a name="output-assets-generated"></a>Ativos de saída gerados

Os comandos de tarefa ML na CLI geram os seguintes ativos na pasta de saída:

- Um modelo .zip serializado ("melhor modelo") pronto para uso voltado à execução de previsões.
- Solução em C# com:
  - Código C# para a execução/pontuação que gerou o modelo (para fazer previsões em seus aplicativos do usuário final com esse modelo).
  - Código C# com o código de treinamento usado para gerar esse modelo (para fins de aprendizado ou retreinamento de modelo).
- Arquivo de log com informações de todas as iterações/varreduras entre vários algoritmos avaliados, incluindo sua configuração/pipeline detalhado.

Os primeiros dois ativos podem ser usados diretamente em seus aplicativos de usuário final (aplicativo Web ASP.NET Core, serviços, aplicativo da área de trabalho etc.) para fazer previsões com esse modelo de ML gerado.

O terceiro ativo, o código de treinamento, mostra a você qual código de API do ML.NET foi usado pela CLI para treinar o modelo gerado, de modo que você pode treinar novamente seu modelo e investigar e iterar em quais hiperparâmetros e treinador/algoritmo específicos foram selecionados pela CLI e AutoML nos bastidores.

## <a name="understanding-the-quality-of-the-model"></a>Noções básicas sobre a qualidade do modelo

Quando você gera um ' melhor modelo ' com a ferramenta CLI, vê métricas de qualidade (como precisão e R-quadrado) conforme apropriado para a tarefa ML que você está direcionando.

Aqui, essas métricas são resumidas agrupadas pela tarefa ML para que você possa entender a qualidade do ' melhor modelo ' gerado automaticamente.

### <a name="metrics-for-classification-models"></a>Métricas para modelos de classificação

O seguinte exibe a lista de métricas de classificação dos cinco principais modelos encontrados pela CLI:

![imagem](media/automate-training-with-cli/cli-multiclass-classification-metrics.png)

 A precisão é uma métrica popular para problemas de classificação. no entanto, a precisão nem sempre é a melhor métrica para selecionar o melhor modelo, conforme explicado nas referências a seguir. Há casos em que você precisa avaliar a qualidade do seu modelo com métricas adicionais.

Para explorar e entender as métricas que são geradas pela CLI, consulte [métricas de avaliação para classificação](resources/metrics.md#evaluation-metrics-for-multi-class-classification).

### <a name="metrics-for-regression-and-recommendation-models"></a>Métricas para modelos de regressão e recomendação

Um modelo de regressão se ajustará bem aos dados se as diferenças entre os valores observados e os previstos do modelo forem pequenas e sem desvio. A regressão pode ser avaliada com certas métricas.

Você verá uma lista semelhante de métricas para os melhores cinco modelos de qualidade encontrados pela CLI. Neste caso em particular, relacionados a uma tarefa de ML de regressão:

![imagem](media/automate-training-with-cli/cli-regression-metrics.png)

Para explorar e entender as métricas que são geradas pela CLI, consulte [métricas de avaliação para regressão](resources/metrics.md#evaluation-metrics-for-regression-and-recommendation).

## <a name="see-also"></a>Consulte também

- [Como instalar a ferramenta de CLI do ML.NET](how-to-guides/install-ml-net-cli.md)
- [Tutorial: analisar o sentimentos usando a CLI do ML.NET](tutorials/sentiment-analysis-cli.md)
- [Referência de comandos da CLI do ML.NET](reference/ml-net-cli-reference.md)
- [Telemetria na CLI do ML.NET](resources/ml-net-cli-telemetry.md)
