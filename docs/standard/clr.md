---
title: Visão geral da CLR (Common Language Runtime) – .NET Framework
description: Introdução ao Common Language Runtime (CLR), o ambiente de tempo de execução do .NET. O CLR executa o código e fornece serviços para facilitar o processo de desenvolvimento.
ms.date: 04/02/2019
ms.technology: dotnet-standard
helpviewer_keywords:
- compiling source code, runtime functionality
- code, execution
- managed data
- runtime
- common language runtime
- metadata, runtime functionality
- .NET Framework, common language runtime
- language compilers
- managed code
- source code execution
- code, runtime functionality
ms.assetid: 059a624e-f7db-4134-ba9f-08b676050482
ms.custom: updateeachrelease
ms.openlocfilehash: ef455ac1c49c1f457d0fa432db91b5375c045840
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84769204"
---
# <a name="common-language-runtime-clr-overview"></a>Visão geral do CLR (Common Language Runtime)

O .NET Framework fornece um ambiente de tempo de execução chamado Common Language Runtime, que executa o código e fornece serviços que facilitam o processo de desenvolvimento.

Compiladores e ferramentas expõem as funcionalidades do Common Language Runtime e permitem que você grave um código que se beneficia desse ambiente de execução gerenciado. O código que você desenvolve com um compilador de linguagem que visa o runtime é chamado de código gerenciado. Ele aproveita recursos como integração em qualquer idioma, tratamento de exceções em qualquer idioma, segurança aprimorada, suporte a controle de versão e implantação, um modelo simplificado para interação entre componentes, além de serviços de depuração e criação de perfil.

> [!NOTE]
> Compiladores e ferramentas podem produzir saída que o Common Language Runtime pode consumir porque o sistema de tipos, o formato dos metadados e o ambiente de tempo de execução (o sistema virtual de execução) são todos definidos por um padrão público, a especificação Common Language Infrastructure ECMA. Para obter mais informações, consulte [Especificações ECMA C# e Common Language Infrastructure](https://visualstudio.microsoft.com/license-terms/ecma-c-common-language-infrastructure-standards/).

Para habilitar o runtime para fornecer serviços de código gerenciado, os compiladores de linguagem devem emitir metadados que descrevam os tipos, os membros e as referências em seu código. Os metadados são armazenados com o código. Todo arquivo PE (Portable Executable) do Common Language Runtime carregável contém metadados. O runtime usa metadados para localizar e carregar classes, dispor instâncias na memória, resolver invocações de método, gerar código nativo, impor segurança e definir limites de contexto do runtime.

O runtime identifica automaticamente o layout de objeto e gerencia referências a objetos, liberando-os quando eles não estão sendo mais usados. Objetos cujos tempos de vida são gerenciados dessa forma são chamados de dados gerenciados. A coleta de lixo elimina perdas de memória, bem como alguns outros erros de programação comuns. Se o código for gerenciado, você poderá usar dados gerenciados, dados não gerenciados ou ambos no seu aplicativo do .NET Framework. Como os compiladores de linguagem fornecem seus próprios tipos, como tipos primitivos, você talvez sem sempre saiba (ou precise saber) se os dados estão sendo gerenciados.

O Common Language Runtime facilita a criação de componentes e aplicativos cujos objetos interagem entre linguagens. Objetos gravados em linguagens diferentes podem se comunicar entre si e seus comportamentos podem estar totalmente integrados. Por exemplo, você pode definir uma classe e usar uma linguagem diferente para derivar uma classe da classe original ou chamar um método na classe original. Você também pode passar uma instância de uma classe para um método de uma classe gravado em uma linguagem diferente. Essa integração em qualquer idioma é possível porque os compiladores de linguagens e ferramentas que segmentam o runtime usam um Common Type System definido pelo runtime e seguem as regras do runtime para definir novos tipos, bem como para criar, usar, manter e associar a tipos.

Como parte de seus metadados, todos os componentes gerenciados transportam informações sobre os componentes e os recursos com base nos quais eles foram compilados. O runtime usa essas informações para garantir que o componente ou o aplicativo tenha as versões especificadas de tudo o que precisa, o que torna seu código menos suscetível à interrupção devido a alguma dependência não atendida. As informações de registro e os dados de estado não são mais armazenados no registro onde pode ser difícil estabelecer e mantê-los. Em vez de isso, as informações sobre os tipos que você define (e suas dependências) são armazenadas com o código como metadados, fazendo com que as tarefas de replicação do componente e remoção sejam muito menos complicadas.

Compiladores de linguagens e ferramentas expõem a funcionalidade do runtime da maneira como seriam úteis e intuitivas para desenvolvedores. Isso significa que alguns recursos do runtime devem ser mais perceptíveis em um ambiente do que em outro. Como você usa o runtime depende de quais compiladores de linguagem ou ferramentas você usa. Por exemplo, se for um desenvolvedor do Visual Basic, você poderá observar que, com o Common Language Runtime, a linguagem do Visual Basic tem mais recursos orientados a objeto do que antes. O runtime oferece os seguintes benefícios:

- Melhorias de desempenho.

- A possibilidade de usar facilmente componentes desenvolvidos em outras linguagens.

- Tipos extensíveis fornecidos por uma biblioteca de classes.

- Recursos de linguagem como herança, interfaces e sobrecarga para programação orientada ao objeto.

- Suporte a threading livre explícito que permite a criação de aplicativos multithread, escalonáveis.

- Suporte ao tratamento de exceções estruturado.

- Suporte a atributos personalizados.

- Coleta de lixo.

- Uso de delegados em vez de ponteiros de função para maior segurança e segurança de tipos. Para saber mais sobre representantes, veja [Common Type System](base-types/common-type-system.md).

## <a name="clr-versions"></a>Versões do CLR

O número de versão de .NET Framework não corresponde necessariamente ao número de versão do CLR que ele inclui. Para obter uma lista de versões de .NET Framework e suas versões correspondentes do CLR, consulte [.NET Framework versões e dependências](../framework/migration-guide/versions-and-dependencies.md). As versões do .NET Core têm uma única versão do produto, ou seja, não há nenhuma versão CLR separada. Para obter uma lista das versões do .NET Core, consulte [baixar o .NET Core](https://dotnet.microsoft.com/download/dotnet-core).

## <a name="related-topics"></a>Tópicos relacionados

|Title|Descrição|
|-----------|-----------------|
|[Processo de execução gerenciada](managed-execution-process.md)|Descreve as etapas obrigatórias para usufruir o Common Language Runtime.|
|[Gerenciamento automático de memória](automatic-memory-management.md)|Descreve como o coletor de lixo aloca e libera memória.|
|[Visão geral do .NET Framework](../framework/get-started/overview.md)|Descreve os conceitos-chave do .NET Framework, como Common Type System, interoperabilidade entre linguagens, execução gerenciada, domínios de aplicativos e assemblies.|
|[Sistema de tipos comum](./base-types/common-type-system.md)|Descreve como os tipos são declarados, usados e gerenciados no runtime para dar suporte à integração entre linguagens.|
