---
title: Sn.exe (Ferramenta de Nome Forte)
description: Introdução ao Sn.exe, a ferramenta de nome forte. Assine assemblies com nomes fortes. Gerenciar chaves e gerar e verificar assinaturas.
ms.date: 03/30/2017
helpviewer_keywords:
- public keys, signing files
- Strong Name tool
- Sn.exe
- assemblies [.NET Framework], signing
- signing files
- strong-named assemblies, signing files
- key pairs for signing files
ms.assetid: c1d2b532-1b8e-4c7a-8ac5-53b801135ec6
ms.openlocfilehash: 8f10dab9b395640e46cb9bf3ca468b8f6bb2bc1b
ms.sourcegitcommit: b4f8849c47c1a7145eb26ce68bc9f9976e0dbec3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87517185"
---
# <a name="snexe-strong-name-tool"></a>Sn.exe (Ferramenta de Nome Forte)
A ferramenta Nome Forte (Sn.exe) ajuda a assinar assemblies com [nomes fortes](../../standard/assembly/strong-named.md). Sn.exe oferece opções para o gerenciamento de chaves, geração de assinaturas e verificação de assinaturas.  
  
> [!WARNING]
> Por segurança, não confie em nomes fortes. Eles apenas fornecem uma identidade exclusiva.

 Para obter mais informações sobre nomes fortes e assemblies com nome forte, consulte [Assemblies com nome forte](../../standard/assembly/strong-named.md) e [Como assinar um assembly com um nome forte](../../standard/assembly/sign-strong-name.md).  
  
 A ferramenta Nome Forte é instalada automaticamente com o Visual Studio. Para iniciar a ferramenta, use o Prompt de Comando do Desenvolvedor (ou o Prompt de Comando do Visual Studio no Windows 7). Para obter mais informações, consulte [Prompts de Comando](developer-command-prompt-for-vs.md).  

> [!NOTE]
> Em computadores 64 bits, execute a versão de 32 bits do Sn.exe usando o Prompt de Comando do Desenvolvedor para Visual Studio e a versão de 64 bits usando o Prompt de Comando Win64 x64 do Visual Studio.
  
 No prompt de comando, digite o seguinte:  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
