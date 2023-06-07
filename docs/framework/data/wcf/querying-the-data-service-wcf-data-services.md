---
title: Consultando o serviço de dados (WCF Data Services)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, client library
- WCF Data Services, querying
- WCF Data Services, accessing data
ms.assetid: 823e9444-27aa-4f1f-be8e-0486d67f54c0
ms.openlocfilehash: 13334f6425c47e45d729d606d99602a99f35d8e6
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286152"
---
# <a name="querying-the-data-service-wcf-data-services"></a>Consultando o serviço de dados (WCF Data Services)

A biblioteca de cliente WCF Data Services permite que você execute consultas em um serviço de dados usando padrões de programação de .NET Framework familiares, incluindo o uso do LINQ (consulta integrada à linguagem). A biblioteca de cliente converte uma consulta, que é definida no cliente como uma instância da classe <xref:System.Data.Services.Client.DataServiceQuery%601>, em uma mensagem de solicitação HTTP GET. A biblioteca recebe a mensagem de resposta e converte-a em instâncias de classes do serviço de dados cliente. Essas classes são rastreadas pelo <xref:System.Data.Services.Client.DataServiceContext> ao qual o <xref:System.Data.Services.Client.DataServiceQuery%601> pertence.

## <a name="data-service-queries"></a>Consultas de serviço de dados

A classe genérica <xref:System.Data.Services.Client.DataServiceQuery%601> representa uma consulta que retorna uma coleção de zero ou mais instâncias de tipos de entidades. Uma consulta do serviço de dados sempre pertence a um contexto de serviço de dados existente. Esse contexto mantém as informações do URI de serviço e de metadados necessárias para compor e executar a consulta.

Quando você usa a caixa de diálogo **Adicionar referência de serviço** para adicionar um serviço de dados a um aplicativo cliente baseado em .NET Framework, é criada uma classe de contêiner de entidade que herda da <xref:System.Data.Services.Client.DataServiceContext> classe. Essa classe inclui as propriedades que retornam instâncias <xref:System.Data.Services.Client.DataServiceQuery%601> tipadas. Existe uma propriedade para cada conjunto de entidades que o serviço de dados expõe. Essas propriedades facilitam a criação de uma instância <xref:System.Data.Services.Client.DataServiceQuery%601>tipada.

Uma consulta é executada nos seguintes cenários:

