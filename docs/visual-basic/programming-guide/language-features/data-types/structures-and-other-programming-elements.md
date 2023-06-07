---
title: Estruturas e outros elementos de programação
ms.date: 07/20/2015
helpviewer_keywords:
- structures [Visual Basic], arrays
- procedures [Visual Basic], structures as arguments to
- objects [Visual Basic], structure elements
- arrays [Visual Basic], structure elements
- nested structures [Visual Basic]
ms.assetid: 0f849313-ccd2-4c9a-acb9-69de6751c088
ms.openlocfilehash: dbd24065a954e5611663963371d5a9f4bbbaea68
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84393488"
---
# <a name="structures-and-other-programming-elements-visual-basic"></a>Estruturas e outros elementos de programação (Visual Basic)
Você pode usar estruturas em conjunto com matrizes, objetos e procedimentos, bem como entre si. As interações usam a mesma sintaxe que esses elementos usam individualmente.  
  
> [!NOTE]
> Você não pode inicializar nenhum dos elementos de estrutura na declaração de estrutura. Você pode atribuir valores somente a elementos de uma variável que foi declarada como sendo de um tipo de estrutura.  
  
## <a name="structures-and-arrays"></a>Estruturas e matrizes  
 Uma estrutura pode conter uma matriz como um ou mais de seus elementos. O exemplo a seguir ilustra isto.  
  
```vb  
Public Structure systemInfo  
    Public cPU As String  
    Public memory As Long  
    Public diskDrives() As String  
    Public purchaseDate As Date  
End Structure
```  
  
 Você acessa os valores de uma matriz dentro de uma estrutura da mesma maneira que acessa uma propriedade em um objeto. O exemplo a seguir ilustra isto.  
  
```vb  
Dim mySystem As systemInfo  
ReDim mySystem.diskDrives(3)  
mySystem.diskDrives(0) = "1.44 MB"  
```  
  
 Você também pode declarar uma matriz de estruturas. O exemplo a seguir ilustra isto.  
  
```vb  
Dim allSystems(100) As systemInfo  
```  
  
 Siga as mesmas regras para acessar os componentes dessa arquitetura de dados. O exemplo a seguir ilustra isto.  
  
```vb  
ReDim allSystems(5).diskDrives(3)  
allSystems(5).CPU = "386SX"  
allSystems(5).diskDrives(2) = "100M SCSI"  
```  
  
## <a name="structures-and-objects"></a>Estruturas e objetos  
 Uma estrutura pode conter um objeto como um ou mais de seus elementos. O exemplo a seguir ilustra isto.  
  
```vb  
Protected Structure userInput  
    Public userName As String  
    Public inputForm As System.Windows.Forms.Form  
    Public userFileNumber As Integer  
End Structure  
```  
  
 Você deve usar uma classe de objeto específica em tal declaração, em vez de `Object` .  
  
## <a name="structures-and-procedures"></a>Estruturas e procedimentos  
 Você pode passar uma estrutura como um argumento de procedimento. O exemplo a seguir ilustra isto.  
  
```vb  
Public currentCPUName As String = "700MHz Pentium compatible"  
Public currentMemorySize As Long = 256  
Public Sub fillSystem(ByRef someSystem As systemInfo)  
    someSystem.cPU = currentCPUName  
    someSystem.memory = currentMemorySize  
    someSystem.purchaseDate = Now  
End Sub  
```  
  
 O exemplo anterior passa a estrutura *por referência*, que permite que o procedimento modifique seus elementos para que as alterações entrem em vigor no código de chamada. Se você quiser proteger uma estrutura contra tal modificação, passe-a por valor.  
  
 Você também pode retornar uma estrutura a partir de um `Function` procedimento. O exemplo a seguir ilustra isto.  
  
```vb  
Dim allSystems(100) As systemInfo  
Function findByDate(ByVal searchDate As Date) As systemInfo  
    Dim i As Integer  
    For i = 1 To 100  
        If allSystems(i).purchaseDate = searchDate Then Return allSystems(i)  
    Next i  
   ' Process error: system with desired purchase date not found.  
End Function  
```  
  
## <a name="structures-within-structures"></a>Estruturas dentro de estruturas  
 Estruturas podem conter outras estruturas. O exemplo a seguir ilustra isto.  
  
```vb  
Public Structure driveInfo  
    Public type As String  
    Public size As Long  
End Structure  
Public Structure systemInfo  
    Public cPU As String  
    Public memory As Long  
    Public diskDrives() As driveInfo  
    Public purchaseDate As Date  
End Structure  
```  
  
```vb  
Dim allSystems(100) As systemInfo  
ReDim allSystems(1).diskDrives(3)  
allSystems(1).diskDrives(0).type = "Floppy"  
```  
  
 Você também pode usar essa técnica para encapsular uma estrutura definida em um módulo dentro de uma estrutura definida em um módulo diferente.  
  
 Estruturas podem conter outras estruturas para uma profundidade arbitrária.  
  
## <a name="see-also"></a>Confira também

- [Tipos de dados](index.md)
- [Tipos de dados elementares](elementary-data-types.md)
- [Tipos de dados compostos](composite-data-types.md)
- [Tipos de valor e referência](value-types-and-reference-types.md)
- [Estruturas](structures.md)
- [Solução de problemas de tipos de dados](troubleshooting-data-types.md)
- [Como: Declarar uma estrutura](how-to-declare-a-structure.md)
- [Variáveis de estrutura](structure-variables.md)
- [Estruturas e classes](structures-and-classes.md)
- [Instrução Structure](../../../language-reference/statements/structure-statement.md)
