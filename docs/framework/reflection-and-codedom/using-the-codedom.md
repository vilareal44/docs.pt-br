---
title: Usando o CodeDOM
description: Use o Modelo de Objeto do Documento de Código (CodeDOM), que fornece tipos que representam muitos tipos comuns de elementos de código-fonte, para montar um grafo de objeto.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- code compilers
- Code Document Object Model
- Code Document Object Model, graphs
- types, CodeDOM
- namespaces [.NET Framework], CodeDOM
- templated code generation
- dynamically representing source code
- source code models
- CodeDOM
- graphing with CodeDOM
- dynamic compilation
- code generators
- CodeDOM, graphs
ms.assetid: 0444ddf3-c3f6-44ed-a999-f710d9c3e0cf
ms.openlocfilehash: 476d8c18f386f889855c664147b1ee20995dc6f9
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86865210"
---
# <a name="using-the-codedom"></a>Usando o CodeDOM
O CodeDOM fornece tipos que representam muitos dos tipos mais comuns de elementos de código-fonte. Você pode criar um programa que cria um modelo de código-fonte usando elementos do CodeDOM para montar um grafo de objeto. Este grafo de objeto pode ser renderizado como código-fonte usando um gerador de código CodeDOM para uma linguagem de programação com suporte. O CodeDOM também pode ser usado para compilar o código-fonte em um assembly binário.  
  
 Alguns usos comuns do CodeDOM incluem:  
  
- Geração de código de modelo: gerar código para ASP.NET, proxies de cliente de serviços Web XML, assistentes de código, designers ou outros mecanismos de emissão de código.  
  
- Compilação dinâmica: suporte a compilação de código em uma ou várias linguagens.  
  
## <a name="building-a-codedom-graph"></a>Compilando um gráfico CodeDOM  
 O namespace <xref:System.CodeDom> fornece classes para representar a estrutura lógica do código-fonte, independente da sintaxe da linguagem.  
  
### <a name="the-structure-of-a-codedom-graph"></a>A estrutura de um gráfico CodeDOM  
 A estrutura de um grafo CodeDOM é como uma árvore de contêineres. O contêiner mais alto de cada grafo CodeDOM compilável, a raiz, é um <xref:System.CodeDom.CodeCompileUnit>. Cada elemento de seu modelo de código-fonte deve ser vinculado ao grafo por meio de uma propriedade de um <xref:System.CodeDom.CodeObject> no grafo.  
  
### <a name="building-a-source-code-model-for-a-sample-hello-world-program"></a>Criando um modelo de código-fonte para um programa Olá, Mundo de exemplo  
 A instrução passo a passo a seguir fornece um exemplo de como criar um grafo de objeto CodeDOM que representa o código de um aplicativo Olá, Mundo simples. Para ver o código-fonte completo deste exemplo de código, consulte o tópico <xref:System.CodeDom.Compiler.CodeDomProvider?displayProperty=nameWithType>.  
  
