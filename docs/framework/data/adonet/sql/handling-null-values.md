---
title: Manipulando valores nulos
description: Saiba mais sobre como o Provedor de Dados de .NET Framework para SQL Server lida com NULL e leia sobre nulo e SqlBoolean, lógica de três valores e atribuindo valores nulos.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: f18b288f-b265-4bbe-957f-c6833c0645ef
ms.openlocfilehash: a4d086d81f1c2c959780366cfeb59f2d265bc40c
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286449"
---
# <a name="handling-null-values"></a>Manipulando valores nulos
Um valor nulo em um banco de dados relacional é usado quando o valor em uma coluna é desconhecido ou está ausente. Um valor nulo não é uma cadeia de caracteres vazia (para tipos de dados character ou datetime) nem um valor zero (para tipos de dados numéricos). A especificação ANSI SQL-92 declara que um valor nulo deve ser o mesmo para todos os tipos de dados, para que todos os nulos sejam manipulados consistentemente. O namespace <xref:System.Data.SqlTypes> fornece semântica nula ao implementar a interface <xref:System.Data.SqlTypes.INullable>. Cada um dos tipos de dados no <xref:System.Data.SqlTypes> tem uma propriedade própria `IsNull` e um valor `Null` que pode ser atribuído a uma instância desse tipo de dados.  
  
> [!NOTE]
> O .NET Framework versão 2,0 introduziu suporte para tipos de valor anulável, que permitem aos programadores estender um tipo de valor para representar todos os valores do tipo subjacente. Esses tipos de valor anuláveis CLR representam uma instância da <xref:System.Nullable> estrutura. Essa funcionalidade é especialmente útil quando os tipos de valor são boxed e unboxed, fornecendo compatibilidade aprimorada com tipos de objeto. Os tipos de valores anuláveis CLR não se destinam ao armazenamento de nulos de banco de dados porque um nulo SQL ANSI não se comporta da mesma forma que uma `null` referência (ou `Nothing` em Visual Basic). Para trabalhar com valores nulos ANSI SQL de banco de dados, use nulos <xref:System.Data.SqlTypes> em vez de <xref:System.Nullable>. Para obter mais informações sobre como trabalhar com tipos anuláveis de valor CLR em Visual Basic consulte [tipos de valor anulável](../../../../visual-basic/programming-guide/language-features/data-types/nullable-value-types.md)e para C#, consulte [tipos de valor anulável](../../../../csharp/language-reference/builtin-types/nullable-value-types.md).  
  
## <a name="nulls-and-three-valued-logic"></a>Valores nulos e lógica de três valores  
 Permitir valores nulos em definições de coluna apresenta uma lógica de três valores em seu aplicativo. Uma comparação pode ser avaliada como uma das três condições:  
  
- True  
  
- Falso  
  
- Unknown  
  
 Como um nulo é considerado desconhecido, dois valores nulos comparados entre si não são considerados iguais. Em expressões que usam operadores aritméticos, se um dos operandos for nulo, o resultado será nulo também.  
  
## <a name="nulls-and-sqlboolean"></a>Nulos e SqlBoolean  
 A comparação entre um <xref:System.Data.SqlTypes> retornará um <xref:System.Data.SqlTypes.SqlBoolean>. A função `IsNull` para cada `SqlType` retorna um <xref:System.Data.SqlTypes.SqlBoolean> e pode ser usada para verificar se há valores nulos. As tabelas da verdade a seguir mostram como os operadores AND, OR e NOT funcionam na presença de um valor nulo. (T = verdadeiro, F = falso e U = desconhecido ou nulo.)  
  
 ![Tabela da verdade](./media/truthtable-bpuedev11.gif "TruthTable_bpuedev11")  
  
### <a name="understanding-the-ansi_nulls-option"></a>Noções básicas sobre a opção ANSI_NULLS  
 O <xref:System.Data.SqlTypes> fornece a mesma semântica que quando a opção ANSI_NULLS é definida no SQL Server. Todos os operadores aritméticos (+,-, \* ,/,%), operadores de bit (~, &, \| ) e a maioria das funções retornarão NULL se qualquer um dos operandos ou argumentos for nulo, exceto para a propriedade `IsNull` .  
  
 O padrão ANSI SQL-92 não oferece suporte a *columnName* = NULL em uma cláusula WHERE. No SQL Server, a opção ANSI_NULLS controla a nulabilidade padrão no banco de dados e a avaliação de comparações com valores nulos. Se ANSI_NULLS estiver ativado (o padrão), o operador IS NULL deverá ser usado em expressões ao testar valores nulos. Por exemplo, a comparação seguinte sempre retorna unknown quando ANSI_NULLS for on:  
  
