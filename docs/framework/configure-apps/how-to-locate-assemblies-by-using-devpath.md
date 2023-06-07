---
title: 'Como: Localizar assemblies usando DEVPATH'
description: Teste se um assembly compartilhado funciona corretamente com muitos aplicativos no .NET usando um arquivo de configuração de computador XML e a variável de ambiente DEVPATH.
ms.date: 03/30/2017
helpviewer_keywords:
- DEVPATH
- .NET Framework application configuration, assemblies
- application configuration files, specifying assembly's location
- app.config files, assembly locations
- locating assemblies
- assemblies [.NET Framework], location
ms.assetid: 44d2eadf-7eec-443c-a2ac-d601fd919e17
ms.openlocfilehash: 50b61eedddabd660b1834565a61738f460ae9ff9
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85105373"
---
# <a name="how-to-locate-assemblies-by-using-devpath"></a>Como: Localizar assemblies usando DEVPATH
Os desenvolvedores podem querer ter certeza de que um assembly compartilhado que está compilando funciona corretamente com vários aplicativos. Em vez de colocar continuamente o assembly no cache de assembly global durante o ciclo de desenvolvimento, o desenvolvedor pode criar uma variável de ambiente DEVPATH que aponta para o diretório de saída da compilação para o assembly.  
  
 Por exemplo, suponha que você esteja criando um assembly compartilhado chamado MySharedAssembly e o diretório de saída seja C:\MySharedAssembly\Debug. Você pode colocar C:\MySharedAssembly\Debug na variável DEVPATH. Em seguida, você deve especificar o [\<developmentMode>](./file-schema/runtime/developmentmode-element.md) elemento no arquivo de configuração da máquina. Esse elemento informa ao Common Language Runtime usar DEVPATH para localizar assemblies.  
  
 O assembly compartilhado deve ser detectável pelo tempo de execução.  Para especificar um diretório privado para resolver referências de assembly, use o [ \<codeBase> elemento](./file-schema/runtime/codebase-element.md) ou [ \<probing> elemento](./file-schema/runtime/probing-element.md) em um arquivo de configuração, conforme descrito em [especificando o local de um assembly](specify-assembly-location.md).  Você também pode colocar o assembly em um subdiretório do diretório do aplicativo. Para saber mais, confira [Como o runtime localiza os assemblies](../deployment/how-the-runtime-locates-assemblies.md).  
  
> [!NOTE]
> Esse é um recurso avançado, destinado apenas ao desenvolvimento.  
  
 O exemplo a seguir mostra como fazer com que o tempo de execução procure por assemblies em diretórios especificados pela variável de ambiente DEVPATH.  
  
## <a name="example"></a>Exemplo  
  
```xml  
<configuration>  
  <runtime>  
    <developmentMode developerInstallation="true"/>  
  </runtime>  
</configuration>  
```  
  
 Essa configuração assume como padrão false.  
  
> [!NOTE]
> Use essa configuração somente no momento do desenvolvimento. O tempo de execução não verifica as versões em assemblies de nome forte encontrados no DEVPATH. Ele simplesmente usa o primeiro assembly que encontra.  
  
## <a name="see-also"></a>Veja também

- [Configurando aplicativos usando arquivos de configuração ](index.md)