sn [-quiet][option [parameter(s)]]  
```  
  
## <a name="parameters"></a>parâmetros  
  
|Opção|Descrição|  
|------------|-----------------|  
|`-a identityKeyPairFile signaturePublicKeyFile`|Gera dados <xref:System.Reflection.AssemblySignatureKeyAttribute> para migrar a chave de identidade para a chave de assinatura de um arquivo.|  
|`-ac identityPublicKeyFile identityKeyPairContainer signaturePublicKeyFile`|Gera dados <xref:System.Reflection.AssemblySignatureKeyAttribute> para migrar a chave de identidade para a chave de assinatura de um contêiner de chave.|  
|`-c [csp]`|Define o provedor de serviços de criptografia (CSP) padrão a ser usado na assinatura de nome forte. Essa configuração se aplica a todo o computador. Se você não especificar um nome CSP, Sn.exe limpará a configuração atual.|  
|`-d container`|Exclui o contêiner de chave especificado do CSP de nome forte.|  
|`-D assembly1 assembly2`|Verifica se dois assemblies são diferentes apenas na assinatura. Isso costuma ser usado como uma verificação após um assembly ser assinado novamente com um par de chaves diferente.|  
|`-e assembly outfile`|Extrai a chave pública do *assembly* e a armazena no *outfile*.|  
|`-h`|Exibe sintaxe de comando e opções para a ferramenta.|  
|`-i infile container`|Instala o par de chaves do *infile* no contêiner de chave especificado. O contêiner de chave reside no CSP de nome forte.|  
|`-k [keysize] outfile`|Gera uma nova chave <xref:System.Security.Cryptography.RSACryptoServiceProvider> com o tamanho especificado e a grava no arquivo especificado.  As chaves pública e privada são gravadas no arquivo.<br /><br /> Se não especificar um tamanho de chave, uma chave 1.024 bits será gerada por padrão se você tiver o provedor criptográfico aprimorado da Microsoft instalado; do contrário, uma chave de 512 bits será gerada.<br /><br /> O parâmetro *keysize* dá suporte a tamanhos de chave de 384 a 16.384 bits em incrementos de 8 bits se você tiver o provedor criptográfico aprimorado da Microsoft instalado.  Ele dá suporte a tamanhos de chave de 384 a 512 bits em incrementos de 8 bits se você tiver o provedor criptográfico de base da Microsoft instalado.|  
|`-m [y|n]`|Especifica se os contêineres de chave são específicos do computador ou do usuário. Se você definir *y*, os contêineres de chave serão específicos do computador. Se você especificar *n*, os contêineres de chave serão específicos ao usuário.<br /><br /> Se nem y nem n for especificado, essa opção exibirá a configuração atual.|  
|`-o infile [outfile]`|Extrai a chave pública do *infile* e a armazena em um arquivo .csv. Uma vírgula separa cada byte da chave pública. Esse formato é útil para referências codificadas para chaves como matrizes inicializadas no código-fonte. Se você não especificar um *outfile*, essa opção colocará a saída na Área de Transferência. **Observação:** essa opção não verifica se a entrada é somente uma chave pública. Se o `infile` contiver um par de chaves com uma chave privada, a chave privada também será extraída.|  
|`-p infile outfile [hashalg]`|Extrai a chave pública do par de chaves em *infile* e a armazena em *outfile*. Outra opção é usar o algoritmo RSA especificado por *hashalg*. Essa chave pública pode ser usada para assinar com atraso um assembly usando as opções **/delaysign+** e **/keyfile** do [Vinculador de Assembly (Al.exe)](al-exe-assembly-linker.md). Quando um assembly é assinado com atraso, somente a chave pública é definida no tempo de compilação e o espaço é reservado no arquivo para a assinatura a ser adicionada posteriormente, quando o chave privada será conhecida.|  
|`-pc container outfile [hashalg]`|Extrai a chave pública do par de chaves no *contêiner* e a armazena em *outfile*. Se você usar a opção *hashalg*, o algoritmo RSA será usado para extrair a chave pública.|  
|`-Pb [y|n]`|Especifica se a política de bypass de nome forte é imposta. Se você especificar *y*, os nomes fortes de assemblies de confiança total não serão validados em um <xref:System.AppDomain> de confiança total. Se você especificar *n*, a correção dos nomes fortes será validada, mas não para um nome forte específico. O <xref:System.Security.Permissions.StrongNameIdentityPermission> não tem efeito sobre assemblies de confiança total. Você deve realizar sua própria verificação de uma correspondência de nome forte.<br /><br /> Se nem `y` nem `n` for especificado, essa opção exibirá a configuração atual. O padrão é `y`. **Observação:** em computadores 64 bits, você deve definir esse parâmetro nas instâncias de 32 e 64 bits de Sn.exe.|  
|`-q[uiet]`|Especifica o modo silencioso; suprime a exibição de mensagens com êxito.|  
|`-R[a] assembly infile`|Assina novamente um assembly assinado anteriormente ou com atraso usando o par de chaves em *infile*.<br /><br /> Se **-Ra** for usado, os hashes serão recomputados para todos os arquivos no assembly.|  
|`-Rc[a] assembly container`|Assina novamente um assembly assinado anteriormente ou com atraso usando o par de chaves em *container*.<br /><br /> Se **-Rca** for usado, os hashes serão recomputados para todos os arquivos no assembly.|  
|`-Rh assembly`|Recomputa hashes para todos os arquivos no assembly.|  
|`-t[p] infile`|Exibe o token da chave pública armazenada em *infile*. O conteúdo de *infile* deve ser uma chave pública gerada anteriormente com base em um arquivo de par de chaves usando **-p**.  Não use a opção **-t[p]** para extrair o token diretamente de um arquivo de par de chaves.<br /><br /> Sn.exe computa o token usando uma função de hash da chave pública. Para economizar espaço, o Common Language Runtime armazena tokens de chave pública no manifesto como parte de uma referência a outro assembly quando registra uma dependência para um assembly com um nome forte. A opção **-TP** exibe a chave pública além do token. Se o atributo <xref:System.Reflection.AssemblySignatureKeyAttribute> tiver sido aplicado ao assembly, o token será para a chave de identidade, e o nome do algoritmo de hash e a chave de identidade será exibida.<br /><br /> Essa opção não verifica a assinatura do assembly e não deve ser usada para tomar decisões de confiança.  Essa opção exibe apenas os dados brutos do token de chave pública.|  
|`-T[p] assembly`|Exibe o token de chave pública do *assembly*. O *assembly* deve ser o nome de um arquivo que contém um manifesto do assembly.<br /><br /> Sn.exe computa o token usando uma função de hash da chave pública. Para economizar espaço, o runtime armazena tokens de chave pública no manifesto como parte de uma referência a outro assembly quando registra uma dependência para um assembly com um nome forte. A opção **-Tp** exibe a chave pública além do token. Se o atributo <xref:System.Reflection.AssemblySignatureKeyAttribute> tiver sido aplicado ao assembly, o token será para a chave de identidade, e o nome do algoritmo de hash e a chave de identidade será exibida.<br /><br /> Essa opção não verifica a assinatura do assembly e não deve ser usada para tomar decisões de confiança.  Essa opção exibe apenas os dados brutos do token de chave pública.|  
|`-TS assembly infile`|Teste-assina o *assembly* assinado ou parcialmente assinado com o par de chaves em *INFILE*.|  
|`-TSc assembly container`|Teste-assina o *assembly* assinado ou parcialmente assinado com o par de chaves no *contêiner*de contêiner de chave.|
|`-v assembly`|Verifica o nome forte em *assembly*, em que *assembly* é o nome de um arquivo que contém um manifesto do assembly.|  
|`-vf assembly`|Verifica o nome forte no *assembly*. Diferente da opção **-v**, **-vf** força a verificação, mesmo que seja desabilitada usando a opção **-Vr**.|  
|`-Vk regfile.reg assembly [userlist] [infile]`|Cria as entradas de um arquivo de registro (.reg) que é possível usar para registrar o assembly especificado ignorar a verificação. As regras de nomenclatura do assembly que se aplicam à opção **-Vr** se aplicam também a **–Vk**. Para obter informações sobre as opções *userlist* e *infile*, consulte a opção **–Vr**.|  
|`-Vl`|Lista as configurações atuais da verificação de nome forte neste computador.|  
|`-Vr  assembly [userlist] [infile]`|Registra *assembly* para ignorar a verificação. Também é possível especificar uma lista separada por vírgulas de nomes de usuário a que a verificação para ignorar deve ser aplicada. Se você especificar *infile*, a verificação permanecerá habilitada, mas a chave pública em *infile* será usada em operações de verificação. É possível especificar *assembly* na forma *\*, strongname* para registrar todos os assemblies com o nome forte especificado. Para *strongname*, especifique a cadeia de caracteres de dígitos hexadecimais que representa o formulário indexado da chave pública. Consulte as opções **-t** e **-T** para exibir o token de chave pública. **Cuidado:** use essa opção somente durante o desenvolvimento. A adição de um conjunto à lista de verificação para ignorar cria uma vulnerabilidade de segurança. Um assembly mal-intencionado pode usar o nome do assembly totalmente especificado (nome de assembly, versão, cultura e token de chave pública) do assembly adicionado à lista de verificação para forjar sua identidade. Isso permitiria que o assembly mal-intencionado também ignorasse a verificação.|  
|||  
|`-Vu  assembly`|Cancela o registro do *assembly* para ignorar a verificação. As mesmas regras de nomenclatura do assembly que se aplicam a **-Vr** se aplicam a **-Vu**.|  
|`-Vx`|Remove todas as entradas para ignorar a verificação.|  
|`-?`|Exibe sintaxe de comando e opções para a ferramenta.|  
  
> [!NOTE]
> Todas as opções de Sn.exe diferenciam maiúsculas de minúsculas e devem ser digitadas exatamente conforme mostrado para serem reconhecidas pela ferramenta.  
  
## <a name="remarks"></a>Comentários  
 As opções **-R** e **–Rc** são úteis com assemblies assinados com atraso. Nesse cenário, apenas a chave pública foi definida no tempo de compilação e a assinatura será realizada posteriormente, quando a chave privada será conhecida.  
  
> [!NOTE]
> Para parâmetros (por exemplo, –**Vr**) gravados em recursos protegidos como, por exemplo, o Registro, execute SN.exe como um administrador.  
  
A ferramenta Nome Forte pressupõe que os pares de chaves pública/privada são gerados com o identificador de algoritmo `AT_SIGNATURE`. Os pares de chaves pública/privada gerados com o algoritmo `AT_KEYEXCHANGE` geram um erro.

## <a name="examples"></a>Exemplos  
 O comando a seguir cria um novo par de chaves aleatório e o armazena em `keyPair.snk`.  
  
```console  
sn -k keyPair.snk  
```  
  
 O comando a seguir armazena a chave em `keyPair.snk` no contêiner `MyContainer` no CSP de nome forte.  
  
```console  
sn -i keyPair.snk MyContainer  
```  
  
 O comando a seguir extrai a chave pública de `keyPair.snk` e a armazena em `publicKey.snk`.  
  
```console  
sn -p keyPair.snk publicKey.snk  
```  
  
 O comando a seguir exibe a chave pública e o token da chave pública contidos em `publicKey.snk`.  
  
```console  
sn -tp publicKey.snk  
```  
  
 O comando a seguir verifica o assembly `MyAsm.dll`.  
  
```console  
sn -v MyAsm.dll  
```  
  
 O comando a seguir exclui `MyContainer` do CSP padrão.  
  
```console  
sn -d MyContainer  
```  
  
## <a name="see-also"></a>Consulte também

- [Ferramentas](index.md)
- [Al.exe (vinculador de assembly)](al-exe-assembly-linker.md)
- [Assemblies de nome forte](../../standard/assembly/strong-named.md)
- [Prompts de comando](developer-command-prompt-for-vs.md)