```sql
colname > NULL  
```  
  
 A comparação com uma variável que contém um valor nulo também produz valores desconhecidos:  
  
```sql
colname > @MyVariable  
```  
  
 Use o predicado IS NULL ou IS NOT NULL para testar um valor nulo. Isso pode adicionar complexidade à cláusula WHERE. Por exemplo, a coluna TerritoryID na tabela Cliente AdventureWorks permite valores nulos. Se uma instrução SELECT for testada para obter valores nulos além de outros, ela deverá incluir um predicado IS NULL:  
  
```sql
SELECT CustomerID, AccountNumber, TerritoryID  
FROM AdventureWorks.Sales.Customer  
WHERE TerritoryID IN (1, 2, 3)  
   OR TerritoryID IS NULL  
```  
  
 Se você desativar ANSI_NULLS no SQL Server, poderá criar expressões que usam o operador de igualdade para comparar com o valor nulo. No entanto, você não pode impedir que diferentes conexões configurem opções nulas para essa conexão. Usar IS NULL para testar valores nulos sempre funciona, independentemente das configurações de ANSI_NULLS para uma conexão.  
  
 A desativação de ANSI_NULLS não é compatível em um `DataSet`, que sempre segue o padrão ANSI SQL-92 para manipular valores nulos no <xref:System.Data.SqlTypes>.  
  
## <a name="assigning-null-values"></a>Atribuindo valores nulos  
 Os valores nulos são especiais e suas semânticas de armazenamento e atribuição diferem em sistemas de tipos e sistemas de armazenamento diferentes. Um `Dataset` foi projetado para ser usado com diferentes sistemas de tipo e armazenamento.  
  
 Esta seção descreve a semântica nula para atribuir valores nulos a um <xref:System.Data.DataColumn> em um <xref:System.Data.DataRow> entre os diferentes sistemas de tipos.  
  
 `DBNull.Value`  
 Essa atribuição é válida para um `DataColumn` de qualquer tipo. Se o tipo implementa `INullable`, o `DBNull.Value` é forçado no valor nulo fortemente tipado apropriado.  
  
 `SqlType.Null`  
 Todos os tipos de dados <xref:System.Data.SqlTypes> implementam `INullable`. Se o valor nulo fortemente tipado puder ser convertido no tipo de dados da coluna usando operadores de conversão implícitos, a atribuição deverá passar. Caso contrário, uma exceção de conversão inválida será lançada.  
  
 `null`  
 Se nulo for um valor válido para o tipo de dados `DataColumn` especificado, ele será forçado no `DbNull.Value` ou no `Null` apropriado associado ao tipo `INullable` (`SqlType.Null`)  
  
 `derivedUdt.Null`  
 Para colunas UDT, os nulos são sempre armazenados com base no tipo associado à `DataColumn`. Considere o caso de um UDT associado a um `DataColumn` que não implementa `INullable`, enquanto sua subclasse faz a implementação. Nesse caso, se um valor nulo fortemente tipado associado à classe derivada for atribuído, ele será armazenado como um `DbNull.Value` não tipado, porque o armazenamento nulo é sempre consistente com o tipo de dados de DataColumn.  
  
> [!NOTE]
> No momento, a estrutura `Nullable<T>` ou <xref:System.Nullable> não é compatível no `DataSet`.  
  
### <a name="multiple-column-row-assignment"></a>Atribuição de várias colunas (linha)  
 `DataTable.Add`, `DataTable.LoadDataRow` ou outras APIs que aceitam um <xref:System.Data.DataRow.ItemArray%2A> que é mapeado para uma linha, mapeiam "nulo" para o valor padrão da DataColumn. Se um objeto na matriz contiver `DbNull.Value` ou sua contraparte fortemente tipada, as mesmas regras descritas acima serão aplicadas.  
  
 Além disso, as seguintes regras se aplicam a uma instância de atribuições nulas de `DataRow.["columnName"]`:  
  
1. O valor *default* padrão é `DbNull.Value` para todos, a não ser para as colunas nulas fortemente tipadas em que ele é o valor nulo fortemente tipado apropriado.  
  
