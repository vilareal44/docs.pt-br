---
title: Como criar uma classe usando o CodeDOM
description: Consulte um exemplo detalhado que explica como criar uma classe usando o Modelo de Objeto do Documento de Código (CodeDOM).
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Code Document Object Model, graphs
- Code Document Object Model, creating classes
- graphing with CodeDOM
- CodeDOM, creating classes
- CodeDOM, graphs
ms.assetid: 0ceb70fe-36e1-49bb-922b-e9f615c20a14
ms.openlocfilehash: 3d7151d384402dba6fbb5da8fe54621346251f7b
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86865301"
---
# <a name="how-to-create-a-class-using-codedom"></a>Como criar uma classe usando o CodeDOM
Os procedimentos a seguir ilustram como criar e compilar um grafo CodeDOM que gera uma classe que contém dois campos, três propriedades, um método, um construtor e um ponto de entrada.  
  
1. Crie um aplicativo de console que usará o código CodeDOM para gerar o código-fonte para uma classe.  
  
     Neste exemplo, a classe de geração é nomeada `Sample` e o código gerado é uma classe chamada `CodeDOMCreatedClass` em um arquivo chamado SampleCode.  
  
2. Na classe de geração, inicialize o grafo CodeDOM e use os métodos CodeCOM para definir os membros, o construtor e o ponto de entrada (método `Main`) da classe gerada.  
  
     Neste exemplo, a classe gerada tem dois campos, três propriedades, um construtor, um método e um método `Main`.  
  
3. Na classe de geração, crie um provedor de código de específico da linguagem e chame seu método <xref:System.CodeDom.Compiler.CodeDomProvider.GenerateCodeFromCompileUnit%2A> para gerar o código do grafo.  
  
4. Compile e execute o aplicativo para gerar o código.  
  
     Neste exemplo, o código gerado está em um arquivo chamado SampleCode. Compilar e executar esse código para ver a saída de exemplo.  
  
### <a name="to-create-the-application-that-will-execute-the-codedom-code"></a>Para criar o aplicativo que executará o código CodeDOM  
  
- Crie uma classe de aplicativo de console para conter o código CodeDOM. Defina os campos globais que devem ser usados na classe para referenciar o assembly (<xref:System.CodeDom.CodeCompileUnit>) e classes (<xref:System.CodeDom.CodeTypeDeclaration>), especifique o nome do arquivo de origem gerado e declare o método `Main`.  
  
     [!code-csharp[CodeDOM Class Sample Main#1](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample Main/CS/program.cs#1)]
     [!code-vb[CodeDOM Class Sample Main#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample Main/VB/program.vb#1)]  
  
### <a name="to-initialize-the-codedom-graph"></a>Para inicializar o grafo CodeDOM  
  
- No construtor para a classe de aplicativo de console, inicialize o assembly e a classe e adicione as declarações apropriadas para o grafo CodeDOM.  
  
     [!code-csharp[CodeDOM Class Sample#2](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#2)]
     [!code-vb[CodeDOM Class Sample#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#2)]  
  
### <a name="to-add-members-to-the-codedom-graph"></a>Para adicionar membros ao grafo CodeDOM  
  
- Adicione campos ao grafo CodeDOM adicionando objetos <xref:System.CodeDom.CodeMemberField> para a propriedade <xref:System.CodeDom.CodeTypeDeclaration.Members%2A> da classe.  
  
     [!code-csharp[CodeDOM Class Sample#3](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#3)]
     [!code-vb[CodeDOM Class Sample#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#3)]  
  
- Adicione propriedades ao grafo CodeDOM adicionando objetos <xref:System.CodeDom.CodeMemberProperty> à propriedade <xref:System.CodeDom.CodeTypeDeclaration.Members%2A> da classe.  
  
     [!code-csharp[CodeDOM Class Sample#4](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#4)]
     [!code-vb[CodeDOM Class Sample#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#4)]  
  
- Adicione um método ao grafo CodeDOM adicionando um objeto <xref:System.CodeDom.CodeMemberMethod> à propriedade <xref:System.CodeDom.CodeTypeDeclaration.Members%2A> da classe.  
  
     [!code-csharp[CodeDOM Class Sample#5](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#5)]
     [!code-vb[CodeDOM Class Sample#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#5)]  
  
- Adicione um construtor ao grafo CodeDOM adicionando um objeto <xref:System.CodeDom.CodeConstructor> à propriedade <xref:System.CodeDom.CodeTypeDeclaration.Members%2A> da classe.  
  
     [!code-csharp[CodeDOM Class Sample#6](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#6)]
     [!code-vb[CodeDOM Class Sample#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#6)]  
  
- Adicione um ponto de entrada ao grafo CodeDOM adicionando um objeto <xref:System.CodeDom.CodeEntryPointMethod> à propriedade <xref:System.CodeDom.CodeTypeDeclaration.Members%2A> da classe.  
  
     [!code-csharp[CodeDOM Class Sample#7](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#7)]
     [!code-vb[CodeDOM Class Sample#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#7)]  
  
### <a name="to-generate-the-code-from-the-codedom-graph"></a>Para gerar o código do grafo CodeDOM  
  
- Gere o código-fonte do grafo CodeDOM chamando o método <xref:System.CodeDom.Compiler.CodeDomProvider.GenerateCodeFromCompileUnit%2A>.  
  
     [!code-csharp[CodeDOM Class Sample#8](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#8)]
     [!code-vb[CodeDOM Class Sample#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#8)]  
  
### <a name="to-create-the-graph-and-generate-the-code"></a>Para criar o gráfico e gerar o código  
  
1. Adicione os métodos criados nas etapas anteriores para o método `Main` definido na primeira etapa.  
  
     [!code-csharp[CodeDOM Class Sample#9](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#9)]
     [!code-vb[CodeDOM Class Sample#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#9)]  
  
2. Compile e execute a classe de geração.  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir mostra o código das etapas anteriores.  
  
 [!code-csharp[CodeDOM Class Sample#1](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#1)]
 [!code-vb[CodeDOM Class Sample#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#1)]  
  
 Quando o exemplo anterior é compilado e executado, ele produz o seguinte código-fonte.  
  
 [!code-csharp[CodeDOM Class Sample#99](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/SampleCode.cs#99)]
 [!code-vb[CodeDOM Class Sample#99](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/SampleCode.vb#99)]  
  
 O código-fonte gerado produz a seguinte saída quando compilado e executado.  
  
```output
The object:  
 width = 5.3,  
 height = 6.9,  
 area = 36.57  
```  
  
## <a name="compiling-the-code"></a>Compilando o código  
  
- Este exemplo de código requer a permissão `FullTrust` definida para ser executado com êxito.  
  
## <a name="see-also"></a>Veja também

- [Usando o CodeDOM](using-the-codedom.md)
- [Gerando e compilando código-fonte de um gráfico CodeDOM](generating-and-compiling-source-code-from-a-codedom-graph.md)
