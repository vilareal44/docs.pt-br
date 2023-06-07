---
title: Protegendo aplicativos
ms.date: 03/30/2017
ms.assetid: 005a1d43-6ee5-471e-ad98-1d30a44d49d5
ms.openlocfilehash: 1e08bb2386dff5d824d46aba652609ec5a373008
ms.sourcegitcommit: e7748001b1cee80ced691d8a76ca814c0b02dd9b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/14/2020
ms.locfileid: "86374514"
---
# <a name="securing-adonet-applications"></a>Protegendo aplicativos ADO.NET

Escrever um aplicativo seguro do ADO.NET envolve mais do que evitar armadilhas comuns de codificação como não validar a entrada do usuário. Um aplicativo que acessa dados tem muitos pontos potenciais de falha que um invasor pode explorar para recuperar, manipular, ou destruir dados confidenciais. Portanto, é importante compreender todos os aspectos de segurança, do processo de modelagem de ameaças durante a fase de projeto do aplicativo até a sua eventual implantação e manutenção contínua.  
  
O .NET Framework fornece muitas classes, serviços e ferramentas úteis para administrar e proteger os aplicativos de banco de dados. O CLR (Common Language Runtime) fornece um ambiente de tipo seguro no qual o código será executado, com CAS (segurança de acesso ao código) para restringir mais as permissões do código gerenciado. Seguir práticas de codificação segura de acesso a dados limita o dano que pode ser infligido por um invasor potencial.  
  
Escrever código seguro não protege contra buracos na segurança autoinfligidos ao trabalhar com recursos não gerenciados como bancos de dados. A maioria dos bancos de dados do servidor, como SQL Server, têm seus próprios sistemas de segurança, que melhoram a segurança quando implementados corretamente. No entanto, até mesmo uma fonte de dados com um sistema de segurança robusto pode ser prejudicada em um ataque se não estiver configurada adequadamente.  
  
## <a name="in-this-section"></a>Nesta seção

 [Visão geral de segurança](security-overview.md)  
 Fornece recomendações para criar aplicativos seguros do ADO.NET.  
  
 [Acesso seguro a dados](secure-data-access.md)  
 Descreve como trabalhar com dados de uma fonte de dados segura.  
  
 [Aplicativos cliente seguros](secure-client-applications.md)  
 Descreve as considerações de segurança para aplicativos cliente.  
  
 [Segurança de acesso do código e o ADO.NET](code-access-security.md)  
 Descreve como o CAS pode ajudar a proteger o código do ADO.NET. Também descreve como trabalhar com confiança parcial.  
  
 [Privacidade e segurança de dados](privacy-and-data-security.md)  
 Descreve as opções de criptografia para aplicativos ADO.NET.  
  
## <a name="related-sections"></a>Seções relacionadas

 [Diretrizes de segurança do conjunto de tabela e DataTable](dataset-datatable-dataview/security-guidance.md)  
 Fornece diretrizes de segurança para o <xref:System.Data.DataSet> e o <xref:System.Data.DataTable> .

 [Segurança de SQL Server](./sql/sql-server-security.md)  
 Descreve os recursos de segurança do SQL Server da perspectiva de um desenvolvedor.  
  
 [Considerações sobre segurança](./ef/security-considerations.md)  
 Descreve a segurança para aplicativos do Entity Framework.  
  
 [Segurança](../../../standard/security/index.md)  
 Contém links para artigos que descrevem todos os aspectos de segurança no .NET.  
  
 [Ferramentas de segurança](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/7w3fd0wb(v=vs.90))  
 As ferramentas do .NET Framework para proteger e administrar a política de segurança.  
  
 [Recursos para criar aplicativos seguros](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/ms165101(v=vs.100))  
 Fornece links para artigos para criar aplicativos seguros.  
  
 [Bibliografia de segurança](/visualstudio/ide/securing-applications)  
 Fornece links para recursos externos disponíveis online e em cópia impressa.  
  
## <a name="see-also"></a>Confira também

- [ADO.NET](index.md)
- [Visão geral do ADO.NET](ado-net-overview.md)
