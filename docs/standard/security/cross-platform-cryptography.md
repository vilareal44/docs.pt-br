---
title: Criptografia de plataforma cruzada no .NET Core e no .NET 5
description: Saiba mais sobre os recursos de criptografia em plataformas com suporte no .NET.
ms.date: 06/19/2020
ms.technology: dotnet-standard
helpviewer_keywords:
- cryptography, cross-platform
- encryption, cross-platform
ms.openlocfilehash: 61fd49e53761deac278b770003eb97241b6c2be9
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87557145"
---
# <a name="cross-platform-cryptography-in-net-core-and-net-5"></a>Criptografia de plataforma cruzada no .NET Core e no .NET 5

As operações criptográficas no .NET Core e no .NET 5 são feitas por bibliotecas de sistema operacional (SO). Essa dependência tem vantagens:

* Aplicativos .NET beneficiam-se da confiabilidade do sistema operacional. Manter as bibliotecas de criptografia protegidas contra vulnerabilidades é uma prioridade alta para OS fornecedores de sistema operacional. Para fazer isso, eles fornecem atualizações que os administradores do sistema devem aplicar.
* Os aplicativos .NET têm acesso a algoritmos validados por FIPS se as bibliotecas do sistema operacional forem validadas por FIPS.

A dependência das bibliotecas do sistema operacional também significa que os aplicativos .NET só podem usar recursos criptográficos compatíveis com o so. Embora todas as plataformas ofereçam suporte a certos recursos principais, alguns recursos que o .NET suporta não podem ser usados em algumas plataformas. Este artigo identifica os recursos que têm suporte em cada plataforma.

Este artigo pressupõe que você tenha uma familiaridade de trabalho com criptografia no .NET. Para obter mais informações, consulte [modelo de criptografia .net](cryptography-model.md) e [serviços de criptografia .net](cryptographic-services.md).

## <a name="hash-algorithms"></a>Algoritmos de hash

Todas as classes de algoritmo de hash e de autenticação de mensagem baseada em hash (HMAC), incluindo as `*Managed` classes, adiam para as bibliotecas do sistema operacional. Enquanto as várias bibliotecas de sistema operacional diferem no desempenho, elas devem ser compatíveis.

## <a name="symmetric-encryption"></a>Criptografia simétrica

As codificações subjacentes e o encadeamento são feitos pelas bibliotecas do sistema e todos têm suporte em todas as plataformas.

| Modo Cipher + | Windows | Linux | macOS |
|---------------|---------|-------|-------|
| AES-CBC       | ✔️     | ✔️    | ✔️   |
| AES-ECB       | ✔️     | ✔️    | ✔️   |
| 3DES-CBC      | ✔️     | ✔️    | ✔️   |
| 3DES-ECB      | ✔️     | ✔️    | ✔️   |
| DES-CBC       | ✔️     | ✔️    | ✔️   |
| DES-ECB       | ✔️     | ✔️    | ✔️   |

## <a name="authenticated-encryption"></a>Criptografia autenticada

O suporte à criptografia autenticada (AE) é fornecido para AES-CCM e AES-GCM por meio das <xref:System.Security.Cryptography.AesCcm?displayProperty=fullName> <xref:System.Security.Cryptography.AesGcm?displayProperty=fullName> classes e.

No Windows e no Linux, as implementações do AES-CCM e do AES-GCM são fornecidas pelas bibliotecas do sistema operacional.

### <a name="aes-ccm-and-aes-gcm-on-macos"></a>AES-CCM e AES-GCM no macOS

No macOS, as bibliotecas do sistema não dão suporte a AES-CCM ou AES-GCM para código de terceiros, portanto, as <xref:System.Security.Cryptography.AesCcm> <xref:System.Security.Cryptography.AesGcm> classes e usam OpenSSL para suporte. Os usuários no macOS precisam obter uma cópia apropriada do OpenSSL (libcrypto) para que esses tipos funcionem, e ele deve estar em um caminho para o qual o sistema carregaria uma biblioteca por padrão. Recomendamos que você instale o OpenSSL por meio de um Gerenciador de pacotes, como homebrew.

