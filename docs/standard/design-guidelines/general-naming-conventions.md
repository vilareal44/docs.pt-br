---
title: Convenções de nomenclatura gerais
description: Use convenções de nomenclatura gerais relacionadas à escolha do Word, diretrizes sobre como usar abreviações e acrônimos e orientação sobre como evitar nomes específicos à linguagem.
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- names [.NET Framework], conflicts
- type names, conflicts
- language-specific type names
- names [.NET Framework], about naming guidelines
- names [.NET Framework], abbreviations
- abbreviation naming guidelines
- acronym naming guidelines
- Hungarian notation
- names [.NET Framework], type names
- names [.NET Framework], acronyms
ms.assetid: d3a77ea1-75d2-4969-a8c3-3e1e3e1aaedc
ms.openlocfilehash: b7f06a57c57800afcfa7febf9452094b4ad5ddc1
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84769074"
---
# <a name="general-naming-conventions"></a>Convenções de nomenclatura gerais

Esta seção descreve as convenções de nomenclatura gerais relacionadas à escolha do Word, diretrizes sobre como usar abreviações e acrônimos e recomendações sobre como evitar o uso de nomes específicos à linguagem.

## <a name="word-choice"></a>Opção de palavra
 ✔️ Escolha nomes de identificadores facilmente legíveis.

 Por exemplo, uma propriedade chamada `HorizontalAlignment` é mais legível do que `AlignmentHorizontal` .

 ✔️ favorecer a legibilidade em breve.

 O nome da propriedade `CanScrollHorizontally` é melhor do que `ScrollableX` (uma referência obscura para o eixo X).

 ❌Não use sublinhados, hifens ou quaisquer outros caracteres não alfanuméricos.

 ❌Não use a notação húngara.

 ❌Evite usar identificadores que entrem em conflito com palavras-chave de linguagens de programação amplamente usadas.

 De acordo com a regra 4 do Common Language Specification (CLS), todos os idiomas compatíveis devem fornecer um mecanismo que permita o acesso a itens nomeados que usam uma palavra-chave desse idioma como um identificador. O C#, por exemplo, usa o sinal @ como um mecanismo de escape nesse caso. No entanto, ainda é uma boa ideia evitar palavras-chave comuns, pois é muito mais difícil usar um método com a sequência de escape de um sem ele.

## <a name="using-abbreviations-and-acronyms"></a>Usando abreviações e acrônimos
 ❌Não use abreviações ou contratações como parte dos nomes de identificador.

 Por exemplo, use `GetWindow` em vez de `GetWin` .

 ❌Não use nenhum acrônimo que não seja amplamente aceito e, mesmo que eles estejam, somente quando necessário.

## <a name="avoiding-language-specific-names"></a>Evitando nomes específicos de idioma
 ✔️ usam nomes semanticamente interessantes em vez de palavras-chave específicas de idioma para nomes de tipos.

 Por exemplo, `GetLength` é um nome melhor do que `GetInt` .

 ✔️ usar um nome de tipo CLR genérico, em vez de um nome específico de idioma, nos casos raros em que um identificador não tem um significado semântico além de seu tipo.

 Por exemplo, um método que converte para <xref:System.Int64> deve ser nomeado `ToInt64` , não `ToLong` (porque <xref:System.Int64> é um nome CLR para o alias específico do C# `long` ). A tabela a seguir apresenta vários tipos de dados base usando os nomes de tipo CLR (bem como os nomes de tipos correspondentes para C#, Visual Basic e C++).

|C#|Visual Basic|C++|CLR|
|---------|------------------|-----------|---------|
|**sbyte**|**SByte**|**char**|**SByte**|
|**byte**|**Minuciosa**|**unsigned char**|**Minuciosa**|
|**short**|**Baixo**|**short**|**Int16**|
|**ushort**|**UInt16**|**unsigned short**|**UInt16**|
|**int**|**Integer**|**int**|**Int32**|
|**uint**|**UInt32**|**unsigned int**|**UInt32**|
|**long**|**Long**|**__int64**|**Int64**|
|**ULONG**|**UInt64**|**unsigned __int64**|**UInt64**|
|**float**|**Single**|**float**|**Single**|
|**double**|**Double**|**double**|**Double**|
|**bool**|**Boolean**|**bool**|**Boolean**|
|**char**|**º**|**wchar_t**|**º**|
|**cadeia de caracteres**|**Cadeia de caracteres**|**Cadeia de caracteres**|**Cadeia de caracteres**|
|**object**|**Objeto**|**Objeto**|**Objeto**|

 ✔️ usar um nome comum, como `value` ou `item` , em vez de repetir o nome do tipo, nos casos raros em que um identificador não tem significado semântico e o tipo do parâmetro não é importante.

## <a name="naming-new-versions-of-existing-apis"></a>Nomeando novas versões de APIs existentes
 ✔️ usar um nome semelhante à antiga API ao criar novas versões de uma API existente.

 Isso ajuda a destacar a relação entre as APIs.

 ✔️ preferir adicionar um sufixo em vez de um prefixo para indicar uma nova versão de uma API existente.

 Isso auxiliará a descoberta ao procurar a documentação ou usar o IntelliSense. A versão antiga da API será organizada perto das novas APIs, pois a maioria dos navegadores e o IntelliSense mostram identificadores em ordem alfabética.

 ✔️ Considere usar um identificador totalmente novo, mas significativo, em vez de adicionar um sufixo ou um prefixo.

 ✔️ usar um sufixo numérico para indicar uma nova versão de uma API existente, especialmente se o nome existente da API for o único nome que faz sentido (ou seja, se for um padrão da indústria) e se adicionar qualquer sufixo significativo (ou alterar o nome) não for uma opção apropriada.

 ❌Não use o sufixo "ex" (ou um semelhante) para um identificador para distingui-lo de uma versão anterior da mesma API.

 ✔️ Use o sufixo "64" ao introduzir versões de APIs que operam em um inteiro de 64 bits (um inteiro longo) em vez de um inteiro de 32 bits. Você só precisa tomar essa abordagem quando existir a API de 32 bits existente; Não faça isso para APIs totalmente novas com uma versão de 64 bits.

 *Partes &copy; 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Veja também

- [Diretrizes de design de estrutura](index.md)
- [Diretrizes de nomenclatura](naming-guidelines.md)
- [Convenções de nomenclatura .NET para EditorConfig](/visualstudio/ide/editorconfig-naming-conventions)
