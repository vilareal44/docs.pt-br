---
ms.openlocfilehash: 5e77b7bd73c09e061a94a29703cf5286814d1ebb
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84602911"
---

[O .NET Core está disponível no repositório de snap.](https://snapcraft.io/dotnet-sdk)

Um snap é um pacote de um aplicativo e suas dependências que funcionam sem modificação em várias distribuições do Linux diferentes. Os snaps são detectáveis e instaláveis no repositório de instantâneos. Para obter mais informações sobre o snap, consulte [introdução ao snap](https://snapcraft.io/docs/getting-started).

Somente as versões com suporte do .NET Core estão disponíveis por meio do snap.

### <a name="install-the-sdk"></a>Instalar o SDK

Pacotes snap para SDK do .NET Core são todos publicados sob o mesmo identificador: `dotnet-sdk` . Uma versão específica do SDK pode ser instalada especificando o canal. O SDK inclui o tempo de execução de coresponder. A tabela a seguir lista os canais:

| Versão do .NET Core | Ajustar pacote             |
|-------------------|--------------------------|
| 3,1 (LTS)         | `3.1` ou `latest/stable` |
| 2,1 (LTS)         | `2.1`                    |
| Versão prévia do .NET 5,0  | `5.0/beta`               |

Use o `snap install` comando para instalar um pacote de snap SDK do .NET Core. Use o `--channel` parâmetro para indicar qual versão deve ser instalada. Se esse parâmetro for omitido, `latest/stable` será usado. Neste exemplo, `3.1` é especificado:

```bash
sudo snap install dotnet-sdk --classic --channel=3.1
```

Em seguida, registre o `dotnet` comando para o sistema com o `snap alias` comando:

```bash
sudo snap alias dotnet-sdk.dotnet dotnet
```

Este comando é formatado como: `sudo snap alias {package}.{command} {alias}` . Você pode escolher qualquer `{alias}` nome que desejar. Por exemplo, você pode nomear o comando após a versão específica instalada pelo snap: `sudo snap alias dotnet-sdk.dotnet dotnet31` . Ao usar o comando `dotnet31` , você invocará essa versão específica do .net. Mas isso é incompatível com a maioria dos tutoriais e exemplos, pois eles esperam que um `dotnet` comando esteja disponível.

### <a name="install-the-runtime"></a>Instalar o runtime

Os pacotes snap para o tempo de execução do .NET Core são publicados em seu próprio identificador de pacote. A tabela a seguir lista os identificadores de pacote:

| Versão do .NET Core | Ajustar pacote        |
|-------------------|---------------------|
| 3,1 (LTS)         | `dotnet-runtime-31` |
| 3.0               | `dotnet-runtime-30` |
| 2.2               | `dotnet-runtime-22` |
| 2,1 (LTS)         | `dotnet-runtime-21` |

Use o `snap install` comando para instalar um pacote de snap do tempo de execução do .NET Core. Neste exemplo, o .NET Core 3,1 está instalado:

```bash
sudo snap install dotnet-runtime-31 --classic
```

Em seguida, registre o `dotnet` comando para o sistema com o `snap alias` comando:

```bash
sudo snap alias dotnet-runtime-31.dotnet dotnet
```

Este comando é formatado como: `sudo snap alias {package}.{command} {alias}` . Você pode escolher qualquer `{alias}` nome que desejar. Por exemplo, você pode nomear o comando após a versão específica instalada pelo snap: `sudo snap alias dotnet-runtime-31.dotnet dotnet31` . Ao usar o comando `dotnet31` , você invocará essa versão específica do .net. Mas isso é incompatível com a maioria dos tutoriais e exemplos, pois eles esperam que um `dotnet` comando esteja disponível.

### <a name="ssl-certificate-errors"></a>Erros de certificado SSL

Quando o .NET é instalado por meio do snap, é possível que, em alguns distribuições, os certificados SSL do .NET possam não ser encontrados e você receba um erro semelhante ao seguinte durante `restore` :

```bash
Processing post-creation actions...
Running 'dotnet restore' on /home/myhome/test/test.csproj...
  Restoring packages for /home/myhome/test/test.csproj...
/snap/dotnet-sdk/27/sdk/2.2.103/NuGet.targets(114,5): error : Unable to load the service index for source https://api.nuget.org/v3/index.json. [/home/myhome/test/test.csproj]
/snap/dotnet-sdk/27/sdk/2.2.103/NuGet.targets(114,5): error :   The SSL connection could not be established, see inner exception. [/home/myhome/test/test.csproj]
/snap/dotnet-sdk/27/sdk/2.2.103/NuGet.targets(114,5): error :   The remote certificate is invalid according to the validation procedure. [/home/myhome/test/test.csproj]
```

Para resolver esse problema, defina algumas variáveis ambiente:

```bash
export SSL_CERT_FILE=[path-to-certificate-file]
export SSL_CERT_DIR=/dev/null
```

O local do certificado variará de acordo com distribuição. Aqui estão os locais para o distribuições onde tivemos o problema.

* Fedora`/etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem`
* OpenSUSE`/etc/ssl/ca-bundle.pem`
* Solus -`/etc/ssl/certs/ca-certificates.crt`
