---
title: Configurando classes de criptografia
description: Entenda como os administradores de computadores podem configurar os algoritmos de criptografia e as implementações de algoritmo padrão que o .NET e os aplicativos usam.
ms.date: 03/30/2017
helpviewer_keywords:
- configuration files [.NET Framework], cryptography
- cryptographic algorithms
- security [.NET Framework], encryption
- cryptography, classes
- .NET Framework application configuration, cryptography
- default cryptography
ms.assetid: eee3ccb8-2c0d-4f35-b38d-6892a46c14e5
ms.openlocfilehash: 5d12aae31ec78f80bea7df1bb0f37ac78dc37de2
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85105064"
---
# <a name="configuring-cryptography-classes"></a>Configurando classes de criptografia
O SDK do Windows permite que os administradores de computador configurem os algoritmos de criptografia e as implementações de algoritmo padrão que o .NET Framework e os aplicativos escritos de forma apropriada usam.  Por exemplo, uma empresa que tem sua própria implementação de um algoritmo criptográfico pode tornar essa implementação o padrão em vez da implementação fornecida no SDK do Windows. Embora os aplicativos gerenciados que usam a criptografia possam sempre optar por ligar explicitamente a uma implementação específica, é recomendável que eles criem objetos criptográficos usando o sistema de configuração de criptografia.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Mapeando nomes de algoritmo para classes de criptografia](map-algorithm-names-to-cryptography-classes.md)  
 Descreve como mapear um nome de algoritmo para uma classe de criptografia.  
  
 [Mapeando identificadores de objeto para algoritmos de criptografia](map-object-identifiers-to-cryptography-algorithms.md)  
 Descreve como mapear um identificador de objeto para um algoritmo de criptografia.  
  
## <a name="related-sections"></a>Seções relacionadas  
 [Serviços de Criptografia](../../standard/security/cryptographic-services.md)  
 Fornece uma visão geral dos serviços de criptografia fornecidos pelo SDK do Windows.  
  
 [Esquema de configurações de criptografia](./file-schema/cryptography/index.md)  
 Descreve os elementos que mapeiam nomes amigáveis de algoritmo a classes que implementam algoritmos de criptografia.
