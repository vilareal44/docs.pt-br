---
title: Realizando marshaling em dados com interoperabilidade COM
description: Consulte os artigos que abordam o marshaling de dados com interoperabilidade COM. As ferramentas Tlbimp.exe e Tlbexp.exe são convertidas entre uma biblioteca de tipos COM e um assembly de interoperabilidade.
ms.date: 09/07/2017
helpviewer_keywords:
- COM interop, data marshaling
- marshaling data, COM interop
ms.openlocfilehash: eedfb60a75e2fe5fafdaa786dbb54adddf28400e
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621503"
---
# <a name="marshaling-data-with-com-interop"></a>Realizando marshaling em dados com interoperabilidade COM
Interoperabilidade COM dá suporte ao uso de objetos COM por código gerenciado e à exposição de objetos gerenciados para COM. O suporte a marshaling dos dados de e para o COM é abrangente e quase sempre proporciona o comportamento de marshaling correto.  
  
 O SDK do Windows inclui as seguintes ferramentas de interoperabilidade COM:  
  
- [Importador de biblioteca de tipos (Tlbimp.exe)](../tools/tlbimp-exe-type-library-importer.md), que converte uma biblioteca de tipos COM para um assembly de interoperabilidade. Desse assembly, o serviço de marshaling de interoperabilidade gera wrappers que realizam marshaling entre memória gerenciada e não gerenciada.  
  
- [Digite o exportador da biblioteca (Tlbexp.exe)](../tools/tlbexp-exe-type-library-exporter.md), que produz uma biblioteca de tipos COM de um assembly e gera um wrapper que realiza marshaling durante as chamadas de método.  
  
 As seções a seguir são vinculadas a tópicos que descrevem os processos para personalizar os wrappers de interoperabilidade quando você pode (ou precisa) fornecer informações de tipo adicionais ao marshaler.  
  
## <a name="in-this-section"></a>Nesta seção  
[Como criar wrappers manualmente](how-to-create-wrappers-manually.md) Descreve como criar um wrapper COM manualmente no código-fonte gerenciado.

 [Como: Migrar código DCOM gerenciado para o WCF](how-to-migrate-managed-code-dcom-to-wcf.md)  
 Descreve como migrar o código DCOM gerenciado para o WCF para obter a solução mais segura possível.  
  
## <a name="related-sections"></a>Seções relacionadas  
 [Tipos de dados COM](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/sak564ww(v=vs.100))  
 Fornece tipos de dados gerenciados e não gerenciados correspondentes.  
  
 [Personalizando COM Callable Wrappers](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/3bwc828w(v=vs.100))  
 Descreve como realizar marshaling nos tipos de dados explicitamente usando o atributo <xref:System.Runtime.InteropServices.MarshalAsAttribute> em tempo de design.  
  
 [Personalizando RCWs (Runtime Callable Wrappers)](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/e753eftz(v=vs.100))  
 Descreve como ajustar o comportamento de marshaling de tipos em um assembly de interoperabilidade e como definir tipos COM manualmente.  
  
 [Interoperabilidade COM avançada](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bd9cdfyx(v=vs.100))  
 Fornece links para obter mais informações sobre como incorporar componentes COM no aplicativo do .NET Framework.  
  
 [Resumo da conversão de assemblies em bibliotecas de tipos](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/xk1120c3(v=vs.100))  
 Descreve o processo de conversão de exportação de assembly em biblioteca de tipos.  
  
 [Resumo da conversão de bibliotecas de tipos em assemblies](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/k83zzh38(v=vs.100))  
 Descreve o processo de conversão de importação de biblioteca de tipos em assembly.  
  
 [Interoperação usando tipos genéricos](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms229590(v=vs.100))  
 Descreve quais ações têm suporte ao usar tipos genéricos para interoperabilidade COM.
