---
title: Gerando e compilando código-fonte de um gráfico CodeDOM
description: Gere e compile o código-fonte de um grafo do CodeDOM no .NET. Use um provedor de código do CodeDOM para gerar código-fonte e compilar assemblies.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- code compilers
- CodeDOM, generating source code
- Code Document Object Model, graphs
- templated code generation
- source code, generating
- dynamically representing source code
- generating CodeDOM graphs
- Code Document Object Model, generating source code
- translating language to language
- compiling assemblies
- generating source code in multiple languages
- graphing with CodeDOM
- dynamic compilation
- assemblies [.NET Framework], CodeDOM
- source code generation
- outputting source code by CodeDOM
- code generators
- compiling source code, multiple languages
- CodeDOM, graphs
ms.assetid: 6c864c8e-6dd3-4a65-ace0-36879d9a9c42
ms.openlocfilehash: 85654fe961f01ad7b8fb886d59a3de9ab0efe7aa
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474040"
---
# <a name="generating-and-compiling-source-code-from-a-codedom-graph"></a>Gerando e compilando código-fonte de um gráfico CodeDOM
O namespace <xref:System.CodeDom.Compiler> fornece interfaces para geração de código-fonte de grafos de objeto CodeDOM e para o gerenciamento da compilação com compiladores com suporte. Um provedor de código pode produzir código-fonte em uma linguagem de programação específica acordo com um grafo CodeDOM. Uma classe que deriva de <xref:System.CodeDom.Compiler.CodeDomProvider> normalmente pode fornecer métodos para gerar e compilar o código para a linguagem à qual o provedor dá suporte.  
  
