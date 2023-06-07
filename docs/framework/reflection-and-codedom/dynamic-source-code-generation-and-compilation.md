---
title: Geração e compilação de código-fonte dinâmico
description: Compile e gere código-fonte dinâmico no .NET com o Modelo de Objeto do Documento de Código (CodeDOM). Os elementos CodeDOM são vinculados para formar um grafo do CodeDOM.
ms.date: 03/30/2017
helpviewer_keywords:
- Code Document Object Model
- System.CodeDom namespace
- language-independent source code modeling
- CodeDOM
- multiple languages supported by CodeDOM
- source code in multiple languages
- languages, multiple language support by CodeDOM
ms.assetid: d077a3e8-bd81-4bdf-b6a3-323857ea30fb
ms.openlocfilehash: 3cdd89ac9745f6af133ca683afff64283f2727d1
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475093"
---
# <a name="compile-and-generate-dynamic-source-code"></a>Compilar e gerar código-fonte dinâmico

O .NET Framework inclui um mecanismo chamado CodeDOM (Modelo de Objeto do Documento de Código) que permite aos desenvolvedores de programas que emitem o código-fonte gerar o código-fonte em várias linguagens de programação no tempo de execução com base em um único modelo que representa o código a ser renderizado.  
  
Para representar o código-fonte, os elementos do CodeDOM são vinculados uns aos outros para formar uma estrutura de dados conhecida como um grafo CodeDOM, que modela a estrutura de parte do código-fonte.  
  
O namespace <xref:System.CodeDom?displayProperty=fullName> define tipos que podem representam a estrutura lógica do código-fonte, independente de uma linguagem de programação específica. O namespace <xref:System.CodeDom.Compiler?displayProperty=fullName> define tipos para gerar código-fonte de grafos CodeDOM e gerenciar a compilação do código-fonte em linguagens com suporte. Fornecedores de compilador ou desenvolvedores podem estender o conjunto de idiomas com suporte.  
  
A modelagem de código-fonte independente de linguagem pode ser útil quando um programa precisa gerar código-fonte para um modelo de programa em várias linguagens ou para uma linguagem de destino incerta. Por exemplo, alguns designers usam CodeDOM como uma interface da abstração de linguagem para produzir código-fonte na linguagem de programação correta, caso haja suporte para a linguagem no CodeDOM.  
  
O .NET Framework inclui geradores e compiladores de código para <xref:Microsoft.CSharp.CSharpCodeProvider>, <xref:Microsoft.JScript.JScriptCodeProvider> e <xref:Microsoft.VisualBasic.VBCodeProvider>.  
  
## <a name="in-this-section"></a>Nesta seção

- [Usando o CodeDOM](using-the-codedom.md)

  Descreve os usos comuns e demonstra como criar um grafo de objeto simples usando o CodeDOM.  
  
- [Gerar código-fonte e compilar um programa de um grafo do CodeDOM](generating-and-compiling-source-code-from-a-codedom-graph.md)  

  Descreve como gerar o código-fonte e compilar o código gerado com um compilador externo usando classes definidas no namespace `System.CodeDom.Compiler`.  
  
- [Como criar um arquivo de documentação XML usando CodeDOM](how-to-create-an-xml-documentation-file-using-codedom.md)  

  Descreve como usar CodeDOM para gerar código com comentários de documentação XML e compilar o código gerado para que ele cria a saída de documentação XML.  
  
- [Como criar uma classe usando o CodeDOM](how-to-create-a-class-using-codedom.md)  

  Descreve como usar CodeDOM para gerar uma classe que contém campos, propriedades, um método, um construtor e um ponto de entrada.  
  
## <a name="reference"></a>Referência  

- <xref:System.CodeDom>  

  Define os elementos que representam os elementos de código em linguagens de programação que usam o Common Language Runtime como destino.  
  
- <xref:System.CodeDom.Compiler>  

  Define as interfaces para gerar e compilar o código no tempo de execução.  
  
## <a name="related-sections"></a>Seções relacionadas  

- A [referência rápida do CodeDom](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/f1dfsbhc(v=vs.100)) fornece uma maneira rápida para os desenvolvedores encontrarem os elementos CodeDOM que representam elementos de código-fonte.