- Quando os resultados são enumerados implicitamente, como:

  - Quando uma propriedade no <xref:System.Data.Services.Client.DataServiceContext> que representa e o conjunto de entidades é enumerada, como durante um `foreach` loop (C#) ou `For Each` (Visual Basic).

  - Quando a consulta é atribuída a uma coleção `List`.

- Quando o método <xref:System.Data.Services.Client.DataServiceQuery%601.Execute%2A> ou <xref:System.Data.Services.Client.DataServiceQuery%601.BeginExecute%2A> é chamado explicitamente.

- Quando um operador de execução de consulta LINQ, como <xref:System.Linq.Enumerable.First%2A> ou <xref:System.Linq.Enumerable.Single%2A>, é chamado.

A consulta a seguir, quando executada, retorna todas as entidades `Customers` do serviço de dados Northwind:

[!code-csharp[Astoria Northwind Client#GetAllCustomersSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#getallcustomersspecific)]
[!code-vb[Astoria Northwind Client#GetAllCustomersSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#getallcustomersspecific)]

Para obter mais informações, consulte [como executar consultas de serviço de dados](how-to-execute-data-service-queries-wcf-data-services.md).

O cliente WCF Data Services dá suporte a consultas para objetos de associação tardia, como quando você usa o tipo *dinâmico* em C#. No entanto, por motivos de desempenho, você sempre deve compor consultas com rigidez de tipos no serviço de dados. O cliente não dá suporte ao tipo <xref:System.Tuple> e a objetos dinâmicos.

## <a name="linq-queries"></a>Consultas LINQ

Como a <xref:System.Data.Services.Client.DataServiceQuery%601> classe implementa a <xref:System.Linq.IQueryable%601> interface definida pelo LINQ, a WCF Data Services biblioteca de cliente é capaz de transformar consultas LINQ em dados de conjunto de entidades em um URI que representa uma expressão de consulta avaliada em relação a um recurso de serviço de dados. O exemplo a seguir é uma consulta LINQ que equivale à classe <xref:System.Data.Services.Client.DataServiceQuery%601> anterior que retorna `Orders` com um custo de frete superior a US$ 30 e ordena os resultados por custo de frete:

[!code-csharp[Astoria Northwind Client#AddQueryOptionsLinqSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#addqueryoptionslinqspecific)]
[!code-vb[Astoria Northwind Client#AddQueryOptionsLinqSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#addqueryoptionslinqspecific)]

Essa consulta LINQ é convertida no seguinte URI de consulta que é executado no serviço de dados de [início rápido](quickstart-wcf-data-services.md) baseado na Northwind:

```http
http://localhost:12345/Northwind.svc/Orders?Orderby=ShippedDate&?filter=Freight gt 30
```

> [!NOTE]
> O conjunto de consultas que podem ser expressas na sintaxe LINQ é mais amplo do que as consultas permitidas na sintaxe URI baseada em REST (Representational State Transfer). Uma classe <xref:System.NotSupportedException> é gerada quando a consulta não pode ser mapeada para um URI no serviço de dados de destino.

Para obter mais informações, consulte [Considerações sobre LINQ](linq-considerations-wcf-data-services.md).

## <a name="adding-query-options"></a>Adicionando opções de consulta

As consultas do serviço de dados dão suporte a todas as opções de consulta fornecidas pelo WCF Data Services. Chame o método <xref:System.Data.Services.Client.DataServiceQuery%601.AddQueryOption%2A> para acrescentar opções de consulta a uma instância <xref:System.Data.Services.Client.DataServiceQuery%601>. <xref:System.Data.Services.Client.DataServiceQuery%601.AddQueryOption%2A> retorna uma nova instância <xref:System.Data.Services.Client.DataServiceQuery%601>, que equivale à consulta original, mas com o novo conjunto de opções de consulta. A consulta a seguir, quando executada, retorna `Orders`, que são filtrados pelo valor `Freight` e ordenados por `OrderID`, em ordem decrescente:

[!code-csharp[Astoria Northwind Client#AddQueryOptionsSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#addqueryoptionsspecific)]
[!code-vb[Astoria Northwind Client#AddQueryOptionsSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#addqueryoptionsspecific)]

Você pode usar a opção de consulta `$orderby` para ordenar e filtrar uma consulta com base em uma única propriedade, como no exemplo a seguir, que filtra e ordena os `Orders` retornados com base no valor da propriedade `Freight`:

[!code-csharp[Astoria Northwind Client#OrderWithFilter](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#orderwithfilter)]
[!code-vb[Astoria Northwind Client#OrderWithFilter](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#orderwithfilter)]

Você pode chamar o método <xref:System.Data.Services.Client.DataServiceQuery%601.AddQueryOption%2A> consecutivamente para construir expressões de consulta complexas. Para obter mais informações, consulte [como: adicionar opções de consulta a uma consulta de serviço de dados](how-to-add-query-options-to-a-data-service-query-wcf-data-services.md).

As opções de consulta oferecem uma outra maneira de expressar os componentes sintáticos de uma consulta LINQ. Para obter mais informações, consulte [Considerações sobre LINQ](linq-considerations-wcf-data-services.md).

> [!NOTE]
> A opção de consulta `$select` não pode ser adicionada a um URI de consulta usando o método <xref:System.Data.Services.Client.DataServiceQuery%601.AddQueryOption%2A>. Recomendamos usar o método LINQ <xref:System.Linq.Enumerable.Select%2A> para que o cliente gere a opção de consulta `$select` no URI de solicitação.

<a name="executingQueries"></a>

## <a name="client-versus-server-execution"></a>Execução do cliente e execução do servidor

O cliente executa uma consulta em duas partes. Sempre que possível, as expressões em uma consulta são avaliadas primeiro no cliente; em seguida, uma consulta baseada em URI é gerada e enviada ao serviço de dados para avaliação em relação aos dados no serviço. Considere a seguinte consulta LINQ:

[!code-csharp[Astoria Northwind Client#LinqQueryClientEvalSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#linqqueryclientevalspecific)]
[!code-vb[Astoria Northwind Client#LinqQueryClientEvalSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#linqqueryclientevalspecific)]

Neste exemplo, a expressão `(basePrice – (basePrice * discount))` é avaliada no cliente. Por essa razão, o URI de consulta real `http://localhost:12345/northwind.svc/Products()?$filter=(UnitPrice gt 90.00M) and substringof('bike',ProductName)`, que é enviado para o serviço de dados, contém o valor decimal já calculado de `90` na cláusula de filtro. Outras partes da expressão de filtragem, incluindo a expressão da subcadeia de caracteres, são avaliadas pelo serviço de dados. As expressões que são avaliadas no cliente seguem a semântica de Common Language Runtime (CLR), enquanto as expressões enviadas para o serviço de dados dependem da implementação do serviço de dados do protocolo OData. Você também deve estar ciente dos cenários onde essa avaliação separada pode causar resultados inesperados, por exemplo, quando o cliente e o serviço executam avaliações com base na hora em fusos horários diferentes.

## <a name="query-responses"></a>Respostas de consulta

Quando executada, a classe <xref:System.Data.Services.Client.DataServiceQuery%601> retorna <xref:System.Collections.Generic.IEnumerable%601> do tipo de entidade solicitado. Esse resultado da consulta pode ser convertido em um objeto <xref:System.Data.Services.Client.QueryOperationResponse%601>, como no exemplo a seguir:

[!code-csharp[Astoria Northwind Client#GetResponseSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#getresponsespecific)]
[!code-vb[Astoria Northwind Client#GetResponseSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#getresponsespecific)]

As instâncias de tipos de entidade que representam entidades no serviço de dados são criadas no cliente por um processo chamado materialização de objeto. Para obter mais informações, consulte [materialização de objeto](object-materialization-wcf-data-services.md). O objeto <xref:System.Data.Services.Client.QueryOperationResponse%601> implementa <xref:System.Collections.Generic.IEnumerable%601> para fornecer acesso aos resultados da consulta.

A classe <xref:System.Data.Services.Client.QueryOperationResponse%601> também possui membros a seguir que permitem acessar informações adicionais sobre um resultado da consulta:

- <xref:System.Data.Services.Client.OperationResponse.Error%2A> - obtém um erro gerado pela operação, caso algum tenha ocorrido.

- <xref:System.Data.Services.Client.OperationResponse.Headers%2A> - contém a coleção de cabeçalhos de resposta HTTP associados à resposta da consulta.

- <xref:System.Data.Services.Client.QueryOperationResponse.Query%2A> - obtém a classe <xref:System.Data.Services.Client.DataServiceQuery%601> original que gerou <xref:System.Data.Services.Client.QueryOperationResponse%601>.

- <xref:System.Data.Services.Client.OperationResponse.StatusCode%2A> - obtém o código de resposta HTTP para a resposta da consulta.

- <xref:System.Data.Services.Client.QueryOperationResponse%601.TotalCount%2A> - obtém o número total de entidades no conjunto de entidades quando o método <xref:System.Data.Services.Client.DataServiceQuery%601.IncludeTotalCount%2A> foi chamado na classe <xref:System.Data.Services.Client.DataServiceQuery%601>.

- <xref:System.Data.Services.Client.QueryOperationResponse.GetContinuation%2A> - retorna um objeto <xref:System.Data.Services.Client.DataServiceQueryContinuation> que contém o URI da próxima página de resultados.

Por padrão, WCF Data Services retorna apenas os dados que são explicitamente selecionados pelo URI de consulta. Isso oferece a opção de carregar explicitamente dados adicionais do serviço de dados quando necessário. Uma solicitação será enviada para o serviço de dados toda vez que você carregar explicitamente dados do serviço de dados. Os dados que podem ser explicitamente carregados incluem entidades relacionadas, dados de respostas paginadas e fluxos de dados binários.

> [!NOTE]
> Como um serviço de dados pode retornar uma resposta paginada, recomendamos que seu aplicativo use o padrão de programação para manipular uma resposta do serviço de dados paginados. Para obter mais informações, consulte [carregando conteúdo adiado](loading-deferred-content-wcf-data-services.md).

Para reduzir a quantidade de dados retornada por uma consulta, é possível especificar que apenas determinadas propriedades de uma entidade sejam retornadas na resposta. Para obter mais informações, consulte [projeções de consulta](query-projections-wcf-data-services.md).

## <a name="getting-a-count-of-the-total-number-of-entities-in-the-set"></a>Obtendo uma contagem do número total de entidades no conjunto

Em alguns cenários, é útil saber o número total de entidades em um conjunto de entidades e não simplesmente o número retornado pela consulta. Chame o método <xref:System.Data.Services.Client.DataServiceQuery%601.IncludeTotalCount%2A> em <xref:System.Data.Services.Client.DataServiceQuery%601> para solicitar que essa contagem total de entidades no conjunto seja incluída no resultado da consulta. Nesse caso, a propriedade <xref:System.Data.Services.Client.QueryOperationResponse%601.TotalCount%2A> da classe <xref:System.Data.Services.Client.QueryOperationResponse%601> retornada retorna o número total de entidades no conjunto.

Você também pode obter apenas a contagem total de entidades no conjunto como um valor <xref:System.Int32> ou <xref:System.Int64> chamando os métodos <xref:System.Linq.Enumerable.Count%2A> ou <xref:System.Linq.Enumerable.LongCount%2A>, respectivamente. Quando esses métodos são chamados, uma classe <xref:System.Data.Services.Client.QueryOperationResponse%601> não é retornada; somente o valor da contagem é retornado. Para obter mais informações, consulte [como: determinar o número de entidades retornadas por uma consulta](number-of-entities-returned-by-a-query-wcf.md).

## <a name="in-this-section"></a>Nesta seção

- [Projeções de consulta](query-projections-wcf-data-services.md)

- [Materialização de objetos](object-materialization-wcf-data-services.md)

- [Considerações sobre o LINQ](linq-considerations-wcf-data-services.md)

- [Como executar consultas de serviço de dados](how-to-execute-data-service-queries-wcf-data-services.md)

- [Como adicionar opções de consulta para uma consulta de serviço de dados](how-to-add-query-options-to-a-data-service-query-wcf-data-services.md)

- [Como determinar o número de entidades retornadas por uma consulta](number-of-entities-returned-by-a-query-wcf.md)

- [Como especificar as credenciais do cliente para uma solicitação de serviço de dados](specify-client-creds-for-a-data-service-request-wcf.md)

- [Como definir os cabeçalhos da solicitação do cliente](how-to-set-headers-in-the-client-request-wcf-data-services.md)

- [Como fazer para projetar resultados de consulta](how-to-project-query-results-wcf-data-services.md)

## <a name="see-also"></a>Veja também

- [Biblioteca de cliente do WCF Data Services](wcf-data-services-client-library.md)
