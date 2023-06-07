---
title: Adicionando dados a um DataTable
description: Consulte este código de exemplo para adicionar novas linhas de dados a uma tabela no ADO.NET, depois de criar uma DataTable e definir sua estrutura usando colunas e restrições.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d6aa8474-7bde-48f7-949d-20dc38a1625b
ms.openlocfilehash: 94ebc97d5f90b5bb92186ba6f33015633bd01127
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286928"
---
# <a name="adding-data-to-a-datatable"></a>Adicionando dados a um DataTable
Depois de criar <xref:System.Data.DataTable> e definir sua estrutura usando colunas e restrições, você pode adicionar novas linhas de dados à tabela. Para adicionar uma nova linha, declare uma nova variável como tipo <xref:System.Data.DataRow>. Um novo objeto **DataRow** é retornado quando você chama o <xref:System.Data.DataTable.NewRow%2A> método. Em seguida, a **DataTable** cria o objeto **DataRow** com base na estrutura da tabela, conforme definido pelo <xref:System.Data.DataColumnCollection> .  
  
 O exemplo a seguir demonstra como criar uma nova linha chamando o método **NewRow** .  
  
```vb  
Dim workRow As DataRow = workTable.NewRow()  
```  
  
```csharp  
DataRow workRow = workTable.NewRow();  
```  
  
 Você pode manipular a linha recém-adicionada usando um índice ou o nome da coluna, conforme mostrado no exemplo a seguir.  
  
```vb  
workRow("CustLName") = "Smith"  
workRow(1) = "Smith"  
```  
  
```csharp  
workRow["CustLName"] = "Smith";  
workRow[1] = "Smith";  
```  
  
 Depois que os dados são inseridos na nova linha, o método **Add** é usado para adicionar a linha ao <xref:System.Data.DataRowCollection> , mostrada no código a seguir.  
  
```vb  
workTable.Rows.Add(workRow)  
```  
  
```csharp  
workTable.Rows.Add(workRow);  
```  
  
 Você também pode chamar o método **Add** para adicionar uma nova linha passando uma matriz de valores, digitada como <xref:System.Object> , conforme mostrado no exemplo a seguir.  
  
```vb  
workTable.Rows.Add(new Object() {1, "Smith"})  
```  
  
```csharp  
workTable.Rows.Add(new Object[] {1, "Smith"});  
```  
  
 Passar uma matriz de valores, tipado como **objeto**, para o método **Add** cria uma nova linha dentro da tabela e define seus valores de coluna para os valores na matriz de objetos. Observe que os valores na matriz são correspondidos sequencialmente a colunas, com base na ordem na qual aparecem na tabela.  
  
 O exemplo a seguir adiciona 10 linhas à tabela **Customers** recém-criada.  
  
```vb  
Dim workRow As DataRow  
Dim i As Integer  
  
For i = 0 To 9  
  workRow = workTable.NewRow()  
  workRow(0) = i  
  workRow(1) = "CustName" & I.ToString()  
  workTable.Rows.Add(workRow)  
Next  
```  
  
```csharp  
DataRow workRow;  
  
for (int i = 0; i <= 9; i++)
{  
  workRow = workTable.NewRow();  
  workRow[0] = i;  
  workRow[1] = "CustName" + i.ToString();  
  workTable.Rows.Add(workRow);  
}  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.Data.DataColumnCollection>
- <xref:System.Data.DataRow>
- <xref:System.Data.DataRowCollection>
- <xref:System.Data.DataTable>
- [Manipulando dados em uma DataTable](manipulating-data-in-a-datatable.md)
- [Visão geral do ADO.NET](../ado-net-overview.md)
