---
title: Criação de novas cadeias de caracteres no .NET
description: Aprenda a criar cadeias de caracteres usando atribuição, construtores de classe ou métodos System. String que combinam várias cadeias de caracteres, matrizes de cadeias de caracteres ou objetos no .NET.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- CopyTo method
- Join method
- Format method
- Concat method
- strings [.NET Framework], creating
- Insert method
ms.assetid: 06fdf123-2fac-4459-8904-eb48ab908a30
ms.openlocfilehash: b44d0f8e1717ead72e28f0be644644961d1482b6
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596442"
---
# <a name="creating-new-strings-in-net"></a>Criação de novas cadeias de caracteres no .NET
O .NET Framework permite que cadeias de caracteres sejam criadas usando uma atribuição simples e também sobrecarrega um construtor de classe para dar suporte à criação de cadeias de caracteres usando um número de parâmetros diferentes. O .NET Framework também fornece vários métodos na classe <xref:System.String?displayProperty=nameWithType> que criam objetos de cadeia de caracteres combinando várias cadeias de caracteres, matrizes de cadeias de caracteres ou objetos.  
  
## <a name="creating-strings-using-assignment"></a>Criando cadeias de caracteres usando atribuição  
 A maneira mais fácil para criar um novo objeto <xref:System.String> é simplesmente atribuir uma cadeia de caracteres literal a um objeto <xref:System.String>.  
  
## <a name="creating-strings-using-a-class-constructor"></a>Criando cadeias de caracteres usando um construtor de classe  
 Você pode usar sobrecargas do constructo de classe <xref:System.String> para criar cadeias de caracteres de matrizes de caracteres. Você também pode criar uma nova cadeia de caracteres duplicando um caractere específico, m número de vezes especificado.  
  
## <a name="methods-that-return-strings"></a>Métodos que retornam cadeias de caracteres  
 A tabela a seguir lista vários métodos úteis que retornam novos objetos de cadeia de caracteres.  
  
|Nome do método|Uso|  
|-----------------|---------|  
|<xref:System.String.Format%2A?displayProperty=nameWithType>|Cria uma cadeia de caracteres formatada de um conjunto de objetos de entrada.|  
|<xref:System.String.Concat%2A?displayProperty=nameWithType>|Cria cadeias de caracteres de duas ou mais cadeias de caracteres.|  
|<xref:System.String.Join%2A?displayProperty=nameWithType>|Cria uma nova cadeia de caracteres combinando uma matriz de cadeias de caracteres.|  
|<xref:System.String.Insert%2A?displayProperty=nameWithType>|Cria uma nova cadeia de caracteres inserindo uma cadeia de caracteres no índice especificado de uma cadeia de caracteres existente.|  
|<xref:System.String.CopyTo%2A?displayProperty=nameWithType>|Copia caracteres especificados de uma cadeia de caracteres para uma posição especificada em uma matriz de caracteres.|  
  
### <a name="format"></a>Formatar  
 Você pode usar o método **String.Format** para criar cadeias de caracteres formatadas e concatenar cadeias de caracteres que representam vários objetos. Este método converte automaticamente qualquer objeto passado em uma cadeia de caracteres. Por exemplo, se seu aplicativo precisar exibir um valor **Int32** e um valor **DateTime** para o usuário, você pode facilmente criar uma cadeia de caracteres para representar esses valores usando o método **Format**. Para obter informações sobre as convenções de formatação usadas com esse método, consulte a seção sobre [formatação de composição](composite-formatting.md).  
  
 O exemplo a seguir usa o método **Format** para criar uma cadeia de caracteres que usa uma variável de inteiro.  
  
 [!code-csharp[Strings.Creating#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Strings.Creating/cs/Example.cs#1)]
 [!code-vb[Strings.Creating#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Strings.Creating/vb/Example.vb#1)]  
  
 Neste exemplo, <xref:System.DateTime.Now%2A?displayProperty=nameWithType> exibe a data e hora atuais da maneira especificada pela cultura associada ao thread atual.  
  
### <a name="concat"></a>Concat  
 O método **String.Concat** pode ser usado para criar facilmente um novo objeto de cadeia de caracteres de dois ou mais objetos existentes. Ele fornece uma maneira independente da linguagem para concatenar cadeias de caracteres. Este método aceita qualquer classe derivada de **System.Object**. O exemplo a seguir cria uma cadeia de caracteres de dois objetos de cadeia de caracteres existentes e um caractere de separação.  
  
 [!code-csharp[Strings.Creating#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Strings.Creating/cs/Example.cs#2)]
 [!code-vb[Strings.Creating#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Strings.Creating/vb/Example.vb#2)]  
  
### <a name="join"></a>Join  
 O método **String.Join** cria uma nova cadeia de caracteres a partir de uma matriz de cadeias de caracteres e uma cadeia de caracteres de separador. Esse método é útil se você quiser concatenar várias cadeias de caracteres fazendo uma lista, talvez separada por vírgula.  
  
 O exemplo a seguir usa um espaço para associar uma matriz de cadeias de caracteres.  
  
 [!code-csharp[Strings.Creating#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Strings.Creating/cs/Example.cs#3)]
 [!code-vb[Strings.Creating#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Strings.Creating/vb/Example.vb#3)]  
  
### <a name="insert"></a>Inserir  
 O método **String.Insert** cria uma nova cadeia de caracteres inserindo uma cadeia de caracteres em uma posição especificada em outra cadeia de caracteres. Este método usa um índice baseado em zero. O exemplo a seguir insere uma cadeia de caracteres na quinta posição do índice de `MyString` e cria uma nova cadeia de caracteres com esse valor.  
  
 [!code-csharp[Strings.Creating#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Strings.Creating/cs/Example.cs#4)]
 [!code-vb[Strings.Creating#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Strings.Creating/vb/Example.vb#4)]  
  
### <a name="copyto"></a>CopyTo  
 O método **String.CopyTo** copia partes de uma cadeia de caracteres em uma matriz de caracteres. Você pode especificar o índice inicial da cadeia de caracteres e o número de caracteres a serem copiados. Este método usa o índice de origem, uma matriz de caracteres, o índice de destino e o número de caracteres a serem copiados. Todos os índices são baseados em zero.  
  
 O exemplo a seguir usa o método **CopyTo** para copiar os caracteres da palavra "Hello" de um objeto de cadeia de caracteres para a primeira posição de índice de uma matriz de caracteres.  
  
 [!code-csharp[Strings.Creating#5](../../../samples/snippets/csharp/VS_Snippets_CLR/Strings.Creating/cs/Example.cs#5)]
 [!code-vb[Strings.Creating#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Strings.Creating/vb/Example.vb#5)]  
  
## <a name="see-also"></a>Consulte também

- [Operações básicas de cadeia de caracteres](basic-string-operations.md)
- [Formatação composta](composite-formatting.md)
