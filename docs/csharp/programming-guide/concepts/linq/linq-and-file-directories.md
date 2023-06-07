---
title: LINQ e diretórios de arquivos (C#)
description: Esses recursos LINQ do C# para operações do sistema de arquivos não são usados para alterar o conteúdo dos arquivos ou pastas.
ms.date: 07/20/2015
ms.assetid: b66c55e4-0f72-44e5-b086-519f9962335c
ms.openlocfilehash: ac00e29f90ee1c04ab9978b6ada3ae5f28991a1c
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87165666"
---
# <a name="linq-and-file-directories-c"></a>LINQ e diretórios de arquivos (C#)

Muitas operações do sistema de arquivos são basicamente consultas e, portanto, são bem adequadas para a abordagem do LINQ.  
  
 As consultas nesta seção não são destrutivas. Elas não são usadas para alterar o conteúdo dos arquivos ou pastas originais. Isso segue a regra de que consultas não devem causar efeitos colaterais. Em geral, qualquer código (incluindo consultas que executam operadores criar / atualizar / excluir) que modifique dados de origem deve ser mantido separado do código que apenas consulta os dados.  
  
 Esta seção contém os seguintes tópicos:  
  
 [Como consultar arquivos com um atributo ou nome especificado (C#)](./how-to-query-for-files-with-a-specified-attribute-or-name.md)\
 Mostra como pesquisar arquivos, examinando uma ou mais propriedades de seu objeto <xref:System.IO.FileInfo>.  
  
 [Como agrupar arquivos por extensão (LINQ) (C#)](./how-to-group-files-by-extension-linq.md)\
 Mostra como retornar grupos de objetos <xref:System.IO.FileInfo> com base em sua extensão de nome de arquivo.  
  
 [Como consultar o número total de bytes em um conjunto de pastas (LINQ) (C#)](./how-to-query-for-the-total-number-of-bytes-in-a-set-of-folders-linq.md)\
 Mostra como retornar o número total de bytes em todos os arquivos de uma árvore de diretórios especificada.  
  
 [Como comparar o conteúdo de duas pastas (LINQ) (C#)](./how-to-compare-the-contents-of-two-folders-linq.md)s  
 Mostra como retornar todos os arquivos que estão presentes em duas pastas especificadas e também todos os arquivos que estão presentes em uma pasta, mas não na outra.  
  
 [Como consultar o maior arquivo ou arquivos em uma árvore de diretório (LINQ) (C#)](./how-to-query-for-the-largest-file-or-files-in-a-directory-tree-linq.md)\
 Mostra como retornar o maior ou o menor arquivo ou um número especificado de arquivos, em uma árvore de diretório.  
  
 [Como consultar arquivos duplicados em uma árvore de diretórios (LINQ) (C#)](./how-to-query-for-duplicate-files-in-a-directory-tree-linq.md)\
 Mostra como agrupar todos os nomes de arquivo que ocorrem em mais de um local em uma árvore de diretórios especificada. Também mostra como realizar comparações mais complexas com base em um comparador personalizado.  
  
 [Como consultar o conteúdo de arquivos em uma pasta (LINQ) (C#)](./how-to-query-the-contents-of-files-in-a-folder-lin.md)\
 Mostra como iterar pelas pastas em uma árvore, abrir cada arquivo e consultar o conteúdo do arquivo.  
  
## <a name="comments"></a>Comentários  
 Há certa complexidade envolvida na criação de uma fonte de dados que representa o conteúdo do sistema de arquivos com precisão e trata exceções de maneira elegante. Os exemplos nesta seção criam uma coleção de instantâneos de objetos <xref:System.IO.FileInfo> que representa todos os arquivos em uma pasta raiz especificada e todas as suas subpastas. O estado real de cada <xref:System.IO.FileInfo> pode ser alterado no tempo entre o momento em que você começa e termina a execução de uma consulta. Por exemplo, você pode criar uma lista de objetos <xref:System.IO.FileInfo> para usar como uma fonte de dados. Se você tentar acessar a propriedade `Length` em uma consulta, o objeto <xref:System.IO.FileInfo> tentará acessar o sistema de arquivos para atualizar o valor de `Length`. Se o arquivo não existir, você obterá uma <xref:System.IO.FileNotFoundException> em sua consulta, embora não esteja consultando diretamente o sistema de arquivos. Algumas consultas nesta seção usam um método separado que consome essas exceções específicas em determinados casos. Outra opção é manter a fonte de dados atualizada dinamicamente usando o <xref:System.IO.FileSystemWatcher>.  
  
## <a name="see-also"></a>Confira também

- [LINQ to Objects (C#)](./linq-to-objects.md)
