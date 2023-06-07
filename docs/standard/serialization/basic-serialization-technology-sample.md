---
title: Exemplo de tecnologia de serialização básica
description: Este exemplo demonstra a capacidade do CLR de serializar um gráfico de objeto na memória para um fluxo. Este exemplo pode usar o SoapFormatter ou o BinaryFormatter.
ms.date: 03/30/2017
ms.assetid: 9d824e16-08d1-4a36-bc7f-2388c1f75f34
ms.openlocfilehash: 3f2273e6afb3a72f9734444ffe92d30871fb762b
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84276563"
---
# <a name="basic-serialization-technology-sample"></a>Exemplo de tecnologia de serialização básica

[Baixar exemplo](https://download.microsoft.com/download/4/7/B/47B2164C-E780-4B10-8DE4-2CB5B886E0A6/Technologies/Serialization/Runtime%20Serialization/Basic.zip.exe)

Esse exemplo demonstra a habilidade do common language runtime de serializar um grafo de objeto na memória para um fluxo. Esse exemplo pode usar o <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter> ou o <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> para serialização. Uma lista vinculada, preenchida com dados, é serializada ou desserializada para ou de um fluxo de arquivos. Em ambos os casos, a lista é exibida para que você possa ver os resultados. A lista vinculada é do tipo `LinkedList`, um tipo definido por essa amostra.

Revise os comentários no código de origem e nos arquivos build.proj para obter mais informações sobre serialização.

### <a name="to-build-the-sample-using-the-command-prompt"></a>Para criar o exemplo usando o Prompt de Comando

1. Navegue para um dos subdiretórios específicos da linguagem no diretório Technologies\Serialization\Runtime Serialization\Basic directory, usando o prompt de comando.

2. Digite **msbuild SerializationCS.sln**, **msbuild SerializationJSL.sln** ou **msbuild SerializationVB.sln**, dependendo de sua escolha da linguagem de programação, na linha de comando.

### <a name="to-build-the-sample-using-visual-studio"></a>Para criar o exemplo usando Visual Studio

1. Abra o explorador de arquivos e navegue até um dos subdiretórios específicos do idioma para o exemplo.

2. Clique duas vezes no ícone para o arquivo SerializationCS.sln, SerializationJSL.sln ou SerializationVB.sln, dependendo de sua escolha da linguagem de programação para abrir o arquivo no Visual Studio.

3. No menu **Compilar**, selecione **Compilar Solução**.

 O aplicativo de exemplo será criado no subdiretório padrão \bin ou \bin\Debug.

### <a name="to-run-the-sample"></a>Para executar a amostra

1. Navegue para o diretório contendo o executável compilado.

2. Digite **Serialization.exe**, junto com os valores de parâmetro desejados, na linha de comando.

  > [!NOTE]
  > Esse exemplo cria um aplicativo de console. Você deve lançá-lo usando o prompt de comando para exibir a saída.

## <a name="remarks"></a>Comentários

O aplicativo de exemplo aceita parâmetros da linha de comando indicando qual teste você gostaria de executar. Para serializar uma lista de 10 nós para um arquivo chamado **Test.xml** usando o formatador SOAP, use os parâmetros **sx Test.xml 10**.

Por exemplo:

```console
Serialize.exe -sx Test.xml 10
```

Para desserializar o arquivo **Test.xml** do exemplo anterior, use os parâmetros **dx Test.xml**.

Por exemplo:

```console
Serialize.exe -dx Test.xml
```

Nos dois exemplos acima, o "x" na opção de linha de comando indica que você quer a serialização SOAP de XML. Você pode usar "b" nesse local para usar a serialização binária. Se você quiser tentar a serialização com um número muito grande de nós, pode querer redirecionar a saída do console para um arquivo.

Por exemplo:

```console
Serialize.exe -sb Test.bin 10000 >somefile.txt
```

Os seguintes marcadores descrevem brevemente as classes e as tecnologias usadas por esse exemplo.

- Serialização em runtime

  - <xref:System.Runtime.Serialization.IFormatter>Usado para fazer referência a um <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> ou a um <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter> objeto.

  - <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>Usado para serializar uma lista vinculada em um fluxo em um formato binário. O formatador binário usa um formato que somente o tipo <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> entende. No entanto, os dados são concisos.

  - <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter>Usado para serializar uma lista vinculada em um fluxo no formato SOAP. SOAP é um formato padrão.

- E/S de fluxo

  - <xref:System.IO.Stream> Usado para serializar e desserializar. O tipo de fluxo específico usado nesse exemplo é o tipo <xref:System.IO.FileStream>. No entanto, a serialização pode ser usada com qualquer tipo derivado de <xref:System.IO.Stream>.

  - <xref:System.IO.File> Usado para criar objetos <xref:System.IO.FileStream> para ler e criar arquivos em disco.

  - <xref:System.IO.FileStream> Usado para serializar e desserializar listas vinculadas.

## <a name="see-also"></a>Confira também

- <xref:System.IO>
- <xref:System.IO.File>
- <xref:System.IO.FileStream>
- <xref:System.IO.Stream>
- <xref:System.Random>
- <xref:System.Runtime.Serialization>
- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>
- <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter>
- <xref:System.Runtime.Serialization.IFormatter>
- <xref:System.SerializableAttribute>
- <xref:System.Xml.Serialization>
- [Serialização básica](basic-serialization.md)
- [Serialização binária](binary-serialization.md)
- [Controlando a serialização XML usando atributos](controlling-xml-serialization-using-attributes.md)
- [Apresentando a serialização XML](introducing-xml-serialization.md)
- [Serialização](index.md)
- [Serialização de XML e SOAP](xml-and-soap-serialization.md)