As `libcrypto.0.9.7.dylib` `libcrypto.0.9.8.dylib` bibliotecas e incluídas no MacOS são de versões anteriores do OpenSSL e não serão usadas. As `libcrypto.35.dylib` `libcrypto.41.dylib` bibliotecas,, e `libcrypto.42.dylib` são de LibreSSL e não serão usadas.

### <a name="aes-ccm-keys-nonces-and-tags"></a>Chaves AES-CCM, nonces e marcas

* Tamanhos de chave

  O AES-CCM funciona com chaves de 128, 192 e 256 bits.

* Tamanhos de nonce

  A <xref:System.Security.Cryptography.AesCcm> classe dá suporte a 56, 64, 72, 80, 88, 96 e 104 bits (7, 8, 9, 10, 11, 12 e 13 bytes) nonces.

* Tamanhos de marca

  A <xref:System.Security.Cryptography.AesCcm> classe dá suporte à criação ou processamento de marcas 32, 48, 64, 80, 96, 112 e 128 bits (4, 8, 10, 12, 14 e 16 bytes).

### <a name="aes-gcm-keys-nonces-and-tags"></a>Chaves AES-GCM, nonces e marcas

* Tamanhos de chave

  O AES-GCM funciona com as chaves 128, 192 e 256 bits.

* Tamanhos de nonce

  A <xref:System.Security.Cryptography.AesGcm> classe dá suporte apenas a nonces de 96 bits (12 bytes).

* Tamanhos de marca

  A <xref:System.Security.Cryptography.AesGcm> classe dá suporte à criação ou processamento de marcas 96, 104, 112, 120 e 128 bits (12, 13, 14, 15 e 16 bytes).

## <a name="asymmetric-cryptography"></a>Criptografia assimétrica

Esta seção inclui as seguintes subseções:

