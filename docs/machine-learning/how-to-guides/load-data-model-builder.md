---
title: Carregar dados de treinamento para o construtor de modelos
description: Saiba como carregar os dados de treinamento de um banco de SQL Server ou de um arquivo para uso em um dos cenários do construtor de modelos para ML.NET.
ms.date: 10/29/2019
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc, how-to, mlnet-tooling
ms.openlocfilehash: 64e366b3c66427ccd2810324abeb880f6cb9ebc1
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84602200"
---
# <a name="load-training-data-into-model-builder"></a>Carregar dados de treinamento no construtor de modelos

Saiba como carregar seus conjuntos de dados de treinamento de um arquivo ou de um SQL Server banco de dados para uso em um dos cenários do construtor de modelos para ML.NET. Os cenários do construtor de modelos podem usar SQL Server bancos de dados, arquivos de imagem e formatos de arquivo CSV ou TSV como dado de treinamento.

## <a name="training-dataset-limitations-in-model-builder"></a>Limitações do conjunto de os conjuntos de modelos

O construtor de modelos limita a quantidade e o tipo de dados que você pode usar para modelos de treinamento:

- Dados de SQL Server: 100.000 linhas
- Arquivos CSV e TSV: sem limite de tamanho
- Imagens: somente PNG e JPG.

## <a name="model-builder-scenarios"></a>Cenários do construtor de modelos

O construtor de modelos ajuda a criar modelos para os seguintes cenários de aprendizado de máquina:

- Análise de sentimentos (classificação binária): classificar dados textuais em duas categorias.
- Classificação do problema (classificação multiclasse): classificar dados textuais em três ou mais categorias.
- Previsão de preço (regressão): prever um valor numérico.
- Classificação de imagem (aprendizado profundo): categorizar imagens com base em características.
- Cenário personalizado: Crie cenários personalizados de seus dados usando regressão, classificação e outras tarefas.

Este artigo aborda cenários de classificação e regressão com dados textuais ou numéricos e cenários de classificação de imagem.

## <a name="load-text-or-numeric-data-from-a-file"></a>Carregar texto ou dados numéricos de um arquivo

Você pode carregar texto ou dados numéricos de um arquivo no construtor de modelos. Ele aceita formatos de arquivo CSV (delimitados por vírgula) ou TSV (delimitados por tabulação).

1. Na etapa dados do construtor de modelos, selecione **arquivo** na lista suspensa fonte de dados.
2. Selecione o botão ao lado da caixa de texto **selecionar um arquivo** e use o explorador de arquivos para procurar e selecionar o arquivo de dados.
3. Escolha uma categoria na **coluna para prever (rótulo)** DropDown.
4. Na lista suspensa **colunas de entrada (recursos)** , confirme se as colunas de dados que você deseja incluir estão marcadas.

Você concluiu a configuração do arquivo de fonte de dados para o construtor de modelos. Selecione o link **treinar** para mover para a próxima etapa no construtor de modelos.

## <a name="load-data-from-a-sql-server-database"></a>Carregar dados de um banco de SQL Server

O construtor de modelos dá suporte ao carregamento de dados de bancos de dado de SQL Server locais e remotos.

Para carregar dados de um banco de dado SQL Server no construtor de módulos:

1. Na etapa dados do construtor de modelos, selecione **SQL Server** na lista suspensa fonte de dados.
1. Selecione o botão ao lado da caixa de texto **conectar ao SQL Server banco de dados** .
    1. Na caixa de diálogo **escolher dados** , selecione **Microsoft SQL Server arquivo de banco**.
    1. Desmarque a caixa de seleção **sempre usar esta seleção** e selecione **continuar**
    1. Na caixa de diálogo **Propriedades da conexão** , selecione **procurar** e selecione o baixado. Arquivo MDF.
    1. Selecione **OK**
1. Escolha o nome do conjunto de campos na lista suspensa **nome da tabela** .
1. Na lista suspensa **coluna a prever (rótulo)** , escolha a categoria de dados na qual você deseja fazer uma previsão.
1. Na lista suspensa **colunas de entrada (recursos)** , confirme se as colunas que você deseja incluir estão marcadas.

Você concluiu a configuração do arquivo de fonte de dados para o construtor de modelos. Selecione o link **treinar** para mover para a próxima etapa no construtor de modelos.

## <a name="set-up-image-data-files"></a>Configurar arquivos de dados de imagem

O construtor de modelos espera que os dados de imagem sejam arquivos JPG ou PNG organizados em pastas que correspondam às categorias da classificação.

Para carregar imagens no construtor de modelos, forneça o caminho para um único diretório de nível superior:

- Esse diretório de nível superior contém uma subpasta para cada uma das categorias a prever.
- Cada subpasta contém os arquivos de imagem pertencentes à sua categoria.

Na estrutura de pastas ilustrada abaixo, o diretório de nível superior é *flower_photos*. Há cinco subdiretórios correspondentes às categorias que você deseja prever: Margarida, dandelion, rosas, flores e tulips. Cada um desses subdiretórios contém imagens que pertencem a sua respectiva categoria.

```text
\---flower_photos
    +---daisy
    |       100080576_f52e8ee070_n.jpg
    |       102841525_bd6628ae3c.jpg
    |       105806915_a9c13e2106_n.jpg
    |
    +---dandelion
    |       10443973_aeb97513fc_m.jpg
    |       10683189_bd6e371b97.jpg
    |       10919961_0af657c4e8.jpg
    |
    +---roses
    |       102501987_3cdb8e5394_n.jpg
    |       110472418_87b6a3aa98_m.jpg
    |       118974357_0faa23cce9_n.jpg
    |
    +---sunflowers
    |       127192624_afa3d9cb84.jpg
    |       145303599_2627e23815_n.jpg
    |       147804446_ef9244c8ce_m.jpg
    |
    \---tulips
            100930342_92e8746431_n.jpg
            107693873_86021ac4ea_n.jpg
            10791227_7168491604.jpg
```

## <a name="next-steps"></a>Próximas etapas

Siga estes tutoriais para criar aplicativos de Machine Learning com o construtor de modelos:

- [Prever os preços usando regressão](../tutorials/predict-prices-with-model-builder.md)
- [Analisar sentimentos em um aplicativo Web usando a classificação binária](../tutorials/sentiment-analysis-model-builder.md)

Se você estiver treinando um modelo usando código, [saiba como carregar dados usando a API ml.net](load-data-ml-net.md).