#### <a name="creating-a-compile-unit"></a>Criando uma unidade de compilação  
 O CodeDOM define um objeto chamado de <xref:System.CodeDom.CodeCompileUnit>, que pode fazer referência a um grafo de objeto do CodeDOM que modela o código-fonte a ser compilado. Um **CodeCompileUnit** tem propriedades para armazenar referências aos atributos, namespaces e assemblies.  
  
 Os provedores do CodeDom que derivam da classe <xref:System.CodeDom.Compiler.CodeDomProvider> contém métodos que processam o grafo do objeto referenciado por um **CodeCompileUnit**.  
  
 Para criar um grafo de objeto para um aplicativo simples, você deve montar o modelo de código-fonte e referenciá-lo de um **CodeCompileUnit**.  
  
 Você pode criar uma nova unidade de compilação com a sintaxe mostrada neste exemplo:  
  
 [!code-cpp[CodeDomExample#12](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source2.cpp#12)]
 [!code-csharp[CodeDomExample#12](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source2.cs#12)]
 [!code-vb[CodeDomExample#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source2.vb#12)]  
  
 Um <xref:System.CodeDom.CodeSnippetCompileUnit> pode conter uma seção de código-fonte que já está na linguagem de destino, mas não pode ser renderizado para outra linguagem.  
  
#### <a name="defining-a-namespace"></a>Definindo um namespace  
 Para definir um namespace, crie um <xref:System.CodeDom.CodeNamespace> e atribua um nome para ele usando o construtor apropriado ou definindo sua propriedade **Nome**.  
  
 [!code-cpp[CodeDomExample#13](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source2.cpp#13)]
 [!code-csharp[CodeDomExample#13](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source2.cs#13)]
 [!code-vb[CodeDomExample#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source2.vb#13)]  
  
#### <a name="importing-a-namespace"></a>Importando um namespace  
 Para adicionar uma diretiva de importação do namespace ao namespace, adicione um <xref:System.CodeDom.CodeNamespaceImport> que indica o namespace a ser importado para a coleção **CodeNamespace.Imports**.  
  
 O código a seguir adiciona uma importação para o namespace **System** à coleção **Importações** de um **CodeNamespace** chamado `samples`:  
  
 [!code-cpp[CodeDomExample#14](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source2.cpp#14)]
 [!code-csharp[CodeDomExample#14](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source2.cs#14)]
 [!code-vb[CodeDomExample#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source2.vb#14)]  
  
#### <a name="linking-code-elements-into-the-object-graph"></a>Vincular elementos de código ao grafo do objeto  
 Todos os elementos de código que formam um grafo CodeDOM devem ser vinculados ao <xref:System.CodeDom.CodeCompileUnit>, que é o elemento raiz da árvore por uma série de referências entre os elementos referenciados diretamente das propriedades do objeto raiz do grafo. Defina um objeto como uma propriedade de um objeto de contêiner para estabelecer uma referência do objeto de contêiner.  
  
 A instrução a seguir adiciona o `samples` **CodeNamespace** à propriedade da coleção **Namespaces** da raiz **CodeCompileUnit**.  
  
 [!code-cpp[CodeDomExample#15](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source2.cpp#15)]
 [!code-csharp[CodeDomExample#15](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source2.cs#15)]
 [!code-vb[CodeDomExample#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source2.vb#15)]  
  
#### <a name="defining-a-type"></a>Definindo um tipo  
 Para declarar uma classe, estrutura, interface ou enumeração usando o CodeDOM, crie um novo <xref:System.CodeDom.CodeTypeDeclaration> e atribua um nome. O exemplo a seguir demonstra isso usando uma sobrecarga de construtor para definir a propriedade **Nome**:  
  
 [!code-cpp[CodeDomExample#16](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source2.cpp#16)]
 [!code-csharp[CodeDomExample#16](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source2.cs#16)]
 [!code-vb[CodeDomExample#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source2.vb#16)]  
  
 Para adicionar um tipo a um namespace, adicione um <xref:System.CodeDom.CodeTypeDeclaration> que representa o tipo a ser adicionado para o namespace à coleção **Tipos** de um **CodeNamespace**.  
  
 O exemplo a seguir demonstra como adicionar uma classe denominada `class1` para um **CodeNamespace** chamado `samples`:  
  
 [!code-cpp[CodeDomExample#17](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source2.cpp#17)]
 [!code-csharp[CodeDomExample#17](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source2.cs#17)]
 [!code-vb[CodeDomExample#17](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source2.vb#17)]  
  
#### <a name="adding-class-members-to-a-class"></a>Adicionar membros de classe a uma classe  
 O namespace <xref:System.CodeDom> fornece uma variedade de elementos que podem ser usados para representar os membros de classe. Cada membro de classe pode ser adicionado à coleção **Membros** de um <xref:System.CodeDom.CodeTypeDeclaration>.  
  
#### <a name="defining-a-code-entry-point-method-for-an-executable"></a>Definindo um método de ponto de entrada de código para um executável  
 Se você estiver compilando código para um programa executável, será necessário indicar o ponto de entrada de um programa criando um <xref:System.CodeDom.CodeEntryPointMethod> para representar o método no qual a execução do programa deve começar.  
  
 O exemplo a seguir demonstra como definir um método de ponto de entrada que contém um <xref:System.CodeDom.CodeMethodInvokeExpression> que chama **System.Console.WriteLine** para imprimir “Hello World!”:  
  
 [!code-cpp[CodeDomExample#18](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source2.cpp#18)]
 [!code-csharp[CodeDomExample#18](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source2.cs#18)]
 [!code-vb[CodeDomExample#18](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source2.vb#18)]  
  
 A instrução a seguir adiciona o método de ponto de entrada denominado `Start` à coleção **Membros** de `class1`:  
  
 [!code-cpp[CodeDomExample#19](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source2.cpp#19)]
 [!code-csharp[CodeDomExample#19](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source2.cs#19)]
 [!code-vb[CodeDomExample#19](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source2.vb#19)]  
  
 Agora o <xref:System.CodeDom.CodeCompileUnit> chamado `compileUnit` contém o grafo CodeDOM para um programa Olá, Mundo simples. Para obter informações sobre como gerar e compilar o código de um grafo CodeDOM, consulte [Gerando código-fonte e compilando um programa de um grafo CodeDOM](generating-and-compiling-source-code-from-a-codedom-graph.md).  
  
### <a name="more-information-on-building-a-codedom-graph"></a>Mais informações sobre a compilação de um grafo CodeDOM  
 O CodeDOM dá suporte a muitos tipos comuns de elementos de código encontrados em linguagens de programação que dão suporte ao Common Language Runtime. O CodeDOM não foi projetado para fornecer elementos para representar todos os recursos de linguagem de programação possíveis. Código que não pode ser representado facilmente com elementos do CodeDOM pode ser encapsulado em um <xref:System.CodeDom.CodeSnippetExpression>, <xref:System.CodeDom.CodeSnippetStatement>, <xref:System.CodeDom.CodeSnippetTypeMember> ou <xref:System.CodeDom.CodeSnippetCompileUnit>. No entanto, snippets não podem ser convertidos para outras linguagens automaticamente pelo CodeDOM.  
  
 Para obter a documentação para cada um dos tipos do CodeDOM, consulte a documentação de referência para o namespace <xref:System.CodeDom>.  
  
 Para uma verificação rápida para localizar o elemento de CodeDOM que representa um tipo específico de elemento de código, consulte a [Referência rápida do CodeDOM](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/f1dfsbhc(v=vs.100)).
