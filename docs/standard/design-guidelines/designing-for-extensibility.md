---
title: Designer voltado para extensibilidade
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- extending class libraries
- extensibility with class libraries in .NET Framework
- class library design guidelines [.NET Framework], extensibility
- class library extensibility [.NET Framework]
ms.assetid: 1cdb8740-871a-456c-9bd9-db96ca8d79b3
ms.openlocfilehash: 406c15b6ce42b637ed1bbb61761d05e040995579
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84280238"
---
# <a name="designing-for-extensibility"></a>Designer voltado para extensibilidade
Um aspecto importante da criação de uma estrutura é garantir que a extensibilidade da estrutura tenha sido cuidadosamente considerada. Isso exige que você compreenda os custos e os benefícios associados a vários mecanismos de extensibilidade. Este capítulo ajuda você a decidir quais dos mecanismos de extensibilidade — a subclasse, os eventos, os membros virtuais, os retornos de chamada e assim por diante — pode atender melhor aos requisitos de sua estrutura.  
  
 Há várias maneiras de permitir a extensibilidade em estruturas. Elas variam de menos poderosas, mas menos dispendiosas a muito poderosas, mas caras. Para qualquer requisito de extensibilidade específico, você deve escolher o mecanismo de extensibilidade menos dispendioso que atenda aos requisitos. Tenha em mente que geralmente é possível adicionar mais extensibilidade posteriormente, mas você nunca pode descartar sem introduzir alterações significativas.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Classes sem lacre](unsealed-classes.md)  
 [Membros protegidos](protected-members.md)  
 [Eventos e retornos de chamada](events-and-callbacks.md)  
 [Membros virtuais](virtual-members.md)  
 [Abstrações (tipos abstratos e interfaces)](abstractions-abstract-types-and-interfaces.md)  
 [Classes base para implementação de abstrações](base-classes-for-implementing-abstractions.md)  
 [Selar](sealing.md)  
 *Partes © 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*  
  
 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*  
  
## <a name="see-also"></a>Veja também

- [Diretrizes de design de estrutura](index.md)
