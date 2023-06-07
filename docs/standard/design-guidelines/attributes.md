---
title: Atributos
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- attributes [.NET Framework], about
- class library design guidelines [.NET Framework], attributes
ms.assetid: ee0038ef-b247-4747-a650-3c5c5cd58d8b
ms.openlocfilehash: 3c0e1b8c20042c085d4ace996a084cbd464d3b21
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617555"
---
# <a name="attributes"></a>Atributos
<xref:System.Attribute?displayProperty=nameWithType>é uma classe base usada para definir atributos personalizados.

 Atributos são anotações que podem ser adicionadas a elementos de programação, como assemblies, tipos, membros e parâmetros. Eles são armazenados nos metadados do assembly e podem ser acessados em tempo de execução usando as APIs de reflexão. Por exemplo, a estrutura define o <xref:System.ObsoleteAttribute> , que pode ser aplicado a um tipo ou um membro para indicar que o tipo ou o membro foi preterido.

 Os atributos podem ter uma ou mais propriedades que contêm dados adicionais relacionados ao atributo. Por exemplo, `ObsoleteAttribute` o pode conter informações adicionais sobre a versão na qual um tipo ou um membro foi preterido e a descrição das novas APIs que substituem a API obsoleta.

 Algumas propriedades de um atributo devem ser especificadas quando o atributo é aplicado. Eles são chamados de propriedades obrigatórias ou argumentos necessários, pois são representados como parâmetros de Construtor posicionais. Por exemplo, a <xref:System.Diagnostics.ConditionalAttribute.ConditionString%2A> propriedade de <xref:System.Diagnostics.ConditionalAttribute> é uma propriedade necessária.

 As propriedades que não precisam necessariamente ser especificadas quando o atributo é aplicado são chamadas de propriedades opcionais (ou argumentos opcionais). Elas são representadas por propriedades de tabela. Os compiladores fornecem uma sintaxe especial para definir essas propriedades quando um atributo é aplicado. Por exemplo, a <xref:System.AttributeUsageAttribute.Inherited%2A?displayProperty=nameWithType> propriedade representa um argumento opcional.

 ✔️ as classes de atributo personalizado de nome com o sufixo "Attribute".

 ✔️ aplicar o <xref:System.AttributeUsageAttribute> aos atributos personalizados.

 ✔️ fornece propriedades configurável para argumentos opcionais.

 ✔️ fornece propriedades somente obtenção para os argumentos necessários.

 ✔️ fornece parâmetros de construtor para inicializar propriedades correspondentes aos argumentos necessários. Cada parâmetro deve ter o mesmo nome (embora com maiúsculas e minúsculas diferentes) como a propriedade correspondente.

 ❌Evite fornecer parâmetros de construtor para inicializar propriedades correspondentes aos argumentos opcionais.

 Em outras palavras, não têm propriedades que podem ser definidas com um construtor e um setter. Essa diretriz torna muito explícito quais argumentos são opcionais e que são necessários e evita que você tenha duas maneiras de fazer a mesma coisa.

 ❌EVITE sobrecarregar construtores de atributos personalizados.

 Ter apenas um construtor se comunica claramente ao usuário quais argumentos são necessários e quais são opcionais.

 ✔️ as classes de atributo personalizado de lacre, se possível. Isso torna a pesquisa para o atributo mais rápida.

 *Partes &copy; 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Consulte também

- [Diretrizes de design de estrutura](index.md)
- [Diretrizes de uso](usage-guidelines.md)
