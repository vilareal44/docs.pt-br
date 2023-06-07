---
title: Mapeando identificadores de objeto para algoritmos de criptografia
description: Consulte como mapear um OID (identificador de objeto) para um algoritmo de criptografia no .NET usando os elementos oidEntry e nameEntry em um arquivo de configuração XML.
ms.date: 03/30/2017
helpviewer_keywords:
- digital signatures
- identifiers, mapping object identifiers
- cryptographic algorithms
- mapping object identifiers
- cryptography, mapping object identifiers
ms.assetid: c9673f81-bf9e-47fd-bc6f-6bc1c1c4c15e
ms.openlocfilehash: e22510014071455b83ba28cd82690b5ecdce9bc9
ms.sourcegitcommit: 6219b1e1feccb16d88656444210fed3297f5611e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85141998"
---
# <a name="mapping-object-identifiers-to-cryptography-algorithms"></a>Mapeando identificadores de objeto para algoritmos de criptografia
As assinaturas digitais garantem que os dados não sejam adulterados quando enviados de um programa para outro. Normalmente, a assinatura digital é computada aplicando uma função matemática ao hash dos dados a serem assinados. Ao Formatar um valor de hash a ser assinado, alguns algoritmos de assinatura digital acrescentam um OID (identificador de objeto) de ASN. 1 como parte da operação de formatação. O OID identifica o algoritmo que foi usado para calcular o hash. Você pode mapear algoritmos para identificadores de objeto para estender o mecanismo de criptografia para usar algoritmos personalizados. O exemplo a seguir mostra como mapear um identificador de objeto para um novo algoritmo de hash.  
  
```xml  
<configuration>  
   <mscorlib>  
      <cryptographySettings>  
         <cryptoNameMapping>  
            <cryptoClasses>  
               <cryptoClass MyNewHash="MyNewHashClass, MyAssembly  
                  Culture='en', PublicKeyToken=a5d015c7d5a0b012,  
                  Version=1.0.0.0"/>  
            </cryptoClasses>  
            <nameEntry name="NewHash" class="MyNewHash"/>  
         </cryptoNameMapping>  
         <oidMap>  
            <oidEntry OID="1.3.14.33.42.46"  name="NewHash"/>  
         </oidMap>  
      </cryptographySettings>  
   </mscorlib>  
</configuration>  
```  
  
 O [ \<oidEntry> elemento](./file-schema/cryptography/oidentry-element.md) contém dois atributos. O atributo **OID** é o número do identificador de objeto. O atributo **Name** é o valor do atributo **Name** do [ \<nameEntry> elemento](./file-schema/cryptography/nameentry-element.md). Deve haver um mapeamento de um nome de algoritmo para uma classe antes que um identificador de objeto possa ser mapeado para um nome simples.  
  
## <a name="see-also"></a>Veja também

- [Configurando classes de criptografia](configure-cryptography-classes.md)
- [Serviços de Criptografia](../../standard/security/cryptographic-services.md)
