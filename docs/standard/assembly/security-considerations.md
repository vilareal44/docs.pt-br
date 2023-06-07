---
title: Considerações sobre a segurança do assembly
description: Ao criar um assembly .NET, você pode especificar as permissões que o assembly requer para ser executado. Este artigo aborda os assemblies de nome forte e as ferramentas de assinatura.
ms.date: 08/20/2019
helpviewer_keywords:
- assemblies [.NET Framework], security
- signcodes
- names [.NET Framework], assemblies
- strong-named assemblies, security considerations
- signing assemblies
- assemblies [.NET Framework], signing
- granting permissions, assemblies
- assemblies [.NET Framework], strong-named
- names [.NET Framework], strong names
- permissions [.NET Framework], assemblies
- security [.NET Framework], assemblies
- integrity with assemblies
ms.assetid: 1b5439c1-f3d5-4529-bd69-01814703d067
ms.openlocfilehash: d0b822f725444248d1037c21ffea9afe1cebb7e2
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290013"
---
# <a name="assembly-security-considerations"></a>Considerações sobre a segurança do assembly
Ao criar um assembly, você pode especificar um conjunto de permissões que o assembly exige para ser executado. Se determinadas permissões são concedidas ou não a um assembly é algo que se baseia na evidência.  
  
 Existem duas formas distintas de usar a evidência:  
  
- A evidência de entrada é mesclada com a evidência coletada pelo carregador para criar um conjunto final de evidências usado na resolução da política. Entre os métodos que usam essa semântica estão **Assembly.Load**, **Assembly.LoadFrom** e **Activator.CreateInstance**.  
  
- A evidência de entrada é usada inalterada como o conjunto final de evidências usado na resolução da política. Entre os métodos que usam essa semântica estão **Assembly.Load(byte[])** e **AppDomain.DefineDynamicAssembly()**.  
  
  Permissões opcionais podem ser concedidas pelo conjunto de [políticas de segurança](../../framework/misc/code-access-security-basics.md) no computador em que o assembly será executado. Se quiser que o código identifique todas as exceções de segurança em potencial, você poderá seguir um destes procedimentos:  
  
- Insira uma solicitação de permissão para todas as permissões que seu código deve ter e identifique antecipadamente a falha de tempo de carga que ocorrerá se as permissões não forem concedidas.  
  
- Não use uma solicitação de permissão para obter permissões de que seu código possa precisar, mas esteja preparado para identificar exceções de segurança se as permissões não forem concedidas.  
  
  > [!NOTE]
  > Segurança é uma área complexa, e você tem várias opções para escolher. Para saber mais, confira [Conceitos-chave sobre segurança](../security/key-security-concepts.md).  
  
 No momento do carregamento, a evidência do assembly é usada como entrada para uma política de segurança. A política de segurança é estabelecida pela empresa e pelo administrador do computador, bem como por configurações de política do usuário e determina o conjunto de permissões concedido a todo o código gerenciado quando executado. A política de segurança pode ser estabelecida para o editor do assembly (se ele tiver uma assinatura gerada por uma ferramenta de assinatura), para o site e a zona (em termos do Internet Explorer) de onde o assembly foi baixado, ou para o nome forte do assembly. Por exemplo, um administrador de computador pode estabelecer uma política de segurança que permite que todo código baixado de um site e assinado por uma determinada empresa de software acesse um banco de dados em um computador, mas não dê acesso para gravar no disco do computador.  
  
## <a name="strong-named-assemblies-and-signing-tools"></a>Assemblies de nome forte e ferramentas de assinatura  

 > [!WARNING]
 > Por segurança, não confie em nomes fortes. Eles apenas fornecem uma identidade exclusiva.

 Você pode assinar um assembly de duas maneiras diferentes, mas complementares: com um nome forte ou usando [SignTool.exe (Ferramenta de assinatura)](../../framework/tools/signtool-exe.md). Assinar um assembly com um nome forte adiciona uma criptografia de chave pública ao arquivo que contém o manifesto do assembly. Assinatura de nome forte ajuda a verificar a exclusividade do nome, a evitar falsificação de nome e a fornecer chamadores com uma identidade quando uma referência é resolvida.  
  
 Nenhum nível de confiança está associado a um nome forte, o que torna a [SignTool.exe (Ferramenta de Assinatura)](../../framework/tools/signtool-exe.md) importante. As duas ferramentas de assinatura exigem um editor para comprovar sua identidade para uma autoridade de terceiros e obter um certificado. Assim, esse certificado é inserido no seu arquivo e pode ser usado por um administrador para decidir se confia na autenticidade do código.  
  
 Você pode fornecer tanto um nome forte quanto uma assinatura digital criada usando-se o [SignTool.exe (Ferramenta de assinatura)](../../framework/tools/signtool-exe.md) a um assembly, ou pode usar qualquer um sozinho. As duas ferramentas de assinatura podem assinar somente um arquivo por vez; para um assembly com vários arquivos, você assina o arquivo que contém o manifesto do assembly. Um nome forte é armazenado no arquivo que contém o manifesto do assembly, mas uma assinatura criada com o [SignTool.exe (Ferramenta de assinatura)](../../framework/tools/signtool-exe.md) é armazenada em um slot reservado no arquivo PE (Portable Executable) contendo a manifesto do assembly. A assinatura de um assembly usando o [SignTool.exe (Ferramenta de assinatura)](../../framework/tools/signtool-exe.md) pode ser usada (com ou sem um nome forte) quando você já tem uma hierarquia de confiança que se baseia em assinaturas geradas por [SignTool.exe (Ferramenta de assinatura)](../../framework/tools/signtool-exe.md) ou quando a sua política usa somente a parte da chave e não verifica uma cadeia de confiança.  
  
> [!NOTE]
> Ao usar um nome forte e uma assinatura gerada por uma ferramenta de assinatura em um assembly, o nome forte deve ser atribuído primeiro.  
  
 O Common Language Runtime também executa uma verificação de hash; o manifesto do assembly contém uma lista de todos os arquivos que compõem o assembly, incluindo um hash de cada arquivo conforme ele existia quando o manifesto foi compilado. À medida que cada arquivo é carregado, seu conteúdo recebe um hash e é comparado com o valor de hash armazenado no manifesto. Se os dois hashes não corresponderem, o assembly não será carregado.  
  
 Como a nomenclatura forte e a assinatura usando o [SignTool.exe (Ferramenta de assinatura)](../../framework/tools/signtool-exe.md) garantem integridade, você pode basear a política de segurança de acesso a códigos nessas duas formas de evidência do assembly. Nomenclatura forte e assinatura usando o [SignTool.exe (Ferramenta de Assinatura)](../../framework/tools/signtool-exe.md) garantem a integridade por meio de certificados e assinaturas digitais. Todas as tecnologias mencionadas — verificação de hash, nomenclatura forte e assinatura usando o [SignTool.exe (Ferramenta de Assinatura)](../../framework/tools/signtool-exe.md) — funcionam juntas para garantir que o assembly não tenha sido alterado de forma alguma.  
  
## <a name="see-also"></a>Veja também

- [Assemblies de nome forte](strong-named.md)
- [Assemblies no .NET](index.md)
- [SignTool. exe (ferramenta de assinatura)](../../framework/tools/signtool-exe.md)
