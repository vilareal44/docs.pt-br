---
title: 'Como: Criar uma política de editor'
description: Saiba como os fornecedores de assembly podem criar um arquivo de política de Publicador com um assembly atualizado no .NET, para estipular que os aplicativos devem usar a versão mais recente.
ms.date: 03/30/2017
helpviewer_keywords:
- publisher policy assembly
- publisher policy files
- GAC (global assembly cache), publisher policy assembly
- global assembly cache, publisher policy assembly
ms.assetid: 8046bc5d-2fa9-4277-8a5e-6dcc96c281d9
ms.openlocfilehash: 23e9d8144ec5742e0371d566b7af59dc9dd30c9b
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85105407"
---
# <a name="how-to-create-a-publisher-policy"></a>Como: Criar uma política de editor

Os fornecedores de assemblies podem declarar que os aplicativos devem usar uma versão mais recente de um assembly, incluindo um arquivo de política do Publicador com o assembly atualizado. O arquivo de política do Publicador especifica as configurações de redirecionamento de assembly e base de código e usa o mesmo formato que um arquivo de configuração de aplicativo. O arquivo de política do Publicador é compilado em um assembly e colocado no cache de assembly global.

Há três etapas envolvidas na criação de uma política de editor:

1. Crie um arquivo de política do Publicador.

2. Crie um assembly de política do Publicador.

3. Adicione o assembly de política do Publicador ao cache de assembly global.

O esquema para a política do Publicador é descrito em [redirecionar versões de assembly](redirect-assembly-versions.md). O exemplo a seguir mostra um arquivo de política do Publicador que redireciona uma versão do `myAssembly` para outra.

```xml
<configuration>
   <runtime>
      <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
       <dependentAssembly>
         <assemblyIdentity name="myAssembly"
                           publicKeyToken="32ab4ba45e0a69a1"
                           culture="en-us" />
         <!-- Redirecting to version 2.0.0.0 of the assembly. -->
         <bindingRedirect oldVersion="1.0.0.0"
                          newVersion="2.0.0.0"/>
       </dependentAssembly>
      </assemblyBinding>
   </runtime>
</configuration>
```

Para saber como especificar uma base de código, consulte [especificando o local de um assembly](specify-assembly-location.md).

## <a name="creating-the-publisher-policy-assembly"></a>Criando o assembly de política do Publicador

Use o [vinculador de assembly (Al.exe)](../tools/al-exe-assembly-linker.md) para criar o assembly de política do Publicador.

#### <a name="to-create-a-publisher-policy-assembly"></a>Para criar um assembly de política do Publicador

Digite o seguinte comando no prompt de comando:

```console
al /link:publisherPolicyFile /out:publisherPolicyAssemblyFile /keyfile:keyPairFile /platform:processorArchitecture
```

Neste comando:

- O `publisherPolicyFile` argumento é o nome do arquivo de política do Publicador.

- O `publisherPolicyAssemblyFile` argumento é o nome do assembly de política do Publicador que resulta desse comando. O nome do arquivo de assembly deve seguir o formato:

  'policy.majorNumber.minorNumber.mainAssemblyName.dll '

- O `keyPairFile` argumento é o nome do arquivo que contém o par de chaves. Você deve assinar o assembly e o assembly da política do Publicador com o mesmo par de chaves.

- O `processorArchitecture` argumento identifica a plataforma de destino de um assembly específico do processador.

  > [!NOTE]
  > A capacidade de direcionar uma arquitetura de processador específica está disponível a partir do .NET Framework 2,0.

A capacidade de direcionar uma arquitetura de processador específica está disponível a partir do .NET Framework 2,0. O comando a seguir cria um assembly de política do publicador chamado `policy.1.0.myAssembly` de um arquivo de política do publicador chamado `pub.config` , atribui um nome forte ao assembly usando o par de chaves no `sgKey.snk` arquivo e especifica que o assembly tem como alvo a arquitetura do processador x86.

```console
al /link:pub.config /out:policy.1.0.myAssembly.dll /keyfile:sgKey.snk /platform:x86
```

O assembly de política do Publicador deve corresponder à arquitetura de processador do assembly ao qual ele se aplica. Portanto, se o assembly tiver um <xref:System.Reflection.AssemblyName.ProcessorArchitecture%2A> valor de <xref:System.Reflection.ProcessorArchitecture.MSIL> , o assembly de política de editor para esse assembly deverá ser criado com `/platform:anycpu` . Você deve fornecer um assembly de política de Publicador separado para cada assembly específico de processador.

Uma consequência dessa regra é que, para alterar a arquitetura do processador de um assembly, você deve alterar o componente principal ou secundário do número de versão, para que você possa fornecer um novo assembly de política de Publicador com a arquitetura de processador correta. O antigo assembly de política do Publicador não pode atender ao seu assembly depois que o assembly tem uma arquitetura de processador diferente.

Outra consequência é que o vinculador da versão 2,0 não pode ser usado para criar um assembly de política do Publicador para um assembly compilado usando versões anteriores do .NET Framework, porque ele sempre especifica a arquitetura do processador.

## <a name="adding-the-publisher-policy-assembly-to-the-global-assembly-cache"></a>Adicionando o assembly de política do Publicador ao cache de assembly global

Use a [ferramenta global assembly cache (Gacutil.exe)](../tools/gacutil-exe-gac-tool.md) para adicionar o assembly de política do Publicador ao cache de assembly global.

### <a name="to-add-the-publisher-policy-assembly-to-the-global-assembly-cache"></a>Para adicionar o assembly de política do Publicador ao cache de assembly global

Digite o seguinte comando no prompt de comando:

```console
gacutil /i publisherPolicyAssemblyFile
```

O comando a seguir adiciona `policy.1.0.myAssembly.dll` ao cache de assembly global.

```console
gacutil /i policy.1.0.myAssembly.dll
```

> [!IMPORTANT]
> O assembly de política do Publicador não pode ser adicionado ao cache de assembly global, a menos que o arquivo de política original do Publicador especificado no `/link` argumento esteja localizado no mesmo diretório que o assembly.

## <a name="see-also"></a>Veja também

- [Programação com assemblies](../../standard/assembly/index.md)
- [Como o runtime localiza assemblies](../deployment/how-the-runtime-locates-assemblies.md)
- [Configurando aplicativos usando arquivos de configuração ](index.md)
- [Esquema de configurações do runtime](./file-schema/runtime/index.md)
- [Esquema de arquivos de configuração](./file-schema/index.md)
- [Redirecionando versões de assembly](redirect-assembly-versions.md)