* [RSA](#rsa)
* [ECDSA](#ecdsa)
* [ECDH](#ecdh)
* [DSA](#dsa)

### <a name="rsa"></a>RSA

A geração de chave RSA (Rivest – Shamir – Adleman) é executada pelas bibliotecas do sistema operacional e está sujeita às suas limitações de tamanho e características de desempenho.

As operações de chave RSA são executadas pelas bibliotecas do sistema operacional, e os tipos de chave que podem ser carregados estão sujeitos aos requisitos do sistema operacional.

O .NET não expõe as operações RSA "RAW" (não preenchidas).

As bibliotecas do sistema operacional são usadas para criptografia e preenchimento de descriptografia. Nem todas as plataformas dão suporte às mesmas opções de preenchimento:

| Modo de preenchimento                          | Windows (CNG) | Linux (OpenSSL) | macOS | Windows (CAPI) |
|---------------------------------------|---------------|-----------------|-------|----------------|
| Criptografia PKCS1                      | ✔️           | ✔️              | ✔️   | ✔️             |
| OAEP-SHA-1                          | ✔️           | ✔️              | ✔️   | ✔️             |
| OAEP-SHA-2 (SHA256, SHA384, SHA512) | ✔️           | ✔️              | ✔️   | ❌             |
| Assinatura PKCS1 (MD5, SHA-1)          | ✔️           | ✔️              | ✔️   | ✔️             |
| Assinatura PKCS1 (SHA-2)               | ✔️           | ✔️              | ✔️   | ⚠️\*           |
| PSS                                   | ✔️           | ✔️              | ✔️   | ❌             |

\*O Windows CryptoAPI (CAPI) é capaz de PKCS1 assinatura com um algoritmo SHA-2. Mas o objeto RSA individual pode ser carregado em um CSP (provedor de serviços de criptografia) que não dá suporte a ele.

#### <a name="rsa-on-windows"></a>RSA no Windows

* O Windows CryptoAPI (CAPI) é usado sempre que [`new RSACryptoServiceProvider()`](xref:System.Security.Cryptography.RSACryptoServiceProvider) é usado.
* A CNG (API de criptografia do Windows próxima geração) é usada sempre que [`new RSACng()`](xref:System.Security.Cryptography.RSACng) é usada.
* O objeto retornado pelo <xref:System.Security.Cryptography.RSA.Create%2A?displayProperty=nameWithType> é fornecido internamente pela CNG do Windows. Esse uso da CNG do Windows é um detalhe de implementação e está sujeito a alterações.
* O <xref:System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPublicKey%2A> método de extensão para <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> retorna uma <xref:System.Security.Cryptography.RSACng> instância. Esse uso do <xref:System.Security.Cryptography.RSACng> é um detalhe de implementação e está sujeito a alterações.
* O <xref:System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPrivateKey%2A> método de extensão <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> , no momento, prefere uma <xref:System.Security.Cryptography.RSACng> instância, mas se <xref:System.Security.Cryptography.RSACng> o não puder abrir a chave, <xref:System.Security.Cryptography.RSACryptoServiceProvider> será tentada. O provedor preferencial é um detalhe de implementação e está sujeito a alterações.

#### <a name="rsa-native-interop"></a>Interoperabilidade nativa RSA

O .NET expõe tipos para permitir que os programas interoperem com as bibliotecas do sistema operacional que o código de criptografia .NET usa. Os tipos envolvidos não são convertidos entre plataformas e só devem ser usados diretamente quando necessário.

| Type                                                         | Windows | Linux         | macOS         |
|--------------------------------------------------------------|---------|---------------|---------------|
| <xref:System.Security.Cryptography.RSACryptoServiceProvider> | ✔️     | ⚠️<sup>1</sup>| ⚠️<sup>1</sup> |
| <xref:System.Security.Cryptography.RSACng>                   | ✔️     | ❌            | ❌            |
| <xref:System.Security.Cryptography.RSAOpenSsl>               | ❌     | ✔️            | ⚠️<sup>2</sup> |

<sup>1</sup> no MacOS e no Linux, <xref:System.Security.Cryptography.RSACryptoServiceProvider> pode ser usado para compatibilidade com os programas existentes. Nesse caso, qualquer método que exija a interoperabilidade do sistema operacional, como abrir uma chave nomeada, gera um <xref:System.PlatformNotSupportedException> .

<sup>2</sup> no MacOS, <xref:System.Security.Cryptography.RSAOpenSsl> funcionará se o OpenSSL estiver instalado e um dylib libcrypto apropriado puder ser encontrado por meio do carregamento da biblioteca dinâmica. Se uma biblioteca apropriada não puder ser encontrada, as exceções serão geradas.

### <a name="ecdsa"></a>ECDSA

A geração de chaves de ECDSA (algoritmo de assinatura digital de curva elíptica) é feita pelas bibliotecas do sistema operacional e está sujeita às suas limitações de tamanho e características de desempenho.

As curvas de chave de ECDSA são definidas pelas bibliotecas do sistema operacional e estão sujeitas às suas limitações.

| Curva elíptica                     | Windows 10    | Windows 7-8,1 | Linux         | macOS         |
|------------------------------------|---------------|-----------------|---------------|---------------|
| NIST P-256 (secp256r1)             | ✔️           | ✔️              | ✔️           | ✔️            |
| NIST P-384 (secp384r1)             | ✔️           | ✔️              | ✔️           | ✔️            |
| NIST P-521 (secp521r1)             | ✔️           | ✔️              | ✔️           | ✔️            |
| Curvas brainpool (como curvas nomeadas) | ✔️           | ❌              | ⚠️<sup>1</sup>| ❌           |
| Outras curvas nomeadas                 | ⚠️<sup>2</sup>| ❌             | ⚠️<sup>1</sup>| ❌           |
| Curvas explícitas                    | ✔️           | ❌              | ✔️           | ❌            |
| Exportar ou importar como explícito       | ✔️           | ❌<sup>Beta</sup>  | ✔️           | ❌<sup>Beta</sup>|

<sup>1</sup> as distribuições do Linux nem todas têm suporte para as mesmas curvas nomeadas.

<sup>2</sup> o suporte para curvas nomeadas foi adicionado à CNG do Windows no Windows 10. Para obter mais informações, consulte [CNG chamado curvas elípticas](https://msdn.microsoft.com/library/windows/desktop/mt632245(v=vs.85).aspx). As curvas nomeadas não estão disponíveis em versões anteriores do Windows, exceto para três curvas no Windows 7.

<sup>3</sup> a exportação com parâmetros de curva explícitos requer suporte à biblioteca do so, que não está disponível no MacOS ou em versões anteriores do Windows.

#### <a name="ecdsa-native-interop"></a>Interoperabilidade nativa de ECDSA

O .NET expõe tipos para permitir que os programas interoperem com as bibliotecas do sistema operacional que o código de criptografia .NET usa. Os tipos envolvidos não são convertidos entre plataformas e só devem ser usados diretamente quando necessário.

| Type                                             | Windows | Linux | macOS |
|--------------------------------------------------|---------|-------|-------|
| <xref:System.Security.Cryptography.ECDsaCng>     | ✔️     | ❌    | ❌    |
| <xref:System.Security.Cryptography.ECDsaOpenSsl> | ❌     | ✔️    | ⚠️\*  |

\*No macOS, <xref:System.Security.Cryptography.ECDsaOpenSsl> o funcionará se o OpenSSL estiver instalado no sistema e um libcrypto dylib apropriado puder ser encontrado por meio do carregamento da biblioteca dinâmica. Se uma biblioteca apropriada não puder ser encontrada, as exceções serão geradas.

### <a name="ecdh"></a>ECDH

A geração de chave ECDH (Diffie-Hellman de curva elíptica) é feita pelas bibliotecas do sistema operacional e está sujeita às suas limitações de tamanho e características de desempenho.

A <xref:System.Security.Cryptography.ECDiffieHellman> classe não retorna o valor "RAW" da computação ECDH. Todos os dados retornados estão em termos de funções de derivação de chave:

* HASH (Z)
* HASH (preceder | | Z | | anexar
* HMAC (chave, Z)
* HMAC (chave, preceder | | Z | | anexar
* HMAC (Z, Z)
* HMAC (Z, preceder | | Z | | anexar
* Tls11Prf (rótulo, semente)

As curvas de chave ECDH são definidas pelas bibliotecas do sistema operacional e estão sujeitas às suas limitações.

| Curva elíptica                     | Windows 10    | Windows 7-8,1 | Linux         | macOS         |
|------------------------------------|---------------|-----------------|---------------|---------------|
| NIST P-256 (secp256r1)             | ✔️           | ✔️              | ✔️           | ✔️            |
| NIST P-384 (secp384r1)             | ✔️           | ✔️              | ✔️           | ✔️            |
| NIST P-521 (secp521r1)             | ✔️           | ✔️              | ✔️           | ✔️            |
| curvas brainpool (como curvas nomeadas) | ✔️           | ❌              | ⚠️<sup>1</sup>| ❌           |
| outras curvas nomeadas                 | ⚠️<sup>2</sup>| ❌             | ⚠️<sup>1</sup>| ❌           |
| curvas explícitas                    | ✔️           | ❌              | ✔️           | ❌            |
| Exportar ou importar como explícito       | ✔️           | ❌<sup>Beta</sup>  | ✔️           | ❌<sup>Beta</sup>|

<sup>1</sup> as distribuições do Linux nem todas têm suporte para as mesmas curvas nomeadas.

<sup>2</sup> o suporte para curvas nomeadas foi adicionado à CNG do Windows no Windows 10. Para obter mais informações, consulte [CNG chamado curvas elípticas](https://msdn.microsoft.com/library/windows/desktop/mt632245(v=vs.85).aspx). As curvas nomeadas não estão disponíveis em versões anteriores do Windows, exceto para três curvas no Windows 7.

<sup>3</sup> a exportação com parâmetros de curva explícitos requer suporte à biblioteca do so, que não está disponível no MacOS ou em versões anteriores do Windows.

#### <a name="ecdh-native-interop"></a>Interoperabilidade nativa do ECDH

O .NET expõe tipos para permitir que os programas interoperem com as bibliotecas do sistema operacional usadas pelo .NET. Os tipos envolvidos não são convertidos entre plataformas e só devem ser usados diretamente quando necessário.

| Type                                                       | Windows | Linux | macOS |
|------------------------------------------------------------|---------|-------|-------|
| <xref:System.Security.Cryptography.ECDiffieHellmanCng>     | ✔️     | ❌    | ❌   |
| <xref:System.Security.Cryptography.ECDiffieHellmanOpenSsl> | ❌     | ✔️    | ⚠️\* |

\*No macOS, <xref:System.Security.Cryptography.ECDiffieHellmanOpenSsl> o funcionará se o OpenSSL estiver instalado e um dylib libcrypto apropriado puder ser encontrado por meio do carregamento da biblioteca dinâmica. Se uma biblioteca apropriada não puder ser encontrada, as exceções serão geradas.

### <a name="dsa"></a>DSA

A geração de chave de DSA (algoritmo de assinatura digital) é executada pelas bibliotecas do sistema e está sujeita às suas limitações de tamanho e características de desempenho.

| Função                      | CNG do Windows | Linux | macOS         | Windows CAPI |
|-------------------------------|-------------|-------|---------------|--------------|
| Criação de chave (<= 1024 bits)   | ✔️         | ✔️    | ❌            | ✔️           |
| Criação de chave (> bits 1024)    | ✔️         | ✔️    | ❌            | ❌            |
| Carregando chaves (<= 1024 bits)   | ✔️         | ✔️    | ✔️            | ✔️           |
| Carregando chaves (> bits 1024)    | ✔️         | ✔️    | ⚠️\*          | ❌            |
| FIPS 186-2                    | ✔️         | ✔️    | ✔️            | ✔️           |
| FIPS 186-3 (assinaturas SHA-2) | ✔️         | ✔️    | ❌            | ❌            |

\*o macOS carrega chaves DSA maiores que 1024 bits, mas o comportamento dessas chaves é indefinido. Eles não se comportam de acordo com o FIPS 186-3.

#### <a name="dsa-on-windows"></a>DSA no Windows

* O Windows CryptoAPI (CAPI) é usado sempre que [`new DSACryptoServiceProvider()`](xref:System.Security.Cryptography.DSACryptoServiceProvider) é usado.
* A CNG (API de criptografia do Windows próxima geração) é usada sempre que [`new DSACng()`](xref:System.Security.Cryptography.DSACng) é usada.
* O objeto retornado pelo <xref:System.Security.Cryptography.DSA.Create%2A?displayProperty=nameWithType> é fornecido internamente pela CNG do Windows. Esse uso da CNG do Windows é um detalhe de implementação e está sujeito a alterações.
* O <xref:System.Security.Cryptography.X509Certificates.DSACertificateExtensions.GetDSAPublicKey%2A> método de extensão para <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> retorna uma <xref:System.Security.Cryptography.DSACng> instância. Esse uso do <xref:System.Security.Cryptography.DSACng> é um detalhe de implementação e está sujeito a alterações.
* O <xref:System.Security.Cryptography.X509Certificates.DSACertificateExtensions.GetDSAPrivateKey%2A> método de extensão para <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> prefere uma <xref:System.Security.Cryptography.DSACng> instância, mas se <xref:System.Security.Cryptography.DSACng> o não puder abrir a chave, <xref:System.Security.Cryptography.DSACryptoServiceProvider> será tentada.  O provedor preferencial é um detalhe de implementação e está sujeito a alterações.

#### <a name="dsa-native-interop"></a>Interoperabilidade nativa de DSA

O .NET expõe tipos para permitir que os programas interoperem com as bibliotecas do sistema operacional que o código de criptografia .NET usa. Os tipos envolvidos não são convertidos entre plataformas e só devem ser usados diretamente quando necessário.

| Type                                                         | Windows | Linux         | macOS         |
|--------------------------------------------------------------|---------|---------------|---------------|
| <xref:System.Security.Cryptography.DSACryptoServiceProvider> | ✔️     | ⚠️<sup>1</sup> | ⚠️<sup>1</sup> |
| <xref:System.Security.Cryptography.DSACng>                   | ✔️     | ❌             | ❌            |
| <xref:System.Security.Cryptography.DSAOpenSsl>               | ❌      | ✔️            | ⚠️<sup>2</sup> |

<sup>1</sup> no MacOS e no Linux, <xref:System.Security.Cryptography.DSACryptoServiceProvider> pode ser usado para compatibilidade com os programas existentes. Nesse caso, qualquer método que exija a interoperabilidade do sistema, como abrir uma chave nomeada, gera um <xref:System.PlatformNotSupportedException> .

<sup>2</sup> no MacOS, <xref:System.Security.Cryptography.DSAOpenSsl> funcionará se o OpenSSL estiver instalado e um dylib libcrypto apropriado puder ser encontrado por meio do carregamento da biblioteca dinâmica. Se uma biblioteca apropriada não puder ser encontrada, as exceções serão geradas.

## <a name="x509-certificates"></a>Certificados X. 509

A maior parte do suporte para certificados X. 509 no .NET é proveniente de bibliotecas do sistema operacional. Para carregar um certificado em uma <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> <xref:System.Security.Cryptography.X509Certificates.X509Certificate> instância do ou no .net, o certificado deve ser carregado pela biblioteca do sistema operacional subjacente.

### <a name="read-a-pkcs12pfx"></a>Ler um PKCS12/PFX

| Cenário                                     | Windows | Linux | macOS |
|----------------------------------------------|---------|-------|-------|
| Vazio                                        | ✔️     | ✔️    | ✔️   |
| Um certificado, nenhuma chave privada              | ✔️     | ✔️    | ✔️   |
| Um certificado, com chave privada            | ✔️     | ✔️    | ✔️   |
| Vários certificados, sem chaves privadas       | ✔️     | ✔️    | ✔️   |
| Vários certificados, uma chave privada       | ✔️     | ✔️    | ✔️   |
| Vários certificados, várias chaves privadas | ✔️     | ⚠️\*  | ✔️   |

\*Disponível em versões de visualização do .NET 5.

### <a name="write-a-pkcs12pfx"></a>Gravar um PKCS12/PFX

| Cenário                                     | Windows | Linux | macOS |
|----------------------------------------------|---------|-------|-------|
| Vazio                                        | ✔️     | ✔️    | ⚠️\* |
| Um certificado, nenhuma chave privada              | ✔️     | ✔️    | ⚠️\* |
| Um certificado, com chave privada            | ✔️     | ✔️    | ✔️   |
| Vários certificados, sem chaves privadas       | ✔️     | ✔️    | ⚠️\* |
| Vários certificados, uma chave privada       | ✔️     | ✔️    | ✔️   |
| Vários certificados, várias chaves privadas | ✔️     | ⚠️\*  | ✔️   |
| Carregamento efêmero                            | ✔️     | ✔️    | ⚠️\* |

\*Disponível em versões de visualização do .NET 5.

o macOS não pode carregar chaves privadas de certificado sem um objeto de conjunto de chaves, que requer gravação no disco. Os conjuntos de chaves são criados automaticamente para carregamento de PFX e são excluídos quando não estiverem mais em uso. Como a <xref:System.Security.Cryptography.X509Certificates.X509KeyStorageFlags.EphemeralKeySet?displayProperty=nameWithType> opção significa que a chave privada não deve ser gravada no disco, a declaração desse sinalizador no MacOS resulta em um <xref:System.PlatformNotSupportedException> .

### <a name="write-a-pkcs7-certificate-collection"></a>Gravar uma coleção de certificados PKCS7

O Windows e o Linux emitem BLOBs PKCS7 codificados em DER. o macOS emite BLOBs PKCS7 codificados por tamanho indefinido CER.

### <a name="x509store"></a>X509Store

No Windows, a <xref:System.Security.Cryptography.X509Certificates.X509Store> classe é uma representação das APIs do repositório de certificados do Windows. Essas APIs funcionam da mesma forma no .NET Core e no .NET 5, como nas .NET Framework.

No Linux, a <xref:System.Security.Cryptography.X509Certificates.X509Store> classe é uma projeção de decisões de confiança do sistema (somente leitura), decisões de confiança do usuário (leitura-gravação) e armazenamento de chave do usuário (leitura-gravação).

No macOS, a <xref:System.Security.Cryptography.X509Certificates.X509Store> classe é uma projeção de decisões de confiança do sistema (somente leitura), decisões de confiança do usuário (somente leitura) e armazenamento de chave do usuário (leitura/gravação).

As tabelas a seguir mostram quais cenários têm suporte em cada plataforma. Para cenários sem suporte ( ❌ nas tabelas), um <xref:System.Security.Cryptography.CryptographicException> é lançado.

#### <a name="the-my-store"></a>O meu repositório

| Cenário                                         | Windows | Linux | macOS |
|--------------------------------------------------|---------|-------|-------|
| Abrir CurrentUser\My (ReadOnly)                   | ✔️     | ✔️    | ✔️   |
| Abrir CurrentUser\My (ReadWrite)                  | ✔️     | ✔️    | ✔️   |
| Abrir CurrentUser\My (ExistingOnly)               | ✔️     | ⚠️    | ✔️   |
| Abrir LocalMachine\My                             | ✔️     | ❌    | ✔️   |

No Linux, as lojas são criadas na primeira gravação e não há armazenamentos de usuário por padrão, portanto, abrir `CurrentUser\My` com `ExistingOnly` pode falhar.

No macOS, o `CurrentUser\My` repositório é o conjunto de chaves padrão do usuário, que é `login.keychain` por padrão. O `LocalMachine\My` repositório é `System.keychain` .

#### <a name="the-root-store"></a>O armazenamento raiz

| Cenário                              | Windows | Linux | macOS           |
|---------------------------------------|---------|-------|-----------------|
| Abrir CurrentUser\Root (ReadOnly)      | ✔️     | ✔️    | ✔️             |
| Abrir CurrentUser\Root (ReadWrite)     | ✔️     | ✔️    | ❌              |
| Abrir CurrentUser\Root (ExistingOnly)  | ✔️     | ⚠️    | ✔️ (se ReadOnly) |
| Abrir LocalMachine\Root (ReadOnly)     | ✔️     | ✔️    | ✔️             |
| Abrir LocalMachine\Root (ReadWrite)    | ✔️     | ❌    | ❌              |
| Abrir LocalMachine\Root (ExistingOnly) | ✔️     | ⚠️    | ✔️ (se ReadOnly) |

No Linux, a `LocalMachine\Root` loja é uma interpretação do pacote de CA no caminho padrão para o OpenSSL.

No macOS, o `CurrentUser\Root` repositório é uma interpretação dos `SecTrustSettings` resultados para o domínio de confiança do usuário. O `LocalMachine\Root` repositório é uma interpretação dos `SecTrustSettings` resultados para os domínios de confiança do administrador e do sistema.

#### <a name="the-intermediate-store"></a>O armazenamento intermediário

| Cenário                                      | Windows | Linux | macOS           |
|-----------------------------------------------|---------|-------|-----------------|
| Abrir CurrentUser\Intermediate (ReadOnly)      | ✔️     | ✔️    | ✔️             |
| Abrir CurrentUser\Intermediate (ReadWrite)     | ✔️     | ✔️    | ❌              |
| Abrir CurrentUser\Intermediate (ExistingOnly)  | ✔️     | ⚠️    | ✔️ (se ReadOnly) |
| Abrir LocalMachine\Intermediate (ReadOnly)     | ✔️     | ✔️    | ✔️             |
| Abrir LocalMachine\Intermediate (ReadWrite)    | ✔️     | ❌    | ❌              |
| Abrir LocalMachine\Intermediate (ExistingOnly) | ✔️     | ⚠️    | ✔️ (se ReadOnly) |

No Linux, o `CurrentUser\Intermediate` repositório é usado como um cache ao baixar CAs intermediárias por seus registros de acesso às informações de autoridade em builds X509Chain bem-sucedidos. O `LocalMachine\Intermediate` repositório é uma interpretação do pacote de CA no caminho padrão para o OpenSSL.

#### <a name="the-disallowed-store"></a>O repositório não permitido

| Cenário                                    | Windows | Linux | macOS           |
|---------------------------------------------|---------|-------|-----------------|
| Abrir CurrentUser\Disallowed (ReadOnly)      | ✔️     | ⚠️    | ✔️             |
| Abrir CurrentUser\Disallowed (ReadWrite)     | ✔️     | ⚠️    | ❌              |
| Abrir CurrentUser\Disallowed (ExistingOnly)  | ✔️     | ⚠️    | ✔️ (se ReadOnly) |
| Abrir LocalMachine\Disallowed (ReadOnly)     | ✔️     | ❌    | ✔️             |
| Abrir LocalMachine\Disallowed (ReadWrite)    | ✔️     | ❌    | ❌              |
| Abrir LocalMachine\Disallowed (ExistingOnly) | ✔️     | ❌    | ✔️ (se ReadOnly) |

No Linux, a `Disallowed` loja não é usada na criação de cadeia e a tentativa de adicionar conteúdo a ela resulta em um <xref:System.Security.Cryptography.CryptographicException> . Um <xref:System.Security.Cryptography.CryptographicException> é gerado ao abrir a `Disallowed` loja se ele já tiver adquirido conteúdo.

No macOS, os armazenamentos CurrentUser\Disallowed e LocalMachine\Disallowed são as interpretações dos resultados SecTrustSettings apropriados para certificados cuja confiança está definida como `Always Deny` .

#### <a name="nonexistent-store"></a>Armazenamento inexistente

| Cenário                                         | Windows | Linux | macOS |
|--------------------------------------------------|---------|-------|-------|
| Abrir armazenamento não existente (ExistingOnly)           | ❌     | ❌     | ❌    |
| Abrir o armazenamento não existente de CurrentUser (ReadWrite)  | ✔️     | ✔️     | ⚠️   |
| Abrir armazenamento não existente de LocalMachine (ReadWrite) | ✔️     | ❌     | ❌    |

No macOS, a criação de repositório personalizado com a API X509Store tem suporte apenas para o `CurrentUser` local. Ele criará um novo conjunto de chaves sem senha no diretório do conjunto de chaves do usuário (*~/library/keychains*). Para criar um conjunto de chaves com senha, um P/Invoke `SecKeychainCreate` pode ser usado. Da mesma forma, o `SecKeychainOpen` pode ser usado para abrir conjuntos de chaves em locais diferentes. O resultado `IntPtr` pode ser passado para [`new X509Store(IntPtr)`](xref:System.Security.Cryptography.X509Certificates.X509Store.%23ctor(System.IntPtr)) para obter um armazenamento com capacidade de leitura/gravação, sujeito às permissões do usuário atual.

### <a name="x509chain"></a>X509Chain

o macOS não dá suporte à utilização de CRL offline e, portanto, `X509RevocationMode.Offline` é tratado como `X509RevocationMode.Online` .

o macOS não dá suporte a um tempo limite iniciado pelo usuário na CRL (lista de certificados revogados)/o download do OCSP (protocolo de status de certificado online)/AIA (acesso a informações da autoridade), portanto, `X509ChainPolicy.UrlRetrievalTimeout` é ignorado.

## <a name="additional-resources"></a>Recursos adicionais

* [Modelo de criptografia .NET](cryptography-model.md)
* [Serviços de criptografia .NET](cryptographic-services.md)
* [Vulnerabilidades de temporização com descriptografia simétrica no modo CBC usando preenchimento](vulnerabilities-cbc-mode.md)
* [Proteção de dados do ASP.NET Core](/aspnet/core/security/data-protection/introduction)
