---
title: Considerações de segurança XSLT
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: fea695be-617c-4977-9567-140e820436fc
ms.openlocfilehash: 81db764016607ebe6facfc530dbb2bac8e6b8cfe
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84282501"
---
# <a name="xslt-security-considerations"></a>Considerações de segurança XSLT
O idioma XSLT tem um conjunto rico de recursos que oferecem várias energia e flexibilidade. Inclui muitos recursos que, quando úteis, podem também ser explorados por fontes fora. Para usar com segurança XSLT, você deve compreender os tipos de problemas de segurança que ocorrem ao usar XSLT, e as estratégias básicas que você pode usar para atenuar esses riscos.  
  
## <a name="xslt-extensions"></a>Extensões XSLT  
 Duas extensões populares XSLT são objetos de script e a extensão de folha de estilos. Essas extensões permitem que o processador XSLT executa o código.  
  
- Os objetos de extensão adicionar recursos de programação as transformações XSL.  
  
- Os scripts podem ser inseridos na folha de estilos usando o elemento de extensão de `msxsl:script` .  
  
### <a name="extension-objects"></a>Objetos de extensão  
 Os objetos de extensão são adicionados usando o método <xref:System.Xml.Xsl.XsltArgumentList.AddExtensionObject%2A> . O conjunto de permissões FullTrust é necessário para oferecer suporte objetos de extensão. Isso garante que a elevação de permissões não ocorre quando o código de objeto de extensão é executado. Tentar chamar o método de <xref:System.Xml.Xsl.XsltArgumentList.AddExtensionObject%2A> sem permissões FullTrust de resulta em uma exceção de segurança que está sendo lançada.  
  
### <a name="style-sheet-scripts"></a>Scripts de folha de estilos  
 Os scripts podem ser inseridos em uma folha de estilos usando o elemento de extensão de `msxsl:script` . Suporte de script é um recurso opcional na classe de <xref:System.Xml.Xsl.XslCompiledTransform> que é desativada por padrão. O script pode ser ativado definindo a propriedade de <xref:System.Xml.Xsl.XsltSettings.EnableScript%2A?displayProperty=nameWithType> a `true` e passando o objeto de <xref:System.Xml.Xsl.XsltSettings> para o método de <xref:System.Xml.Xsl.XslCompiledTransform.Load%2A> .  
  
#### <a name="guidelines"></a>Diretrizes  
 Habilitar scripts somente quando a folha de estilos vem de uma fonte confiável. Se você não pode verificar a fonte da folha de estilos, ou se a folha de estilos não vem de uma fonte confiável, passe em `null` para o argumento configurações de fonte.  
  
## <a name="external-resources"></a>Recursos externos  
 O idioma XSLT tem recursos como `xsl:import`, `xsl:include`, ou a função de `document()` , onde o processador precisa resolver referências de um URI. A classe de <xref:System.Xml.XmlResolver> é usada para resolver recursos externos. Os recursos externos podem precisar ser resolvido nos dois seguintes casos:  
  
- Ao criar uma folha de estilos, <xref:System.Xml.XmlResolver> é usado para `xsl:import` e resolução de `xsl:include` .  
  
- Ao executar a transformação, <xref:System.Xml.XmlResolver> é usado para resolver a função de `document()` .  
  
    > [!NOTE]
    > A função de `document()` é desativada por padrão na classe de <xref:System.Xml.Xsl.XslCompiledTransform> . Esse recurso pode ser ativado definindo a propriedade de <xref:System.Xml.Xsl.XsltSettings.EnableDocumentFunction%2A?displayProperty=nameWithType> a `true` e passando o objeto de <xref:System.Xml.Xsl.XsltSettings> para o método de <xref:System.Xml.Xsl.XslCompiledTransform.Load%2A> .  
  
 Os métodos cada um de <xref:System.Xml.Xsl.XslCompiledTransform.Load%2A> e de <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> incluem as sobrecargas que aceitam <xref:System.Xml.XmlResolver> como um dos argumentos. Se <xref:System.Xml.XmlResolver> não for especificado, <xref:System.Xml.XmlUrlResolver> padrão sem credenciais é usado.  
  
#### <a name="guidelines"></a>Diretrizes  
 Ativar a função de `document()` somente quando a folha de estilos vem de uma fonte confiável.  
  
 A lista a seguir descreve quando você pode querer especificar um objeto de <xref:System.Xml.XmlResolver> :  
  
- Se o processo XSLT precisa acessar um recurso de rede que requer autenticação, você pode usar <xref:System.Xml.XmlResolver> com as credenciais necessárias.  
  
- Se você deseja restringir os recursos que o processo XSLT pode acessar, você pode usar <xref:System.Xml.XmlSecureResolver> com o conjunto de permissões correto. Use a classe de <xref:System.Xml.XmlSecureResolver> se você precisa abrir um recurso que você não controle, ou que é não confiável.  
  
- Se você desejar personalizar o comportamento, você pode implementar sua própria classe de <xref:System.Xml.XmlResolver> e usá-la para resolver recursos.  
  
- Se você quiser garantir que nenhum recurso externo é acessado, você pode especificar `null` para o argumento de <xref:System.Xml.XmlResolver> .  
  
## <a name="see-also"></a>Veja também

- [Transformações XSLT](xslt-transformations.md)
- [Resolvendo recursos externos durante processamento XSLT](resolving-external-resources-during-xslt-processing.md)
- [Segurança de acesso do código](../../../framework/misc/code-access-security.md)