2. Os valores nulos nunca são gravados durante a serialização para arquivos XML (como em "xsi:nil").  
  
3. Todos os valores não nulos, incluindo os padrões, sempre são gravados durante a serialização para XML. Isso é diferente da semântica XSD/XML em que um valor nulo (xsi:nil) é explícito e o valor padrão é implícito (se não estiver presente em XML, um analisador de validação poderá obtê-lo de um esquema XSD associado). O oposto é verdadeiro para um `DataTable`: um valor nulo é implícito e o valor padrão é explícito.  
  
4. Todos os valores de coluna ausentes para linhas lidas da entrada XML são atribuídos como NULL. As linhas criadas usando <xref:System.Data.DataTable.NewRow%2A> ou métodos semelhantes recebem o valor padrão de DataColumn.  
  
5. O método <xref:System.Data.DataRow.IsNull%2A> retorna `true` para `DbNull.Value` e `INullable.Null`.  
  
## <a name="assigning-null-values"></a>Atribuindo valores nulos  
 O valor padrão para uma instância <xref:System.Data.SqlTypes> é nulo.  
  
 Os valores nulos em <xref:System.Data.SqlTypes> são específicos do tipo e não podem ser representados por um valor, como `DbNull`. Use a propriedade `IsNull` para verificar valores nulos.  
  
 Os valores nulos podem ser atribuídos a um <xref:System.Data.DataColumn>, conforme mostrado no exemplo de código a seguir. Você pode atribuir diretamente valores nulos a variáveis `SqlTypes` sem disparar uma exceção.  
  
### <a name="example"></a>Exemplo  
 O exemplo de código a seguir cria um <xref:System.Data.DataTable> com duas colunas definidas como <xref:System.Data.SqlTypes.SqlInt32> e <xref:System.Data.SqlTypes.SqlString>. O código adiciona uma linha de valores conhecidos, uma linha de valores nulos e, em seguida, itera por <xref:System.Data.DataTable>, atribuindo os valores a variáveis e exibindo a saída na janela do console.  
  
 [!code-csharp[DataWorks SqlTypes.IsNull#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlTypes.IsNull/CS/source.cs#1)]
 [!code-vb[DataWorks SqlTypes.IsNull#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlTypes.IsNull/VB/source.vb#1)]  
  
 Esse exemplo mostra os seguintes resultados:  
  
```output
isColumnNull=False, ID=123, Description=Side Mirror  
isColumnNull=True, ID=Null, Description=Null  
```  
  
## <a name="comparing-null-values-with-sqltypes-and-clr-types"></a>Comparando valores nulos com SqlTypes e os tipos CLR  
 Ao comparar valores nulos, é importante entender a diferença entre a maneira como o método `Equals` avalia valores nulos em <xref:System.Data.SqlTypes> em comparação com o modo como ele funciona com tipos CLR. Todos os métodos <xref:System.Data.SqlTypes>`Equals` usam semânticas de banco de dados para avaliar valores nulos: se um ou ambos os valores forem nulos, a comparação produzirá um valor nulo. Por outro lado, usar o método CLR `Equals` em dois <xref:System.Data.SqlTypes> produzirá true se ambos forem valores nulos. Isso reflete a diferença entre usar um método de instância, como o método CLR `String.Equals`, e usar o método estático/compartilhado, `SqlString.Equals`.  
  
 O exemplo a seguir demonstra a diferença nos resultados entre o método `SqlString.Equals` e o método `String.Equals` quando cada um é passado por um par de valores nulos e, em seguida, um par de cadeias de caracteres vazias.  
  
 [!code-csharp[DataWorks SqlTypes.CompareNulls#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlTypes.CompareNulls/CS/source.cs#1)]
 [!code-vb[DataWorks SqlTypes.CompareNulls#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlTypes.CompareNulls/VB/source.vb#1)]  
  
 O código produz a seguinte saída:  
  
```output
SqlString.Equals shared/static method:  
  Two nulls=Null  
  
String.Equals instance method:  
  Two nulls=True  
  
SqlString.Equals shared/static method:  
  Two empty strings=True  
  
String.Equals instance method:  
  Two empty strings=True
```  
  
## <a name="see-also"></a>Veja também

- [Tipos de dados SQL Server e ADO.NET](sql-server-data-types.md)
- [Visão geral do ADO.NET](../ado-net-overview.md)
