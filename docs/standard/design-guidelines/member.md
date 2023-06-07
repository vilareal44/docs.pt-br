---
title: Diretrizes de design de membro
description: Aprenda as diretrizes de design de membro no .NET. Os membros incluem métodos, propriedades, eventos, construtores e campos.
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- member design guidelines [.NET Framework], about member design guidelines
- members [.NET Framework], design guidelines
- class library design guidelines [.NET Framework], members
- member design guidelines [.NET Framework]
ms.assetid: 0ce93180-1d7b-4f8c-9306-f828b2d66b8f
ms.openlocfilehash: a1f0c1d74e8bffa7cfef975c7dafb9fd01479470
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594485"
---
# <a name="member-design-guidelines"></a>Diretrizes de design de membro
Métodos, propriedades, eventos, construtores e campos são chamados coletivamente de membros. Os membros são, em última instância, o meio pelo qual a funcionalidade de estrutura é exposta aos usuários finais de uma estrutura.  
  
 Os membros podem ser virtuais ou não virtuais, concretos ou abstratos, estáticos ou de instância, e podem ter vários escopos diferentes de acessibilidade. Toda essa variedade fornece uma expressividade incrível, mas, ao mesmo tempo, requer cuidado com a parte do designer de Framework.  
  
 Este capítulo oferece diretrizes básicas que devem ser seguidas durante a criação de membros de qualquer tipo.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Sobrecarga de membros](member-overloading.md)  
 [Design de propriedade](property.md)  
 [Design do Construtor](constructor.md)  
 [Design de eventos](event.md)  
 [Design de campo](field.md)  
 [Métodos de Extensão](extension-methods.md)  
 [Sobrecargas de operador](operator-overloads.md)  
 [Design de parâmetro](parameter-design.md)  
 *Partes © 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*  
  
 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*  
  
## <a name="see-also"></a>Confira também

- [Diretrizes de design de estrutura](index.md)
