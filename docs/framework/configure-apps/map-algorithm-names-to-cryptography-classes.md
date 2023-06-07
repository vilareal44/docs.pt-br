---
title: Mapeando nomes de algoritmo para classes de criptografia
description: Mapeie os nomes de algoritmos para classes de criptografia no .NET. Um desenvolvedor tem quatro opções para criar um objeto de criptografia.
ms.date: 03/30/2017
helpviewer_keywords:
- mapping algorithm names
- cryptography, mapping algorithm names
- cryptographic algorithms
- names [.NET Framework], algorithm mapping
ms.assetid: 01327c69-c5e1-4ef6-b73f-0a58351f0492
ms.openlocfilehash: 5a1d7acdd34182dd82f4dce66d136c4ef4de6e95
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85105346"
---
# <a name="mapping-algorithm-names-to-cryptography-classes"></a>Mapeando nomes de algoritmo para classes de criptografia
Há quatro maneiras pelas quais um desenvolvedor pode criar um objeto de criptografia usando o SDK do Windows:  
  
- Crie um objeto usando o operador **New** .  
  
- Crie um objeto que implemente um algoritmo de criptografia específico chamando o método **Create** na classe abstrata para esse algoritmo.  
  
- Crie um objeto que implemente um algoritmo de criptografia específico chamando o <xref:System.Security.Cryptography.CryptoConfig.CreateFromName%2A?displayProperty=nameWithType> método.  
  
- Crie um objeto que implemente uma classe de algoritmos criptográficos (como uma codificação de bloco simétrico) chamando o método **Create** na classe abstrata para esse tipo de algoritmo (como <xref:System.Security.Cryptography.SymmetricAlgorithm> ).  
  
 Por exemplo, suponha que um desenvolvedor queira calcular o hash SHA1 de um conjunto de bytes. O <xref:System.Security.Cryptography> namespace contém duas implementações do algoritmo SHA1, uma implementação puramente gerenciada e outra que encapsula a CryptoAPI. O desenvolvedor pode optar por criar uma instância de uma implementação SHA1 específica (como a <xref:System.Security.Cryptography.SHA1Managed> ) chamando o operador **New** . No entanto, se não importa qual classe o Common Language Runtime carrega contanto que a classe implemente o algoritmo hash SHA1, o desenvolvedor pode criar um objeto chamando o <xref:System.Security.Cryptography.SHA1.Create%2A?displayProperty=nameWithType> método. Esse método chama **System. Security. Cryptography. CryptoConfig. CreateFromName ("System. Security. Cryptography. SHA1")**, que deve retornar uma implementação do algoritmo de hash SHA1.  
  
 O desenvolvedor também pode chamar **System. Security. Cryptography. CryptoConfig ("SHA1")** porque, por padrão, a configuração de criptografia inclui nomes curtos para os algoritmos fornecidos no .NET Framework.  
  
 Se não importa qual algoritmo de hash é usado, o desenvolvedor pode chamar o <xref:System.Security.Cryptography.HashAlgorithm.Create%2A?displayProperty=nameWithType> método, que retorna um objeto que implementa uma transformação de hash.  
  
## <a name="mapping-algorithm-names-in-configuration-files"></a>Mapeando nomes de algoritmos em arquivos de configuração  
 Por padrão, o tempo de execução retorna um <xref:System.Security.Cryptography.SHA1CryptoServiceProvider> objeto para todos os quatro cenários. No entanto, um administrador de máquina pode alterar o tipo de objeto que os métodos nos dois últimos cenários retornam. Para fazer isso, você deve mapear um nome de algoritmo amigável para a classe que deseja usar no arquivo de configuração de computador (Machine.config).  
  
 O exemplo a seguir mostra como configurar o tempo de execução para que **System. Security. Cryptography. SHA1. Create**, **System. Security. CryptoConfig. CreateFromName ("SHA1")** e **System. Security. Cryptography. HashAlgorithm. Create** retorne um `MySHA1HashClass` objeto.  
  
```xml  
<configuration>  
   <!-- Other configuration settings. -->  
   <mscorlib>  
      <cryptographySettings>  
         <cryptoNameMapping>  
            <cryptoClasses>  
               <cryptoClass MySHA1Hash="MySHA1HashClass, MyAssembly  
                  Culture='en', PublicKeyToken=a5d015c7d5a0b012,  
                  Version=1.0.0.0"/>  
            </cryptoClasses>  
            <nameEntry name="SHA1" class="MySHA1Hash"/>  
            <nameEntry name="System.Security.Cryptography.SHA1"  
                       class="MySHA1Hash"/>  
            <nameEntry name="System.Security.Cryptography.HashAlgorithm"  
                       class="MySHA1Hash"/>  
         </cryptoNameMapping>  
      </cryptographySettings>  
   </mscorlib>  
</configuration>  
```  
  
 Você pode especificar o nome do atributo no [ \> elemento<cryptoClass](./file-schema/cryptography/cryptoclass-element.md) (o exemplo anterior nomeia o atributo `MySHA1Hash` ). O valor do atributo no **\<cryptoClass>** elemento é uma cadeia de caracteres que o Common Language Runtime usa para localizar a classe. Você pode usar qualquer cadeia de caracteres que atenda aos requisitos especificados na [especificação de nomes de tipo totalmente qualificados](../reflection-and-codedom/specifying-fully-qualified-type-names.md).  
  
 Muitos nomes de algoritmos podem ser mapeados para a mesma classe. O [ \<nameEntry> elemento](./file-schema/cryptography/nameentry-element.md) mapeia uma classe para um nome de algoritmo amigável. O atributo **Name** pode ser uma cadeia de caracteres que é usada ao chamar o método **System. Security. Cryptography. CryptoConfig. CreateFromName** ou o nome de uma classe de criptografia abstrata no <xref:System.Security.Cryptography> namespace. O valor do atributo **Class** é o nome do atributo no **\<cryptoClass>** elemento.  
  
> [!NOTE]
> Você pode obter um algoritmo SHA1 chamando o <xref:System.Security.Cryptography.SHA1.Create%2A?displayProperty=nameWithType> ou o método **Security. CryptoConfig. CreateFromName ("SHA1")** . Cada método garante apenas que ele retorne um objeto que implementa o algoritmo SHA1. Você não precisa mapear cada nome amigável de um algoritmo para a mesma classe no arquivo de configuração.  
  
 Para obter uma lista de nomes padrão e as classes que eles mapeiam, consulte <xref:System.Security.Cryptography.CryptoConfig> .  
  
## <a name="see-also"></a>Veja também

- [Serviços de Criptografia](../../standard/security/cryptographic-services.md)
- [Configurando classes de criptografia](configure-cryptography-classes.md)
