---
title: Estado e dados em aplicativos do Docker
description: Saiba qual é a opção disponível para salvar o estado nos aplicativos em contêineres.
ms.date: 08/06/2020
ms.openlocfilehash: dc9a1a3eccb77e9fd67e69fd3295f3db1edf5e66
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87915305"
---
# <a name="state-and-data-in-docker-applications"></a>Estado e dados em aplicativos do Docker

Na maioria dos casos, você pode considerar um contêiner uma instância de um processo. Um processo não mantém o estado persistente. Enquanto um contêiner pode gravar em seu armazenamento local, supondo que uma instância permanecerá indefinidamente seria como presumir que um único local na memória será durável. Deve-se supor que imagens de contêiner, assim como processos, têm várias instâncias e que serão eventualmente eliminadas. Caso sejam gerenciadas com um orquestrador de contêineres, deve-se presumir que podem ser movidas de um nó ou VM para outra.

As soluções a seguir são usadas para gerenciar dados persistentes em aplicativos do Docker:

Do host do Docker, como [Volumes do Docker](https://docs.docker.com/engine/admin/volumes/):

- **Volumes** são armazenados em uma área do sistema de arquivos de host que é gerenciado pelo Docker.

- **Montagens de associação** podem mapear para qualquer pasta no sistema de arquivos de host, portanto, o acesso não pode ser controlado do processo do Docker e pode representar um risco de segurança, já que um contêiner pode acessar pastas confidenciais do sistema operacional.

- **Montagens tmpfs** são como pastas virtuais que só existem na memória do host e nunca são gravadas no sistema de arquivos.

Do armazenamento remoto:

- O [Armazenamento do Azure](https://azure.microsoft.com/documentation/services/storage/) oferece armazenamento geograficamente distribuído, fornecendo uma boa solução de persistência de longo prazo para contêineres.

- Os bancos de dados relacionais remotos, como [Banco de Dados SQL do Azure](https://azure.microsoft.com/services/sql-database/); bancos de dados NoSQL, como o [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction); ou serviços de cache, como o [Redis](https://redis.io/).

Do contêiner do Docker:

- O Docker oferece um recurso chamado *sistema de arquivos de sobreposição*. Ele implementa uma tarefa de cópia em gravação que armazena informações atualizadas no sistema de arquivos de raiz do contêiner. Essas informações são adicionais à imagem original na qual o contêiner se baseia. Se o contêiner for excluído do sistema, essas alterações serão perdidas. Portanto, embora seja possível salvar o estado de um contêiner em seu armazenamento local, criar um sistema com base nesse recurso entraria em conflito com o local do projeto do contêiner, que por padrão é sem estado.

- No entanto, os Volumes do Docker agora são a maneira preferencial de lidar com os dados locais no Docker. Se você precisar de mais informações sobre o armazenamento em contêineres, procure em [Drivers de armazenamento do Docker](https://docs.docker.com/engine/userguide/storagedriver/) e [Sobre imagens, contêineres e drivers de armazenamento](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/).

O exemplo a seguir fornece mais detalhes sobre essas opções.

**Volumes** são diretórios mapeados do sistema operacional do host para diretórios em contêineres. Quando o código no contêiner tem acesso ao diretório, o acesso é, na verdade, a um diretório no sistema operacional host. Esse diretório não está vinculado ao tempo de vida do contêiner em si, sendo que o diretório é gerenciado pelo Docker e isolado em relação à funcionalidade básica do computador host. Assim, os volumes de dados são projetados para persistir dados independentemente do tempo de vida do contêiner. Se você excluir um contêiner ou uma imagem do host do Docker, os dados persistentes no volume de dados não serão excluídos.

Volumes podem ser nomeados ou anônimos (o padrão). Volumes nomeados são a evolução dos **Contêineres de Volume de Dados** e facilitam o compartilhamento de dados entre contêineres. Volumes também são compatíveis com drivers de volume, que permitem armazenar dados em hosts remotos, entre outras opções.

As **montagens de associação** estão disponíveis há muito tempo e permitem o mapeamento de qualquer pasta para um ponto de montagem em um contêiner. Montagens de associação têm mais limitações que os volumes e alguns problemas de segurança importantes, por isso os volumes são a opção recomendada.

as ** `tmpfs` montagens** são pastas virtuais que residem apenas na memória do host e nunca são gravadas no sistema de arquivos. Elas são rápidas e seguras, mas usam a memória e destinam-se somente a dados não persistentes.

Conforme mostrado na Figura 4-5, os volumes Docker regulares podem ser armazenados fora dos contêineres de si, mas dentro dos limites físicos do servidor host ou VM. No entanto, contêineres do Docker não podem acessar um volume de um servidor host ou VM para outro. Em outras palavras, com esses volumes, não é possível gerenciar os dados compartilhados entre contêineres executados em hosts diferentes do Docker, embora esses dados possam ser obtidos com um driver de volume que dá suporte a hosts remotos.

![Diagrama mostrando os volumes do Docker armazenados fora dos contêineres.](./media/state-and-data-in-docker-applications/container-based-application-external-data-sources.png)

**Figura 4-5**. Volumes e fontes de dados externas para aplicativos baseados em contêiner

Além disso, quando gerenciados por um orquestrador, contêineres do Docker podem se "mover" entre os hosts de acordo com as otimizações realizadas pelo cluster. Portanto, não é recomendável usar volumes de dados para dados empresariais. Porém, eles são um bom mecanismo para trabalhar com arquivos de rastreamento, arquivos temporais ou similares que não afetarão a consistência dos dados de negócios.

**Fontes de dados remotas e ferramentas de cache** como o Banco de Dados SQL do Azure, o Azure Cosmos DB ou um cache remoto como o Redis podem ser usados em aplicativos em contêineres da mesma forma que são usados durante o desenvolvimento sem contêineres. Essa é uma forma comprovada de armazenar dados de aplicativo de negócios.

**Armazenamento do Azure.** Os dados de negócios geralmente precisam ser colocados em recursos ou bancos de dados externos, como o Armazenamento do Azure. O Armazenamento do Azure fornece os seguintes serviços na nuvem:

- O Armazenamento de Blobs armazena dados de objeto não estruturados. Um blob pode ser qualquer tipo de texto ou dados binários, como documentos ou arquivos de mídia (imagens, áudio e vídeo). O Armazenamento de Blobs também é chamado de armazenamento de Objeto.

- O Armazenamento de Arquivos oferece o armazenamento compartilhado para aplicativos herdados com o protocolo SMB Standard. Os serviços de nuvem e as máquinas virtuais do Azure podem compartilhar dados de arquivos em vários componentes de aplicativo por meio de compartilhamentos montados. Os aplicativos locais podem acessar dados de arquivo em um compartilhamento por meio da API REST do Serviço de Arquivo.

- O Armazenamento de Tabela armazena conjuntos de dados estruturados. O Armazenamento de Tabelas é um armazenamento de dados do atributo-chave NoSQL, que permite o rápido desenvolvimento e acesso a grandes quantidades de dados.

**Bancos de dados relacionais e bancos de dados NoSQL.** Há muitas opções para bancos de dados externos, de bancos de dados relacionais como bancos de dados SQL Server, PostgreSQL, Oracle ou NoSQL, como Azure Cosmos DB, MongoDB, etc. Esses bancos de dados não serão explicados como parte deste guia, pois eles são um tópico diferente.

>[!div class="step-by-step"]
>[Anterior](monolithic-applications.md) 
> [Avançar](soa-applications.md)
