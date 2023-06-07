---
title: Referência de comando da CLI ML.NET
description: Visão geral, exemplos e referência para o comando de treinamento automático na ferramenta de CLI do ML.NET.
ms.date: 06/03/2020
ms.custom: mlnet-tooling
ms.openlocfilehash: 4c6cb1346c16f6162077d3414140d693de9e0d8c
ms.sourcegitcommit: 182c7b6c079ebcc0e1898dfd9e921b9ef472ea2c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2020
ms.locfileid: "85946935"
---
# <a name="the-mlnet-cli-command-reference"></a>A referência de comando da CLI ML.NET

Os `classification` `regression` comandos, e `recommendation` são os principais comandos fornecidos pela ferramenta CLI do ml.net. Esses comandos permitem que você gere bons modelos de ML.NET de qualidade para classificação, regressão e modelos de recomendação usando o AutoML (Machine Learning automatizado), bem como o código C# de exemplo para executar/pontuar esse modelo. Além disso, o código C# para treinar o modelo é gerado para você pesquisar o algoritmo e as configurações do modelo.

> [!NOTE]
> Este tópico refere-se à CLI do ML.NET e ao AutoML do ML.NET, que estão atualmente em Versão Prévia. O material pode estar sujeito a alterações.

## <a name="overview"></a>Visão geral

Exemplo de uso:

```console
mlnet regression --dataset "cars.csv" --label-col price
```

Os `mlnet` comandos de tarefa ml ( `classification` , `regression` e `recommendation` ) geram os seguintes ativos:

- Um modelo serializado. zip ("melhor modelo") pronto para uso.
- Código C# para executar/pontuar o modelo gerado.
- Código C# com o código de treinamento usado para gerar esse modelo.

Os dois primeiros ativos podem ser usados diretamente em seus aplicativos de usuário final (ASP.NET Core aplicativo Web, serviços, aplicativo de área de trabalho e muito mais) para fazer previsões com o modelo.

O terceiro ativo, o código de treinamento, mostra o que o código da API ML.NET foi usado pela CLI para treinar o modelo gerado, para que você possa investigar o algoritmo e as configurações específicas do modelo.

## <a name="examples"></a>Exemplos

O comando da CLI mais simples para um problema de classificação (AutoML infere a maior parte da configuração dos dados fornecidos):

```console
mlnet classification --dataset "customer-feedback.tsv" --label-col Sentiment
```

Outro comando simples da CLI para um problema de regressão:

``` console
mlnet regression --dataset "cars.csv" --label-col Price
```

Crie e treine um modelo de classificação com um conjunto de informações de treinamento, um conjunto de testes e mais argumentos explícitos de personalização:

```console
mlnet classification --dataset "/MyDataSets/Population-Training.csv" --test-dataset "/MyDataSets/Population-Test.csv" --label-col "InsuranceRisk" --cache on --train-time 600
```

## <a name="command-options"></a>Opções de comando

Os `mlnet` comandos de tarefa ml ( `classification` , `regression` , e `recommendation` ) treinam vários modelos com base nas opções de conjunto de ml.net e CLI fornecidas. Esses comandos também selecionam o melhor modelo, salvam o modelo como um arquivo. zip serializado e geram código C# relacionado para pontuação e treinamento.

### <a name="classification-options"></a>Opções de classificação

`mlnet classification`A execução treinará um modelo de classificação. Escolha este comando se desejar que um modelo de ML categorize dados em duas ou mais classes (por exemplo, análise de sentimentos).

```console
mlnet classification

--dataset <path> (REQUIRED)

--label-col <col> (REQUIRED)

--cache <option>

--has-header (Default: true)

--ignore-cols <cols>

--log-file-path <path>

--name <name>

-o, --output <path>

--test-dataset <path>

--train-time <time> (Default: 30 minutes, in seconds)

--validation-dataset <path>

-v, --verbosity <v>

-?, -h, --help

```

### <a name="regression-options"></a>Opções de regressão

`mlnet regression`A execução treinará um modelo de regressão. Escolha este comando se desejar que um modelo de ML preveja um valor numérico (por exemplo, previsão de preço).

```console
mlnet classification

--dataset <path> (REQUIRED)

--label-col <col> (REQUIRED)

--cache <option>

--has-header (Default: true)

--ignore-cols <cols>

--log-file-path <path>

--name <name>

-o, --output <path>

--test-dataset <path>

--train-time <time> (Default: 30 minutes, in seconds)

--validation-dataset <path>

-v, --verbosity <v>

-?, -h, --help

```

### <a name="recommendation-options"></a>Opções de recomendação

`mlnet recommendation`A execução treinará um modelo de recomendação.  Escolha este comando se desejar que um modelo de ML recomende itens aos usuários com base em classificações (por exemplo, recomendação de produto).

