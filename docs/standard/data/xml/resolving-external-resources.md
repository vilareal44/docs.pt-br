---
title: Resolvendo recursos externos
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: ad3fa320-4b8f-4e5c-b549-01157591007a
ms.openlocfilehash: 82e9231be8a3619f59313460f0d5e0b246eb9436
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291481"
---
# <a name="resolving-external-resources"></a>Resolvendo recursos externos
A propriedade **XmlResolver** do **XmlDocument** é usada pela classe **XmlDocument** para localizar os recursos que não são embutidos nos dados XML, como definições de tipo de documento (DTDs), entidades e esquemas externos. Esses itens podem ser localizados em uma rede ou em uma unidade local, e são identificáveis por Uniform Resource Identifier (URI). Isso permite que o **XmlDocument** resolva os nós de **EntityReference** presentes no documento e valide o documento de acordo com DTDs ou esquema externos.  
  
## <a name="fully-trusted-xmldocument"></a>XmlDocument totalmente confiável  
 A propriedade **XmlResolver** afeta a funcionalidade do método **XmlDocument.Load**. A tabela abaixo mostra como funciona a propriedade **XmlDocument.XmlResolver** quando o objeto **XmlDocument** é totalmente confiável. A tabela a seguir mostra os métodos **XmlDocument.Load** quando a entrada para a carga é **TextReader**, **String**, **Stream** ou **URI**. Esta tabela não se aplicará ao método **Carga** se o **XmlDocument** for carregado do **XmlReader**.  
  
|Propriedade de XmlResolver|Função|Observações|  
|--------------------------|--------------|-----------|  
|A propriedade é definida como classe **XmlResolver** criada previamente e com as propriedades já definidas nela pelo usuário.|O **XmlDocument** usa o **XmlResolver** fornecido para resolver nomes de arquivo e referências a recursos externos, como DTDs, entidades e esquemas.<br /><br /> O **XmlResolver** também é usado para resolver recursos externos necessários ao adicionar ou editar nós no **XmlDocument**.|O **XmlResolver** dado ao **XmlDocument** é o resolvedor usado sempre que recursos externos precisam ser localizados e resolvidos.|  
|A propriedade é definida como **nula** (**Nothing** no Microsoft Visual Basic .NET).|Os recursos que exigem um recurso externo não são suportados, como localizar um esquema externo, ou o DTD. Nem as entidades externos serão resolvidas, e executar funções de edição, como inserir os nós de entidade que exigem resolução, não é suportado.|O **XmlDocument** carrega arquivos como anônimos e não tenta resolver qualquer outro recurso.|  
|A propriedade não está definida, mas esquerda em seu estado padrão.|Um **XmlUrlResolver** com credenciais NULAS será instanciado e usado pelo **XmlDocument** ao resolver nomes de arquivos e localizar DTDs, entidades e esquemas externos, e as credenciais **nulas** serão usadas ao editar nós.||  
  
 A tabela a seguir mostra o método **XmlDocument.Load** quando a entrada à **Carga** é um **XmlReader** e o **XmlDocument** é totalmente confiável.  
  
|Propriedade de XmlResolver|Função|Observações|  
|--------------------------|--------------|-----------|  
|A classe **XmlResolver** usada pelo **XmlDocument** é a mesma que está sendo usada pelo **XmlReader**.|O **XmlDocument** usa o **XmlResolver** atribuído ao **XmlReader**.<br /><br /> A propriedade **XmlDocument.Resolver** não pode ser definida, independentemente do nível de confiança do **XmlDocument**, porque está obtendo um **XmlResolver** do **XmlReader**. Não tente substituir as configurações de **XmlResolver** do **XmlReader** definindo a propriedade do **XmlResolver** do **XmlDocument**.|O **XmlReader** pode ser o **XmlTextReader**, o **XmlValidatingReader** ou um leitor personalizado. Se o leitor usado oferece suporte a resolução de entidade, as entidades externos são resolvidas. Se o leitor passado não oferece suporte a referências a entidades, então as referências a entidades não são resolvidas.|  
  
