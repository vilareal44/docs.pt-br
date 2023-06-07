---
title: Marshaling de diversos tipos de matrizes
description: Empacotar tipos de matriz diferentes, como inteiros por valor ou referência, inteiros bidimensionais por valor, cadeias de caracteres por valor e estruturas com números inteiros ou cadeias de caracteres.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- marshaling, Arrays sample
- data marshaling, Arrays sample
ms.assetid: c5ac9920-5b6e-4dc9-bf2d-1f6f8ad3b0bf
ms.openlocfilehash: f1473c7917189f0b36c96b2adcf20005c5fd6b48
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621490"
---
# <a name="marshaling-different-types-of-arrays"></a>Marshaling de diversos tipos de matrizes
Uma matriz é um tipo de referência em código gerenciado que contém um ou mais elementos do mesmo tipo. Embora as matrizes sejam tipos de referência, elas são passadas como parâmetros para funções não gerenciadas. Esse comportamento é inconsistente com a maneira que matrizes gerenciadas são passadas para objetos gerenciados, que é na forma de parâmetros de In/Out. Para obter detalhes adicionais, consulte [Copiando e fixando](copying-and-pinning.md).  
  
 A tabela a seguir lista as opções de marshaling para matrizes e descreve o uso delas.  
  
|Array|Descrição|  
|-----------|-----------------|  
|De inteiros por valor.|Passa uma matriz de inteiros como um parâmetro In.|  
|De inteiros por referência.|Passa uma matriz de inteiros como um parâmetro In/Out.|  
|De inteiros por valor (bidimensional).|Passa uma matriz de inteiros como um parâmetro In.|  
|De cadeias de caracteres por valor.|Passa uma matriz de cadeias de caracteres como um parâmetro In.|  
|De estruturas com inteiros.|Passa uma matriz de estruturas que contêm inteiros como um parâmetro In.|  
|De estruturas com cadeias de caracteres.|Passa uma matriz de estruturas que contêm somente cadeias de caracteres como um parâmetro Entrada/Saída. Os membros da matriz podem ser alterados.|  
  
## <a name="example"></a>Exemplo  
 Este exemplo demonstra como passar os seguintes tipos de matrizes:  
  
- Matriz de inteiros por valor.  
  
- Matriz de inteiros por referência, que pode ser redimensionada.  
  
- Matriz multidimensional de inteiros por valor.  
  
- Matriz de cadeias de caracteres por valor.  
  
- Matriz de estruturas com inteiros.  
  
- Matriz de estruturas com cadeias de caracteres.  
  
 A menos que o marshaling de uma matriz seja realizado explicitamente por referência, o comportamento padrão realizará o marshaling da matriz como um parâmetro In. Você pode alterar esse comportamento ao aplicar os atributos <xref:System.Runtime.InteropServices.InAttribute> e <xref:System.Runtime.InteropServices.OutAttribute> explicitamente.  
  
 A amostra de matrizes usa as seguintes funções não gerenciadas, mostradas com a sua declaração de função original:  
  
- **TestArrayOfInts** exportado de PinvokeLib.dll.  
  
    ```cpp
    int TestArrayOfInts(int* pArray, int pSize);  
    ```  
  
- **TestRefArrayOfInts** exportado de PinvokeLib.dll.  
  
    ```cpp
    int TestRefArrayOfInts(int** ppArray, int* pSize);  
    ```  
  
- **TestMatrixOfInts** exportado de PinvokeLib.dll.  
  
    ```cpp
    int TestMatrixOfInts(int pMatrix[][COL_DIM], int row);  
    ```  
  
- **TestArrayOfStrings** exportado de PinvokeLib.dll.  
  
    ```cpp
    int TestArrayOfStrings(char** ppStrArray, int size);  
    ```  
  
- **TestArrayOfStructs** exportado de PinvokeLib.dll.  
  
    ```cpp
    int TestArrayOfStructs(MYPOINT* pPointArray, int size);  
    ```  
  
- **TestArrayOfStructs2** exportado de PinvokeLib.dll.  
  
    ```cpp
    int TestArrayOfStructs2 (MYPERSON* pPersonArray, int size);  
    ```  
  
 [PinvokeLib.dll](marshaling-data-with-platform-invoke.md#pinvokelibdll) é uma biblioteca personalizada não gerenciada que contém implementações para as funções listadas anteriormente e duas variáveis de estrutura, **MYPOINT** e **MYPERSON**. As estruturas contêm os seguintes elementos:  
  
```cpp
typedef struct _MYPOINT  
{  
   int x;
   int y;
} MYPOINT;  
  
typedef struct _MYPERSON  
{  
   char* first;
   char* last;
} MYPERSON;  
```  
  
 Neste exemplo, as estruturas `MyPoint` e `MyPerson` contêm tipos inseridos. O atributo <xref:System.Runtime.InteropServices.StructLayoutAttribute> é definido para garantir que os membros sejam organizados na memória em sequência, na ordem em que aparecem.  
  
 A classe `NativeMethods` contém um conjunto de métodos chamados pela classe `App`. Para obter detalhes específicos sobre passagem de matrizes, consulte os comentários na amostra a seguir. Uma matriz, que é um tipo de referência, é passada por padrão como um parâmetro In. Para o chamador receber os resultados, **InAttribute** e **OutAttribute** devem ser aplicados explicitamente ao argumento que contém a matriz.  
  
### <a name="declaring-prototypes"></a>Declarando Protótipos  
 [!code-csharp[Conceptual.Interop.Marshaling#31](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/arrays.cs#31)]
 [!code-vb[Conceptual.Interop.Marshaling#31](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/arrays.vb#31)]  
  
### <a name="calling-functions"></a>Chamando Funções  
 [!code-csharp[Conceptual.Interop.Marshaling#32](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/arrays.cs#32)]
 [!code-vb[Conceptual.Interop.Marshaling#32](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/arrays.vb#32)]  
  
## <a name="see-also"></a>Consulte também

- [Tipos de dados de invocação de plataforma](marshaling-data-with-platform-invoke.md#platform-invoke-data-types)
- [Criando protótipos em código gerenciado](creating-prototypes-in-managed-code.md)
