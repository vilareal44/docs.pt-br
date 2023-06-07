---
title: Instrução ReDim
ms.date: 07/20/2015
f1_keywords:
- vb.ReDim
- vb.Preserve
helpviewer_keywords:
- fixed-length strings [Visual Basic], declaring
- ReDim statement [Visual Basic], syntax
- dynamic arrays [Visual Basic], ReDim statement
- arrays [Visual Basic], reallocating
- arrays [Visual Basic], reinitializing
- arrays [Visual Basic], dimensions
- scalars, and arrays
- scalars
- declarations [Visual Basic], dynamic arrays
- variables [Visual Basic], scalar
- ReDim statement [Visual Basic]
- data types [Visual Basic], assigning
- As keyword [Visual Basic], in ReDim statement
- To keyword [Visual Basic], ReDim statement
- arrays [Visual Basic], declaring
- Preserve keyword [Visual Basic], ReDim statement
- storage [Visual Basic], allocating
- arrays [Visual Basic], and scalars
- declaration statements [Visual Basic]
- scalar variables [Visual Basic]
ms.assetid: ad1c5e07-dcd7-4ae1-a79e-ad3f2dcc2083
ms.openlocfilehash: 82f19762865fdf3c3f32a0349e21e3b97bebd567
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404272"
---
# <a name="redim-statement-visual-basic"></a>Instrução ReDim (Visual Basic)
Realoca espaço de armazenamento para uma variável de matriz.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
ReDim [ Preserve ] name(boundlist) [ ,  name(boundlist) [, ... ] ]  
```  
  
## <a name="parts"></a>Partes  
  
|Termo|Definição|  
|----------|----------------|  
|`Preserve`|Opcional. Modificador usado para preservar os dados na matriz existente quando você altera o tamanho da última dimensão.|  
|`name`|Obrigatórios. Nome da variável de matriz. Consulte [nomes de elementos declarados](../../programming-guide/language-features/declared-elements/declared-element-names.md).|  
|`boundlist`|Obrigatórios. Lista de limites de cada dimensão da matriz redefinida.|  
  
## <a name="remarks"></a>Comentários  
 Você pode usar a `ReDim` instrução para alterar o tamanho de uma ou mais dimensões de uma matriz que já foi declarada. Se você tiver uma matriz grande e não precisar mais de alguns de seus elementos, o `ReDim` poderá liberar memória reduzindo o tamanho da matriz. Por outro lado, se sua matriz precisar de mais elementos, o `ReDim` poderá adicioná-los.  
  
 A `ReDim` instrução é destinada apenas a matrizes. Não é válido em escalares (variáveis que contêm apenas um único valor), coleções ou estruturas. Observe que, se você declarar uma variável para ser do tipo `Array` , a `ReDim` instrução não terá informações de tipo suficientes para criar a nova matriz.  
  
 Você pode usar `ReDim` somente no nível do procedimento. Portanto, o contexto da declaração para a variável deve ser um procedimento; Ele não pode ser um arquivo de origem, um namespace, uma interface, uma classe, uma estrutura, um módulo ou um bloco. Para obter mais informações, consulte [Contextos de declaração e níveis de acesso padrão](declaration-contexts-and-default-access-levels.md).  
  
## <a name="rules"></a>Regras  
  
- **Várias variáveis.** Você pode redimensionar várias variáveis de matriz na mesma instrução de declaração e especificar as `name` `boundlist` partes e para cada variável. Várias variáveis são separadas por vírgulas.  
  
- **Limites de matriz.** Cada entrada no `boundlist` pode especificar os limites inferior e superior dessa dimensão. O limite inferior é sempre 0 (zero). O limite superior é o valor de índice mais alto possível para essa dimensão, não o comprimento da dimensão (que é o limite superior mais um). O índice de cada dimensão pode variar de 0 ao seu valor de limite superior.  
  
     O número de dimensões em `boundlist` deve corresponder ao número original de dimensões (classificação) da matriz.  
  
- **Tipos de dados.** A `ReDim` instrução não pode alterar o tipo de dados de uma variável de matriz ou seus elementos.  
  
- **Initialization.** A `ReDim` instrução não pode fornecer novos valores de inicialização para os elementos da matriz.  
  
- **Fique.** A `ReDim` instrução não pode alterar a classificação (o número de dimensões) da matriz.  
  
- **Redimensionando com Preserve.** Se você usar `Preserve` o, poderá redimensionar apenas a última dimensão da matriz. Para todas as outras dimensões, você deve especificar o limite da matriz existente.  
  
     Por exemplo, se a matriz tiver apenas uma dimensão, você poderá redimensionar essa dimensão e ainda preservar todo o conteúdo da matriz, pois você está alterando a última e a única dimensão. No entanto, se sua matriz tiver duas ou mais dimensões, você poderá alterar o tamanho da última dimensão se usar `Preserve` .  
  
- **Properties.** Você pode usar `ReDim` em uma propriedade que contém uma matriz de valores.  
  
## <a name="behavior"></a>Comportamento  
  
- **Substituição de matriz.** `ReDim`Libera a matriz existente e cria uma nova matriz com a mesma classificação. A nova matriz substitui a matriz liberada na variável de matriz.  
  
- **Inicialização sem preservar.** Se você não especificar `Preserve` , `ReDim` Inicializa os elementos da nova matriz usando o valor padrão para seu tipo de dados.  
  
- **Inicialização com Preserve.** Se você especificar `Preserve` , Visual Basic copiará os elementos da matriz existente para a nova matriz.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir aumenta o tamanho da última dimensão de uma matriz dinâmica sem perder os dados existentes na matriz e, em seguida, diminui o tamanho com perda de dados parcial. Por fim, ele diminui o tamanho de volta para seu valor original e reinicializa todos os elementos da matriz.  
  
 [!code-vb[VbVbalrStatements#52](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#52)]  
  
 A `Dim` instrução cria uma nova matriz com três dimensões. Cada dimensão é declarada com um limite de 10, portanto, o índice de matriz para cada dimensão pode variar de 0 a 10. Na discussão a seguir, as três dimensões são referidas como camada, linha e coluna.  
  
 O primeiro `ReDim` cria uma nova matriz que substitui a matriz existente na variável `intArray` . `ReDim`Copia todos os elementos da matriz existente para a nova matriz. Ele também adiciona mais 10 colunas ao final de cada linha em cada camada e inicializa os elementos nessas novas colunas como 0 (o valor padrão de `Integer` , que é o tipo de elemento da matriz).  
  
 O segundo `ReDim` cria outra nova matriz e copia todos os elementos que se ajustam. No entanto, cinco colunas são perdidas do final de cada linha em cada camada. Isso não será um problema se você tiver terminado de usar essas colunas. Reduzir o tamanho de uma matriz grande pode liberar memória que você não precisa mais.  
  
 O terceiro `ReDim` cria outra nova matriz e remove outras cinco colunas do final de cada linha em cada camada. Desta vez, ele não copia nenhum elemento existente. Essa instrução reverte a matriz para seu tamanho original. Como a instrução não inclui o `Preserve` modificador, ele define todos os elementos de matriz para seus valores padrão originais.  
  
 Para obter exemplos adicionais, consulte [matrizes](../../programming-guide/language-features/arrays/index.md).  
  
## <a name="see-also"></a>Confira também

- <xref:System.IndexOutOfRangeException>
- [Instrução Const](const-statement.md)
- [Instrução Dim](dim-statement.md)
- [Instrução Erase](erase-statement.md)
- [Nada](../nothing.md)
- [Matrizes](../../programming-guide/language-features/arrays/index.md)