```console
mlnet classification

--dataset <path> (REQUIRED)

--item-col <col> (REQUIRED)

--rating-col <col> (REQUIRED)

--user-col <col> (REQUIRED)

--cache <option>

--has-header (Default: true)

--log-file-path <path>

--name <name>

-o, --output <path>

--test-dataset <path>

--train-time <time> (Default: 30 minutes, in seconds)

--validation-dataset <path>

-v, --verbosity <v>

-?, -h, --help

```

As opções de entrada inválidas fazem com que a ferramenta CLI emita uma lista de entradas válidas e uma mensagem de erro.

## <a name="dataset"></a>Dataset

`--dataset | -d` (string)

Esse argumento fornece o caminho do arquivo para uma das seguintes opções:

- *R: todo o arquivo do conjunto de arquivos:* Se você estiver usando essa opção e o usuário não estiver fornecendo `--test-dataset` e `--validation-dataset` , em seguida, a validação cruzada (k-fold, etc.) ou abordagens de divisão de dados automatizadas serão usadas internamente para validar o modelo. Nesse caso, o usuário precisará apenas fornecer o caminho do arquivo do conjunto de dados.

- *B: o arquivo do conjunto de arquivos de treinamento:* Se o usuário também estiver fornecendo conjuntos de valores para validação de modelo (usando `--test-dataset` e, opcionalmente `--validation-dataset` ), o `--dataset` argumento significará ter apenas o "conjunto de linhas de treinamento". Por exemplo, ao usar uma abordagem de 80-20% para validar a qualidade do modelo e obter métricas de precisão, o "conjunto de dados treinamento" terá 80% dos dados e o "conjunto de dados de teste" terá 20% dos dados.

## <a name="test-dataset"></a>Conjunto de dados de teste

`--test-dataset | -t` (string)

Caminho do arquivo que aponta para o arquivo de conjunto de dados de teste, por exemplo, ao usar uma abordagem de 80-20% ao realizar validações regulares para obter métricas de precisão.

Ao usar `--test-dataset`, `--dataset` também é necessária.

O argumento `--test-dataset` é opcional, a menos que o --validation-dataset seja usado. Nesse caso, o usuário precisa usar os três argumentos.

## <a name="validation-dataset"></a>Conjunto de dados de validação

`--validation-dataset | -v` (string)

Caminho do arquivo apontando para o arquivo de conjunto de dados de validação. O conjunto de dados de validação é opcional em qualquer caso.

Se usar um `validation dataset`, o comportamento deverá ser:

- Os argumentos `test-dataset` e `--dataset` também são necessários.

- O conjunto de dados `validation-dataset` é usado para estimar o erro de previsão para a seleção de modelo.

- O `test-dataset` é usado para a avaliação do erro de generalização do modelo final escolhido. O ideal é que o conjunto de teste seja mantido em um "cofre" e seja levado somente no final da análise de dados.

Basicamente, ao usar um `validation dataset` mais o `test dataset`, a fase de validação é dividida em duas partes:

1. Na primeira parte, você apenas examina seus modelos e seleciona a abordagem com melhor desempenho usando os dados de validação (=validation)
2. Em seguida, você pode estimar a precisão da abordagem selecionada (=test).

Portanto, a separação dos dados pode ser 80/10/10 ou 75/15/10. Por exemplo:

- O arquivo `training-dataset` deve ter 75% dos dados.
- O arquivo `validation-dataset` deve ter 15% dos dados.
- O arquivo `test-dataset` deve ter 10% dos dados.

Em qualquer caso, essas porcentagens serão decididas pelo usuário usando a CLI que fornecerá os arquivos já dividido.

## <a name="label-column"></a>Coluna de rótulo

`--label-col`(int ou String)

Com esse argumento, uma coluna de objetivo/destino específica (a variável que você deseja prever) pode ser especificada usando o nome da coluna definido no cabeçalho do conjunto de informações ou no índice numérico da coluna no arquivo do conjunto de informações (os valores de índice da coluna começam em 0).

Esse argumento é usado para problemas de *classificação* e *regressão* .

## <a name="item-column"></a>Coluna do item

`--item-col`(int ou String)

A coluna item tem a lista de itens que os usuários avaliam (itens são recomendados para os usuários). Essa coluna pode ser especificada usando-se o nome da coluna definido no cabeçalho do conjunto de valores ou no índice numérico da coluna no arquivo do conjunto de arquivos (o valor do índice da coluna começa em 0).

Esse argumento é usado somente para a tarefa de *recomendação* .

## <a name="rating-column"></a>Coluna de classificação

`--rating-col`(int ou String)

A coluna classificação tem a lista de classificações que são dadas aos itens pelos usuários. Essa coluna pode ser especificada usando-se o nome da coluna definido no cabeçalho do conjunto de valores ou no índice numérico da coluna no arquivo do conjunto de arquivos (o valor do índice da coluna começa em 0).