## <a name="using-a-codedom-code-provider-to-generate-source-code"></a>Usando um provedor de código CodeDOM para gerar código-fonte  
 Para gerar o código-fonte em uma determinada linguagem, você precisa de um grafo CodeDOM que representa a estrutura do código-fonte a ser gerada.  
  
 O exemplo a seguir demonstra como criar uma instância de um <xref:Microsoft.CSharp.CSharpCodeProvider>:  
  
 [!code-cpp[CodeDomExample#21](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source3.cpp#21)]
 [!code-csharp[CodeDomExample#21](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source3.cs#21)]
 [!code-vb[CodeDomExample#21](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source3.vb#21)]  
  
 O gráfico para geração de código geralmente está contido em um <xref:System.CodeDom.CodeCompileUnit>. Para gerar o código para uma **CodeCompileUnit** que contém um grafo CodeDOM, chame o método <xref:System.CodeDom.Compiler.CodeDomProvider.GenerateCodeFromCompileUnit%2A> do provedor de código. Esse método tem um parâmetro para um <xref:System.IO.TextWriter> que ele usa para gerar o código-fonte, portanto, às vezes é necessário criar primeiro um **TextWriter** que pode ser gravado. O exemplo a seguir demonstra o código de geração de um **CodeCompileUnit** e a gravação do código-fonte gerado em um arquivo chamado HelloWorld.cs.  
  
 [!code-cpp[CodeDomExample#22](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source3.cpp#22)]
 [!code-csharp[CodeDomExample#22](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source3.cs#22)]
 [!code-vb[CodeDomExample#22](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source3.vb#22)]  
  
## <a name="using-a-codedom-code-provider-to-compile-assemblies"></a>Usando um provedor de código CodeDOM para compilar assemblies  
 **Invocando a compilação**  
  
 Para compilar um assembly usando um provedor de CodeDom, você deve ter o código-fonte para compilar em uma linguagem para a qual tenha um compilador ou um grafo CodeDOM do qual o código-fonte pode ser gerado.  
  
 Se você estiver compilando de um grafo CodeDOM, passe o <xref:System.CodeDom.CodeCompileUnit> que contém o grafo para o método <xref:System.CodeDom.Compiler.CodeDomProvider.CompileAssemblyFromDom%2A> do provedor de código. Se você tiver um arquivo de código-fonte em uma linguagem que o compilador compreende, passe o nome do arquivo que contém o código-fonte para o método <xref:System.CodeDom.Compiler.CodeDomProvider.CompileAssemblyFromFile%2A> do provedor de CodeDom. Você também pode passar uma cadeia de caracteres que contém o código-fonte em uma linguagem que o compilador compreende para o método <xref:System.CodeDom.Compiler.CodeDomProvider.CompileAssemblyFromSource%2A> do provedor de CodeDom.  
  
 **Configurando parâmetros de compilação**  
  
 Todos os métodos padrão de invocar a compilação de um provedor de CodeDom têm um parâmetro do tipo <xref:System.CodeDom.Compiler.CompilerParameters> que indica as opções a serem usadas para compilação.  
  
 Você pode especificar um nome de arquivo do assembly de saída na propriedade <xref:System.CodeDom.Compiler.CompilerParameters.OutputAssembly%2A> de **CompilerParameters**. Caso contrário, um nome de arquivo de saída padrão será usado.  
  
 Por padrão, um novo **CompilerParameters** é inicializado com sua propriedade <xref:System.CodeDom.Compiler.CompilerParameters.GenerateExecutable%2A> definida como **false**. Se estiver compilando um programa executável, você deverá definir a propriedade **GenerateExecutable** como **true**. Quando o **GenerateExecutable** estiver definido como **false**, o compilador gerará uma biblioteca de classes.  
  
 Se você estiver compilando um executável de um grafo CodeDOM, um <xref:System.CodeDom.CodeEntryPointMethod> deverá ser definido no grafo. Se houver vários pontos de entrada de código, poderá ser necessário definir a propriedade <xref:System.CodeDom.Compiler.CompilerParameters.MainClass%2A> de **CompilerParameters** para o nome da classe que define o ponto de entrada a ser usado.  
  
 Para incluir informações de depuração em um executável gerado, defina a propriedade <xref:System.CodeDom.Compiler.CompilerParameters.IncludeDebugInformation%2A> como **true**.  
  
 Se seu projeto fizer referência a quaisquer assemblies, você precisará especificar os nomes de assembly como itens em uma <xref:System.Collections.Specialized.StringCollection> como a propriedade <xref:System.CodeDom.Compiler.CompilerParameters.ReferencedAssemblies%2A> de **CompilerParameters** usado ao invocar a compilação.  
  
 Você pode compilar um assembly que é gravado na memória e não no disco definindo a propriedade <xref:System.CodeDom.Compiler.CompilerParameters.GenerateInMemory%2A> como **true**. Quando um assembly é gerado na memória, seu código pode obter uma referência para o assembly gerado da propriedade <xref:System.CodeDom.Compiler.CompilerResults.CompiledAssembly%2A> de um <xref:System.CodeDom.Compiler.CompilerResults>. Se um assembly estiver gravado em disco, você poderá obter o caminho para o assembly gerado na propriedade <xref:System.CodeDom.Compiler.CompilerResults.PathToAssembly%2A> de um **CompilerResults**.  
  
 Para especificar uma cadeia de caracteres de argumentos de linha de comando personalizados a ser usada ao invocar o processo de compilação, defina a cadeia de caracteres na propriedade <xref:System.CodeDom.Compiler.CompilerParameters.CompilerOptions%2A>.  
  
 Se um token de segurança do Win32 for necessário para invocar o processo do compilador, especifique o token na propriedade <xref:System.CodeDom.Compiler.CompilerParameters.UserToken%2A>.  
  
 Para vincular um arquivo de recurso Win32 no assembly compilado, especifique o nome do arquivo de recurso Win32 na propriedade <xref:System.CodeDom.Compiler.CompilerParameters.Win32Resource%2A>.  
  
 Para especificar um nível de aviso no qual interromper a compilação, defina a propriedade <xref:System.CodeDom.Compiler.CompilerParameters.WarningLevel%2A> como um inteiro que representa o nível de aviso no qual interromper a compilação. Você também pode configurar o compilador para interromper a compilação se forem encontrados avisos definindo a propriedade <xref:System.CodeDom.Compiler.CompilerParameters.TreatWarningsAsErrors%2A> como **true**.  
  
 O exemplo de código a seguir demonstra a compilação de um arquivo de origem usando um provedor CodeDom derivado da classe <xref:System.CodeDom.Compiler.CodeDomProvider>.  
  
 [!code-cpp[CodeDomExample#23](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source3.cpp#23)]
 [!code-csharp[CodeDomExample#23](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source3.cs#23)]
 [!code-vb[CodeDomExample#23](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source3.vb#23)]  
  
## <a name="languages-with-initial-support"></a>Linguagens com suporte inicial  
 O .NET Framework fornece compiladores de código e geradores de código para as seguintes linguagens: C#, Visual Basic, C++ e JScript. O suporte do CodeDOM pode ser estendido a outras linguagens implementando geradores de código e compiladores de código específicos da linguagem.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.CodeDom>
- <xref:System.CodeDom.Compiler>
- [Geração e compilação de código-fonte dinâmico](dynamic-source-code-generation-and-compilation.md)
- [Referência rápida do CodeDOM](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/f1dfsbhc(v=vs.100))
