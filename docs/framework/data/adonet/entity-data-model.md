---
title: Modelo de Dados de Entidade
description: O Modelo de Dados de Entidade descreve a estrutura de dados, independentemente de sua forma armazenada, que aborda os desafios resultantes do armazenamento de dados em muitas formas.
ms.date: 03/30/2017
ms.assetid: 2dda3d5b-4582-4ba0-a91d-fcd7a1498137
ms.openlocfilehash: c98b1f4559ef297f8b11051940fd91f5f6fa06fd
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286734"
---
# <a name="entity-data-model"></a>Modelo de Dados de Entidade
O EDM (Modelo de Dados de Entidade) é um conjunto de conceitos que descrevem a estrutura de dados, independentemente do formato armazenado. O EDM pede emprestado o modelo de relacionamento entre entidades descrito por Peter Chen em 1976, mas também cria a partir desse modelo e estende seus usos tradicionais.  
  
 O EDM resolve os desafios que ocorrem de ter dados armazenados em vários formatos. Por exemplo, considere um negócio que armazena dados em bancos de dados relacionais, arquivos de texto, arquivos XML, planilhas e relatórios. Isso representa desafios significativos na modelagem, na criação de aplicativos e no acesso a dados. Ao criar um aplicativo orientado a dados, o desafio é escrever código eficiente e sustentável sem sacrificar o acesso eficiente a dados, o armazenamento e a escalabilidade. Quando os dados têm uma estrutura relacional, o acesso a dados, o armazenamento e a escalabilidade são muito eficientes, mas a escrita de código eficiente e sustentável torna-se mais difícil. Quando os dados têm uma estrutura de objeto, os conflitos são inversos: escrever código eficiente e sustentável sacrifica o acesso a dados eficiente, o armazenamento e a escalabilidade. Mesmo que o equilíbrio correto entre esses conflitos seja encontrado, novos desafios surgem quando os dados são movidos de um formato para outro. O Modelo de Dados de Entidade resolve esses desafios descrevendo a estrutura dos dados em termos de entidades e relacionamentos que são independentes de qualquer esquema de armazenamento. Isso torna o formato de dados armazenado de dados irrelevante para a criação e o desenvolvimento de aplicativos. E, como as entidades e os relacionamentos descrevem a estrutura de dados como é usada em um aplicativo (não o formato armazenado), eles podem evoluir junto com o aplicativo.  
  
 Um `conceptual model` é uma representação específica da estrutura de dados como entidades e relações, e é definido geralmente em um DSL (linguagem específica do domínio) que implementa os conceitos do EDM. O [CSDL (linguagem de definição de esquema conceitual)](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec) é um exemplo de tal linguagem específica de domínio. Entidades e relacionamentos descritos em um modelo conceitual podem ser consideradas abstrações de objetos e associações em um aplicativo. Isso permite que os desenvolvedores concentrem-se no modelo conceitual sem se preocuparem com o esquema de armazenamento, e permite que eles escrevam o código pensando em eficiência e facilidade de manutenção. Enquanto isso, os designers de esquema de armazenamento podem se concentrar na eficiência de acesso a dados, armazenamento e escalabilidade.  
  
## <a name="in-this-section"></a>Nesta seção  
 Os tópicos nesta seção descrevem os conceitos do Modelo de Dados de Entidade. Qualquer DSL que implemente o EDM deve incluir os conceitos descritos aqui. Observe que o [Entity Framework ADO.net](./ef/index.md) usa CSDL para definir modelos conceituais. Para obter mais informações, consulte [especificação CSDL](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec).  
  
 [Conceitos chave do Modelo de Dados de Entidade](entity-data-model-key-concepts.md)  
  
 [Modelo de Dados de Entidade: namespaces](entity-data-model-namespaces.md)  
  
 [Modelo de Dados de Entidade: Tipos de dados primitivos](entity-data-model-primitive-data-types.md)  
  
 [Modelo de Dados de Entidade: Herança](entity-data-model-inheritance.md)  
  
 [extremidade de associação](association-end.md)  
  
 [multiplicidade da extremidade da associação](association-end-multiplicity.md)  
  
 [conjunto de associações](association-set.md)  
  
 [extremidade do conjunto de associações](association-set-end.md)  
  
 [tipo de associação](association-type.md)  
  
 [tipo complexo](complex-type.md)  
  
 [contêiner de entidade](entity-container.md)  
  
 [chave de entidade](entity-key.md)  
  
 [conjunto de entidades](entity-set.md)  
  
 [tipo de entidade](entity-type.md)  
  
 [particular](facet.md)  
  
 [propriedade de chave estrangeira](foreign-key-property.md)  
  
 [função declarada por modelo](model-declared-function.md)  
  
 [função definida pelo modelo](model-defined-function.md)  
  
 [propriedade de navegação](navigation-property.md)  
  
 [property](property.md)  
  
 [restrição de integridade referencial](referential-integrity-constraint.md)  
  
## <a name="see-also"></a>Veja também

- [Ferramentas de Modelo de Dados de Entidade de ADO.NET](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb399249(v=vs.100))
- [Visão geral do arquivo. edmx](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/cc982042(v=vs.100))
- [Especificação de CSDL](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)