Esse argumento é usado somente para a tarefa de *recomendação* .

## <a name="user-column"></a>Coluna do usuário

`--user-col`(int ou String)

A coluna usuário tem a lista de usuários que fornecem classificações aos itens. Essa coluna pode ser especificada usando-se o nome da coluna definido no cabeçalho do conjunto de valores ou no índice numérico da coluna no arquivo do conjunto de arquivos (o valor do índice da coluna começa em 0).

Esse argumento é usado somente para a tarefa de *recomendação* .

## <a name="ignore-columns"></a>Ignorar colunas

`--ignore-columns` (string)

Com este argumento, você pode ignorar as colunas existentes no arquivo de conjunto de dados para que elas não sejam carregadas e usadas pelos processos de treinamento.

Especifique os nomes de colunas que você deseja ignorar. Use ',' (vírgula com espaço) ou ' ' (espaço) para separar vários nomes de coluna. Você pode usar aspas para nomes de coluna que contêm espaço em branco (por exemplo, "usuário logado").

Exemplo:

`--ignore-columns email, address, id, logged_in`

## <a name="has-header"></a>Tem cabeçalho

`--has-header` (bool)

Especifique se os arquivos de conjunto de dados têm uma linha de cabeçalho.
Os valores possíveis são:

- `true`
- `false`

A CLI do ML.NET tentará detectar essa propriedade se esse argumento não for especificado pelo usuário.

## <a name="train-time"></a>Tempo de treinamento

`--train-time` (string)

Por padrão, o tempo máximo de exploração/treinamento é de 30 minutos.

Esse argumento define o tempo máximo (em segundos) para o processo explorar vários treinadores e configurações. O tempo configurado poderá ser excedido se o tempo fornecido for muito curto (digamos, dois segundos) para uma única iteração. Nesse caso, o tempo real é o tempo necessário para produzir uma configuração de modelo em uma única iteração.

O tempo necessário para iterações pode variar, dependendo do tamanho do conjunto de dados.

## <a name="cache"></a>Cache

`--cache` (string)

Se você usar o armazenamento em cache, o conjunto de dados de treinamento inteiro será carregado na memória.

Para conjuntos de dados pequenos e médios, usar o cache pode melhorar drasticamente o desempenho de treinamento, o que significa que o tempo de treinamento pode ser menor do que quando você não usa o cache.

No entanto, para grandes conjuntos de dados, carregar todos os dados na memória pode ter um efeito negativo, pois você poderá ficar com memória insuficiente. Ao treinar com arquivos de conjunto de dados grandes sem usar o cache, o ML.NET transmitirá partes de dados da unidade quando ele precisar carregar mais dados durante o treinamento.

É possível especificar os seguintes valores:

`on`: Força o cache a ser usado durante o treinamento.
`off`: Força o cache a não ser usado durante o treinamento.
`auto`: Dependendo da heurística do AutoML, o cache será usado ou não. Em geral, conjuntos de dados pequenos/médios usarão o cache e grandes conjuntos de dados não usarão o cache se você usar a escolha `auto`.

Se você não especificar o parâmetro `--cache`, a configuração de cache `auto` será usada por padrão.

## <a name="name"></a>Name

`--name` (string)

O nome para a solução ou projeto de saída criado. Se nenhum nome for especificado, o nome `sample-{mltask}` será usado.

O arquivo de modelo do ML.NET (arquivo zip) também receberá o mesmo nome.

## <a name="output-path"></a>Caminho de saída

`--output | -o` (string)

Local/pasta raiz para colocar a saída gerada. O padrão é o diretório atual.

## <a name="verbosity"></a>Detalhamento

`--verbosity | -v` (string)

Define o nível de detalhamento da saída padrão.

Valores permitidos são:

- `q[uiet]`
- `m[inimal]` (por padrão)
- `diag[nostic]` (nível de informações de registro em log)

Por padrão, a ferramenta CLI deve mostrar alguns comentários mínimos ( `minimal` ) ao trabalhar, como mencionar que ele está funcionando e, se possível, quanto tempo é deixado ou qual% do tempo é concluído.

## <a name="help"></a>Ajuda

`-h |--help`

Imprime uma ajuda para o comando com uma descrição para o parâmetro de cada comando.

## <a name="see-also"></a>Veja também

- [Como instalar a ferramenta de CLI do ML.NET](../how-to-guides/install-ml-net-cli.md)
- [Visão geral da CLI do ML.NET](../automate-training-with-cli.md)
- [Tutorial: analisar o sentimentos usando a CLI do ML.NET](../tutorials/sentiment-analysis-cli.md)
- [Telemetria na CLI do ML.NET](../resources/ml-net-cli-telemetry.md)
