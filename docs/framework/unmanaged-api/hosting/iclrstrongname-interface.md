---
title: Interface ICLRStrongName
ms.date: 03/30/2017
api_name:
- ICLRStrongName
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRStrongName
helpviewer_keywords:
- ICLRStrongName interface [.NET Framework hosting]
ms.assetid: 2fac66fd-6b3b-4dbd-8baf-86038bd85526
topic_type:
- apiref
ms.openlocfilehash: 0b6efcbe4458977e8e938afabd7ae59171bc065a
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501645"
---
# <a name="iclrstrongname-interface"></a>Interface ICLRStrongName
Fornece funções estáticas globais básicas para assinar assemblies com nomes fortes. Todos os `ICLRStrongName` métodos retornam o padrão com HResults.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método GetHashFromAssemblyFile](iclrstrongname-gethashfromassemblyfile-method.md)|Obtém um hash do arquivo do assembly especificado, usando o algoritmo de hash especificado.|  
|[Método GetHashFromAssemblyFileW](iclrstrongname-gethashfromassemblyfilew-method.md)|Obtém um hash do arquivo do assembly especificado como uma cadeia de caracteres Unicode, usando o algoritmo de hash especificado.|  
|[Método GetHashFromBlob](iclrstrongname-gethashfromblob-method.md)|Obtém um hash do assembly no endereço de memória especificado, usando o algoritmo de hash especificado.|  
|[Método GetHashFromFile](iclrstrongname-gethashfromfile-method.md)|Gera um hash sobre o conteúdo do arquivo especificado.|  
|[Método GetHashFromFileW](iclrstrongname-gethashfromfilew-method.md)|Gera um hash sobre o conteúdo do arquivo especificado por uma cadeia de caracteres Unicode.|  
|[Método GetHashFromHandle](iclrstrongname-gethashfromhandle-method.md)|Gera um hash sobre o conteúdo do arquivo com o identificador de arquivo especificado, usando o algoritmo de hash especificado.|  
|[Método StrongNameCompareAssemblies](iclrstrongname-strongnamecompareassemblies-method.md)|Determina se dois assemblies diferem somente por suas assinaturas de nome forte.|  
|[Método StrongNameFreeBuffer](iclrstrongname-strongnamefreebuffer-method.md)|Libera a memória que foi alocada com uma chamada anterior para um método de nome forte, como [StrongNameGetPublicKey](iclrstrongname-strongnamegetpublickey-method.md), [StrongNameTokenFromPublicKey](iclrstrongname-strongnametokenfrompublickey-method.md)ou [StrongNameSignatureGeneration](iclrstrongname-strongnamesignaturegeneration-method.md).|  
|[Método StrongNameGetBlob](iclrstrongname-strongnamegetblob-method.md)|Preenche o buffer especificado com a representação binária do arquivo executável no endereço especificado.|  
|[Método StrongNameGetBlobFromImage](iclrstrongname-strongnamegetblobfromimage-method.md)|Obtém uma representação binária da imagem do assembly no endereço de memória especificado.|  
|[Método StrongNameGetPublicKey](iclrstrongname-strongnamegetpublickey-method.md)|Obtém a chave pública de um par de chaves pública/privada.|  
|[Método StrongNameHashSize](iclrstrongname-strongnamehashsize-method.md)|Obtém o tamanho do buffer necessário para um hash, usando o algoritmo de hash especificado.|  
|[Método StrongNameKeyDelete](iclrstrongname-strongnamekeydelete-method.md)|Exclui o contêiner de chave especificado.|  
|[Método StrongNameKeyGen](iclrstrongname-strongnamekeygen-method.md)|Cria um novo par de chaves públicas/privadas para uso de nome forte.|  
|[Método StrongNameKeyGenEx](iclrstrongname-strongnamekeygenex-method.md)|Gera um novo par de chaves públicas/privadas com o tamanho da chave especificado para o uso de nome forte.|  
|[Método StrongNameKeyInstall](iclrstrongname-strongnamekeyinstall-method.md)|Importa um par de chaves públicas/privadas em um contêiner.|  
|[Método StrongNameSignatureGeneration](iclrstrongname-strongnamesignaturegeneration-method.md)|Gera uma assinatura de nome forte para o assembly especificado.|  
|[Método StrongNameSignatureGenerationEx](iclrstrongname-strongnamesignaturegenerationex-method.md)|Gera uma assinatura de nome forte para o assembly especificado, com base nos sinalizadores especificados.|  
|[Método StrongNameSignatureSize](iclrstrongname-strongnamesignaturesize-method.md)|Retorna o tamanho da assinatura de nome forte.|  
|[Método StrongNameSignatureVerification](iclrstrongname-strongnamesignatureverification-method.md)|Obtém um valor que indica se o manifesto do assembly no caminho fornecido contém uma assinatura de nome forte, que é verificada de acordo com os sinalizadores especificados.|  
|[Método StrongNameSignatureVerificationEx](iclrstrongname-strongnamesignatureverificationex-method.md)|Obtém um valor que indica se o manifesto do assembly no caminho fornecido contém uma assinatura de nome forte.|  
|[Método StrongNameSignatureVerificationFromImage](iclrstrongname-strongnamesignatureverificationfromimage-method.md)|Verifica se um assembly, que já foi mapeado para a memória, é válido para a chave pública associada.|  
|[Método StrongNameTokenFromAssembly](iclrstrongname-strongnametokenfromassembly-method.md)|Cria um token de nome forte do arquivo do assembly especificado.|  
|[Método StrongNameTokenFromAssemblyEx](iclrstrongname-strongnametokenfromassemblyex-method.md)|Cria um token de nome forte por meio do arquivo do assembly especificado e retorna a chave pública.|  
|[Método StrongNameTokenFromPublicKey](iclrstrongname-strongnametokenfrompublickey-method.md)|Obtém um token que representa uma chave pública.|  
  
## <a name="remarks"></a>Comentários  
 Você pode obter uma instância do `ICLRStrongName` chamando o método [ICLRRuntimeInfo:: GetInterface](iclrruntimeinfo-getinterface-method.md) usando `CLSID_CLRStrongName` e `IID_ICLRStrongName` como parâmetros.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MetaHost. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interfaces de hospedagem](hosting-interfaces.md)
- [Hosting](index.md)
