---
title: Usar System.Transactions no ASP.NET
description: Use System. Transactions dentro de um aplicativo ASP.NET. Habilite as permissões de transação distribuída e trabalhe com a compilação dinâmica.
ms.date: 03/30/2017
ms.assetid: 1982c300-7ea6-4242-95ed-dc28ccfacac9
ms.openlocfilehash: 48907faf99953e34d045a1e0d79eed8a788187b5
ms.sourcegitcommit: 6219b1e1feccb16d88656444210fed3297f5611e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85141638"
---
# <a name="using-systemtransactions-in-aspnet"></a>Usar System.Transactions no ASP.NET
Este tópico descreve como você pode usar com êxito <xref:System.Transactions> dentro de um aplicativo ASP.net.

## <a name="enable-distributedtransactionpermission-in-aspnet"></a>Habilitar DistributedTransactionPermission no ASP.NET
 <xref:System.Transactions>dá suporte a chamadores parcialmente confiáveis e é marcado com o `AllowPartiallyTrustedCallers` atributo (APTCA). Os níveis de confiança para <xref:System.Transactions> são definidos com base nos tipos de recursos (por exemplo, memória do sistema, recursos compartilhados de todo o processo, recursos de todo o sistema e outros recursos) que <xref:System.Transactions> expõe e o nível de confiança que deve ser necessário para acessar esses recursos. Em um ambiente de confiança parcial, um assembly de confiança total não pode usar somente as transações dentro do domínio de aplicativo (nesse caso, o único recurso que está sendo protegido é a memória do sistema), a menos que ele recebe o <xref:System.Transactions.DistributedTransactionPermission>.

 <xref:System.Transactions.DistributedTransactionPermission>é exigido sempre que o gerenciamento de transações é escalonado para ser gerenciado pelo Microsoft Coordenador de Transações Distribuídas (MSDTC). Esse tipo de cenário usa recursos de todo o processo e especialmente um recurso global, que é o espaço reservado no log do MSDTC. Um exemplo dessa utilização é uma Web front-end para um banco de dados ou um aplicativo que usa um banco de dados como parte dos serviços que ele fornece.

 O ASP.NET tem seu próprio conjunto de níveis de confiança e associa um conjunto específico de permissões a esses níveis de confiança por meio de arquivos de política. Para obter mais informações, consulte [níveis de confiança e arquivos de política do ASP.net](https://docs.microsoft.com/previous-versions/aspnet/wyts434y(v=vs.100)). Ao instalar inicialmente o SDK do Windows, nenhum dos arquivos de política ASP.NET padrão está associado ao <xref:System.Transactions.DistributedTransactionPermission> . Dessa forma, quando sua transação em um aplicativo ASP.NET é escalonada para ser gerenciada pelo MSDTC, o escalonamento falha com um <xref:System.Security.SecurityException> após exigir o <xref:System.Transactions.DistributedTransactionPermission> . Para habilitar o escalonamento de transações em um ambiente de confiança parcial ASP.NET, você deve conceder o <xref:System.Transactions.DistributedTransactionPermission> nos mesmos níveis de confiança padrão que os do <xref:System.Data.SqlClient.SqlClientPermission> . Você pode configurar seu próprio nível de confiança personalizado e arquivo de política para dar suporte a isso, ou pode modificar os arquivos de política padrão, que são **Web_hightrust.config** e **Web_mediumtrust.config**.

 Para modificar os arquivos de política, adicione um `SecurityClass` elemento para `DistributedTransactionPermission` ao `SecurityClasses` elemento sob o `PolicyLevel` elemento e adicione um `IPermission` elemento correspondente sob o ASP.NET `NamedPermissionSet` para System. Transactions. O arquivo de configuração a seguir demonstra isso.

```xml
<SecurityClasses>
   <SecurityClass Name="DistributedTransactionPermission" Description="System.Transactions.DistributedTransactionPermission, System.Transactions, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"/>
...
</SecurityClasses>

<PermissionSet
  class="NamedPermissionSet"
  version="1"
  Name="ASP.Net">
     <IPermission
        class="System.Transactions.DistributedTransactionPermission, System.Transactions, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"
        version="1"
        Unrestricted="true"
     />
...
</PermissionSet>
```

 Para obter mais informações sobre a política de segurança do ASP.NET, consulte [elemento SecurityPolicy (esquema de configurações do ASP.net)](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/zhs35b56(v=vs.100)).

## <a name="dynamic-compilation"></a>Compilação dinâmica
 Se você quiser importar e usar <xref:System.Transactions> o em um aplicativo ASP.NET que é compilado dinamicamente no Access, você deve inserir uma referência ao <xref:System.Transactions> assembly no arquivo de configuração. Especificamente, a referência deve ser adicionada na `compilation/assemblies` seção do arquivo de configuração de **Web.config** raiz padrão ou em um arquivo de configuração de aplicativo Web específico. O exemplo a seguir demonstra isso.

```xml
<configuration>
   <system.web>
      <compilation>
         <assemblies>
      <add assembly="System.Transactions, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
         </assemblies>
      </compilation>
   </system.web>
</configuration>
```

 Para obter mais informações, consulte [Adicionar elemento para assemblies para compilação (esquema de configurações de ASP.net)](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/37e2zyhb(v=vs.100)).

## <a name="see-also"></a>Veja também

- [ASP.NET de níveis de confiança e arquivos de política](https://docs.microsoft.com/previous-versions/aspnet/wyts434y(v=vs.100))
- [Elemento securityPolicy (esquema de configurações de ASP.NET)](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/zhs35b56(v=vs.100))
- [Escalonamento de gerenciamento de transações](transaction-management-escalation.md)
