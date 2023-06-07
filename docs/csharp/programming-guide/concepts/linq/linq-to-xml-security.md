---
title: Segurança do LINQ to XML (C#)
description: Saiba mais sobre os problemas de segurança associados ao LINQ to XML, incluindo algumas diretrizes para atenuar a exposição da segurança.
ms.date: 07/20/2015
ms.assetid: ef2c0dc9-ecf9-4c17-b24e-144184ab725f
ms.openlocfilehash: dc9fd13f19dcf6d9cbbb2b0b7608009cc4da1108
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87165314"
---
# <a name="linq-to-xml-security-c"></a>Segurança do LINQ to XML (C#)
Este tópico descreve problemas de segurança associadas LINQ to XML. Além disso, fornece alguma orientação para a exposição de segurança de abrandamento.  
  
## <a name="linq-to-xml-security-overview"></a>Visão geral de segurança LINQ to XML  
 LINQ to XML é criado mais para sua conveniência de programação do que para aplicativos do lado com requisitos de segurança estritos. A maioria das situações XML consistem processar documentos XML de confiança, em vez de processar os documentos XML não confiáveis que são carregados em um servidor. LINQ to XML é otimizado para esses cenários.  
  
 Se você deve processar dados não confiáveis de fontes desconhecidas, Microsoft recomendável que você use uma instância da classe de <xref:System.Xml.XmlReader> que foi configurada para filtrar ataques conhecidos para fora de (DoS) de negação de serviço XML.  
  
 Se você configurou <xref:System.Xml.XmlReader> para atenuar ataques de negação de serviço, você pode usar esse leitor para preencher uma árvore LINQ to XML e a tirá-lo ainda beneficia dos aprimoramentos de produtividade do programador LINQ to XML. Várias técnicas mitigação envolvem criar os leitores que são configurados para reduzir a problema de segurança, e então instanciar uma árvore XML através do leitor configurado.  
  
 XML é intrinsecamente vulnerável a ataques de negação de serviço como documentos são ilimitadas em tamanho, a profundidade, o tamanho do nome do elemento, e mais. Independentemente do componente que você usa para processar XML, você sempre deve estar preparado para reciclar o domínio de aplicativo usa recursos excessivos.  
  
## <a name="mitigation-of-xml-xsd-xpath-and-xslt-attacks"></a>Redução de XML, XSD, XPath, e de ataques XSLT  
 LINQ to XML é compilado em cima de <xref:System.Xml.XmlReader> e de <xref:System.Xml.XmlWriter>. LINQ to XML suporta XSD e o XPath com métodos de extensão em <xref:System.Xml.Schema?displayProperty=nameWithType> e nos namespaces de <xref:System.Xml.XPath?displayProperty=nameWithType> . Usando <xref:System.Xml.XmlReader>, <xref:System.Xml.XPath.XPathNavigator>, e em conjunto com classes LINQ to XML de <xref:System.Xml.XmlWriter> , você pode chamar XSLT para transformar árvores XML.  
  
 Se você estiver trabalhando no menos ambiente seguro, há um número de problemas de segurança que são associadas com XML e o uso de classes em <xref:System.Xml?displayProperty=nameWithType>, em <xref:System.Xml.Schema?displayProperty=nameWithType>, em <xref:System.Xml.XPath?displayProperty=nameWithType>, e em <xref:System.Xml.Xsl?displayProperty=nameWithType>. Esses problemas incluem, mas não estão limitados a, o seguinte:  
  
- XSD, XPath, e XSLT são linguagens cadeia de caracteres- baseados em que você pode especificar as operações que consomem muito tempo ou memória. É responsabilidade de programadores do aplicativo que recebem XSD, o XPath, ou cadeias de caracteres XSLT de fontes não confiáveis para validar que as cadeias de caracteres não são mal-intencionados, ou para monitorar e reduzir a possibilidade que avaliar estas cadeias de caracteres resultará ao consumo excessivo de recurso do sistema.  
  
- Os esquemas XSD (incluindo esquemas in-line) são inerentemente vulneráveis a ataques de negação de serviço; você não deve aceitar esquemas de fontes não confiáveis.  
  
- XSD e XSLT podem incluir referências a outros arquivos, e essas referências podem levar a ataques entre zona e entre domínios.  
  
- As entidades externas nos DTDs pode levar a ataques entre zona e entre domínios.  
  
- Os DTDs é vulnerável a ataques de negação de serviço.  
  
- Documentos XML exclusivamente profundos podem disparar problemas de negação de serviço; talvez você queira limitar o tamanho dos documentos XML.  
  
- Aceitar componentes de suporte, como <xref:System.Xml.NameTable>, <xref:System.Xml.XmlNamespaceManager>, e objetos de <xref:System.Xml.XmlResolver> , assemblies não confiáveis.  
  
- Ler dados em partes para atenuar grandes ataques do documento.  
  
- Blocos de script em folhas de estilos XSLT podem expor um número de ataques.  
  
- Validar cuidadosamente antes de construir expressões XPath dinâmicos.  
  
## <a name="linq-to-xml-security-issues"></a>Problemas de segurança LINQ to XML  
 Questões de segurança neste tópico não são apresentadas em qualquer ordem específica. Todos os problemas são importantes e devem ser endereçados conforme apropriado.  
  
 Um ataue de elevação de privilégio fornece a um conjunto mal-intencionado mais controle sobre seu ambiente. Um ataue de elevação de privilégio pode levar a divulgação de dados, negação de serviço, e mais.  
  
 Aplicativos não devem revelar dados para os usuários que não estão autorizados consulte os dados.  
  
 Ataques de negação de serviço faz com que o analisador XML ou LINQ to XML consome quantidades de memória excessivas ou processador central - tempo. Ataques de negação de serviço são considerados menos severos da ataques de elevação de privilégio ou de divulgação de ataques de dados. No entanto, são importantes em um cenário onde um servidor precisa processar documentos XML de fontes não confiáveis.  
  
### <a name="exceptions-and-error-messages-might-reveal-data"></a>Exceções e mensagens de erro podem revelar dados  
 A descrição de um erro pode revelar dados, como os dados que estão sendo convertidos, nomes de arquivo, ou detalhes de implementação. Mensagens de erro não devem ser expostos aos chamadores que não são expostos. Devido você capturar erros e erros de relatório com suas próprias mensagens de erro personalizadas.  
  
### <a name="do-not-call-codeaccesspermissionsassert-in-an-event-handler"></a>Não chamar CodeAccessPermissions.Assert em um manipulador de eventos  
 Um assembly pode ter o invés ou mais permissões. Um assembly que tem mais permissões tem maior controle sobre o computador e seus ambientes.  
  
 Se o código em um assembly com mais permissões chama <xref:System.Security.CodeAccessPermission.Assert%2A?displayProperty=nameWithType> em um manipulador de eventos, e a árvore XML é passada a um conjunto mal-intencionado que restringir permissões, o conjunto mal-intencionado pode causar um evento a ser gerado. Como o evento executa o código que está no assembly com mais permissões, o conjunto mal-intencionado então seria operando com privilégios elevados.  
  
 Microsoft recomendável que você nunca chama <xref:System.Security.CodeAccessPermission.Assert%2A?displayProperty=nameWithType> em um manipulador de eventos.  
  
### <a name="dtds-are-not-secure"></a>Os DTDs não é seguro  
 As entidades nos DTDs não são inerentemente seguros. É possível para um documento XML mal-intencionado que contém um DTD para fazer com que o analisador use qualquer memória e processador central - tempo, causando um ataque de negação de serviço. Portanto, em LINQ to XML, o processamento de DTD é desativada por padrão. Você não deve aceitar os DTDs de fontes não confiáveis.  
  
 Um exemplo de aceitar os DTDs de fontes não confiáveis é um aplicativo da Web que permite que os usuários carreguem Web de um arquivo XML que referencia um DTD e um arquivo de DTD. Em cima de validação de arquivo, um DTD mal-intencionado pode executar um ataque de negação de serviço no seu servidor. Um exemplo de aceitar os DTDs de fontes não confiáveis é fazer referência a um DTD em um compartilhamento de rede que também permite acesso de FTP anônimo.  
  
### <a name="avoid-excessive-buffer-allocation"></a>Evite a alocação excessiva de buffer  
 Desenvolvedores de aplicativos devem estar cientes que as fontes de dados muito grandes podem levar a exaustão e a ataques de negação de serviço de recurso.  
  
 Se um usuário mal-intencionado envia ou carregar um documento XML muito grande, pode fazer com que LINQ to XML consome recursos do sistema excessivos. Isso pode constituir um ataque de negação de serviço. Para evitar isso, você pode definir a propriedade de <xref:System.Xml.XmlReaderSettings.MaxCharactersInDocument%2A?displayProperty=nameWithType> , e cria um leitor que é delimitado no tamanho de documento que pode carregar. Você usa o leitor para criar a árvore XML.  
  
 Por exemplo, se você souber que o tamanho esperado máximo dos documentos XML que vêm de uma fonte não confiável será menor que os bytes 50K, defina <xref:System.Xml.XmlReaderSettings.MaxCharactersInDocument%2A?displayProperty=nameWithType> a 100.000. Isso não impedirá seu processamento de documentos XML, e ao mesmo tempo abrandará as ameaças de negação de serviço onde os documentos podem ser carregados que consumiriam grandes quantidades de memória.  
  
### <a name="avoid-excess-entity-expansion"></a>Evite a expansão adicional de entidade  
 Um de ataques de negação de serviço conhecidas quando usar um DTD for um documento que causa a expansão excessiva de entidade. Para evitar isso, você pode definir a propriedade de <xref:System.Xml.XmlReaderSettings.MaxCharactersFromEntities%2A?displayProperty=nameWithType> , e cria um leitor que é delimitado no número de caracteres resultantes de expansão de entidade. Você usa o leitor para criar a árvore XML.  
  
### <a name="limit-the-depth-of-the-xml-hierarchy"></a>Limitar o tamanho da hierarquia XML  
 Um ataque de negação de serviço é possível quando um documento é enviado que possui a profundidade excessiva a hierarquia. Para evitar isso, você pode envolver <xref:System.Xml.XmlReader> em sua própria classe que a conta profundidade de elementos. Se o tamanho excede um nível razoável pré-determinado, você pode finalizar o processamento de documento mal-intencionado.  
  
### <a name="protect-against-untrusted-xmlreader-or-xmlwriter-implementations"></a>Proteger contra implementações não confiáveis de XmlReader ou de XmlWriter  
 Os administradores devem verificar que alguns externamente fornecer <xref:System.Xml.XmlReader> ou implementações de <xref:System.Xml.XmlWriter> tem nomes fortes e ter sido registrados na configuração do computador. Isso evita código mal-intencionado de masquerading como um leitor ou gravador de ser carregado.  
  
### <a name="periodically-free-objects-that-reference-xname"></a>Liberar periodicamente os objetos que referenciam XName  
 Para proteger contra determinados tipos de ataques, programadores de aplicativo deve liberar todos os objetos que fazem referência a um objeto de <xref:System.Xml.Linq.XName> no domínio de aplicativo todo base regular.  
  
### <a name="protect-against-random-xml-names"></a>Proteger contra nomes aleatórios XML  
 Os aplicativos que recebem dados de fontes não confiáveis devem considerar usar <xref:System.Xml.XmlReader> que está envolvido no código personalizado para verificar a possibilidade de nomes aleatórios e de namespaces XML. Se tais nomes aleatórios e namespaces XML são detectados, o aplicativo pode então terminar o processamento de documento mal-intencionado.  
  
 Talvez você queira limitar o número de nomes em qualquer namespace determinada (incluindo nomes em qualquer namespace) a um limite razoável.  
  
### <a name="annotations-are-accessible-by-software-components-that-share-a-linq-to-xml-tree"></a>As anotações são acessíveis por componentes de software que compartilham uma árvore LINQ to XML  
 LINQ to XML pode ser usado para criar pipelines de processamento em que os diferentes componentes do aplicativo, carregam valida, consulta, transformações, atualizar, e salvar os dados XML que são passados entre componentes como árvores XML. Isso pode ajudar a otimizar o desempenho, porque a sobrecarga de carregamento e objetos serializar ao texto XML é feita apenas termina de pipeline. Os desenvolvedores devem estar cientes, entretanto, que todas as anotações e manipuladores de eventos criados por um componente são acessíveis a outros componentes. Isso pode criar um número de vulnerabilidades se os componentes têm diferentes níveis de confiança. Para compilar proteger pipelines através de componentes menos confiável, você deve serializar objetos LINQ to XML ao texto XML antes de passar os dados a um componente não confiável.  
  
 Qualquer segurança é fornecida pelo Common Language Runtime (CLR). Por exemplo, um componente que não inclui uma classe privada não pode acessar as anotações fechadas pela classe. No entanto, as anotações podem ser excluídas por componentes que não podem ler os. Isso pode ser usado como um ataque violação.  
  
## <a name="see-also"></a>Confira também

- [Guia de Programação (LINQ to XML) (C#)](linq-to-xml-overview.md)