## <a name="semi-trusted-xmldocument"></a>XmlDocument de confiança parcial  
 A tabela a seguir mostra como funciona a propriedade **XmlDocument.XmlResolver** quando o objeto é de confiança parcial. Esta tabela se aplica aos métodos **XmlDocument.Load** quando a entrada à Carga é um **TextReader**, uma **String**, um **Stream** ou **URI**. Esta tabela não se aplicará ao método **Carga** se o **XmlDocument** for carregado do **XmlReader**.  
  
|Propriedade de XmlResolver|Função|Observações|  
|--------------------------|--------------|-----------|  
|No cenário de confiança parcial, a propriedade **XmlResolver** só pode ser definida como nula.|Um **XmlUrlResolver** com credenciais **nulas** será instanciado e usado pelo **XmlDocument** ao resolver nomes de arquivo, localizar DTDs, entidades e esquemas externos, e credenciais **nulas** serão usadas ao editar nós.|Esse comportamento é idêntico ao apresentado quando a propriedade **XmlResolver** não está definida, mas deixada no estado padrão.<br /><br /> O **XmlDocument** usa permissões anônimas para todas as ações.|  
|A propriedade é definida como **nula** (**Nothing** no Microsoft Visual Basic .NET).|Nenhum recurso que requer um recurso externo é suportado, como localizar um esquema externo ou um DTD. Nem as entidades externos serão resolvidas, e executar funções de edição como inserir os nós de entidade que exigem resolução, não é suportado.|Quando a propriedade é **nula**, o comportamento é o mesmo, não importando se o **XmlDocument** é total ou parcialmente confiável.|  
|A propriedade não está definida, mas esquerda em seu estado padrão.|Um **XmlUrlResolver** com credenciais **nulas** será instanciado e usado pelo **XmlDocument** ao resolver nomes de arquivo, localizar DTDs, entidades e esquemas externos, e credenciais **nulas** serão usadas ao editar nós.|O **XmlDocument** usa permissões anônimas para todas as ações.|  
  
 Esta tabela se aplica ao método **XmlDocument.Load** quando a entrada à **Carga** é um **XmlReader** e o **XmlDocument** tem confiança parcial.  
  
|Propriedade de XmlResolver|Função|Observações|  
|--------------------------|--------------|-----------|  
|A classe **XmlResolver** usada pelo **XmlDocument** é a mesma que está sendo usada pelo **XmlReader**.|O **XmlDocument** usa o **XmlResolver** atribuído ao **XmlReader**.<br /><br /> A propriedade **XmlDocument.Resolver** não pode ser definida, independentemente do nível de confiança do **XmlDocument**, porque está obtendo um **XmlResolver** do **XmlReader**. Você não pode tentar substituir as configurações do **XmlReader** **XmlResolver** definindo a propriedade **XmlResolver** do **XmlDocument**.|O **XmlReader** pode ser o **XmlTextReader**, o <xref:System.Xml.XmlReader> de validação ou um leitor personalizado. Se o leitor usado oferece suporte a resolução de entidade, as entidades externos são resolvidas. Se o leitor passado não oferece suporte a referências a entidades, então as referências a entidades não são resolvidas.|  
  
 Defina o XmlResolver para conter as credenciais corretas permite acesso aos recursos externos.  
  
> [!NOTE]
> Não é possível recuperar a propriedade **XmlResolver**. Isso ajuda a impedir que um usuário reutilize um **XmlResolver** em que as credenciais foram definidas. Além disso, se um **XmlTextReader** ou um <xref:System.Xml.XmlReader> de validação for usado para carregar o **XmlDocument** e o **XmlDocument** tiver um resolvedor definido, os resolvedores desses leitores não serão armazenados em cache pelo **XmlDocument** após a fase de **Carga**, pois isso também apresenta um risco de segurança.  
  
 Para obter mais informações, consulte a seção de Comentários da página de referência <xref:System.Xml.XmlResolver>.  
  
## <a name="see-also"></a>Veja também

- [XML Document Object Model (DOM)](xml-document-object-model-dom.md)
