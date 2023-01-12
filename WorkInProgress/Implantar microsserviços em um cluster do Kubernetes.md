- [Ir para Conteúdo](https://docs.oracle.com/pt-br/solutions/deploy-microservices/index.html#maincontent)
- [Ir para Pesquisa](https://docs.oracle.com/pt-br/solutions/deploy-microservices/index.html#skip_to_search_form)
- [Página Inicial](https://docs.oracle.com/)

[**Help Center**](https://docs.oracle.com/)

Implantar microsserviços em um cluster do Kubernetes







[Centro de Arquitetura](https://docs.oracle.com/solutions/?lang=pt-br)[Arquiteturas de Referência](https://docs.oracle.com/solutions/?lang=pt-br&cType=reference-architectures)

# Implantar microsserviços em um cluster do Kubernetes

[ Voltar às Soluções](https://docs.oracle.com/pt-br/solutions/index.html)

**Esta página foi traduzida automaticamente.**

Conteúdo

- 

  [Implante Microsserviços em um Cluster do Kubernetes](https://docs.oracle.com/pt-br/solutions/deploy-microservices/index.html#GUID-3BB86E87-11C6-4DF1-8CA9-1FD385A9B9E9)

  - [Arquitetura](https://docs.oracle.com/pt-br/solutions/deploy-microservices/index.html#GUID-4D4895AA-6BBF-4FDA-BC34-61F5B1B10B35)
  - [Recomendações](https://docs.oracle.com/pt-br/solutions/deploy-microservices/index.html#GUID-C67233A3-D8FF-409F-B544-FA3AADEA43D3)
  - [Considerações](https://docs.oracle.com/pt-br/solutions/deploy-microservices/index.html#GUID-E018114A-EF18-43BB-AC08-C7474AB8BD10)
  - [Implantar](https://docs.oracle.com/pt-br/solutions/deploy-microservices/index.html#GUID-1F41C913-13C5-4AA7-B6D5-10763BF08C37)
  - [Explorar Mais](https://docs.oracle.com/pt-br/solutions/deploy-microservices/index.html#GUID-76B3D78C-F9CD-43E2-AA1B-2713248C6063)
  - [Alterar Log](https://docs.oracle.com/pt-br/solutions/deploy-microservices/index.html#GUID-411BFA15-EBF7-4011-B855-48DB6258B0CD)



Em uma arquitetura de microsserviços, cada microsserviço executa uma tarefa simples e se comunica com clientes ou outros microsserviços usando mecanismos leves, como solicitações de API REST. Você pode codificar cada microsserviço usando uma linguagem de programação mais adequada para a tarefa que ele executa. Aplicativos baseados em microsserviços são mais fáceis de implantar e manter.



### Arquitetura

Esta arquitetura de referência mostra microsserviços Python Flask e Redis implantados como contêineres Docker em um cluster Kubernetes no Oracle Cloud Infrastructure. Os contêineres extraem imagens do Docker do Oracle Cloud Infrastructure Registry.

O diagrama a seguir ilustra essa arquitetura de referência.

![A descrição de microsserviços - oci.png é mostrada a seguir](https://docs.oracle.com/pt-br/solutions/deploy-microservices/img/microservices-oci.png)
[Descrição da ilustração microservices-oci.png](https://docs.oracle.com/pt-br/solutions/deploy-microservices/img_text/microservices-oci.html)



Download diagram



Diagram downloading



A arquitetura tem os seguintes componentes:

- Região

  Uma região do Oracle Cloud Infrastructure é uma área geográfica localizada que contém um ou mais data centers, denominados domínios de disponibilidade. As regiões são independentes de outras regiões, e grandes distâncias podem separá-las (entre países ou mesmo continentes).

- Domínios de disponibilidade

  Os domínios de disponibilidade são data centers independentes e independentes dentro de uma região. Os recursos físicos de cada domínio de disponibilidade são isolados dos recursos de outros domínios de disponibilidade, o que fornece tolerância a falhas. Os domínios de disponibilidade não compartilham infraestrutura como energia ou resfriamento ou a rede interna de domínios de disponibilidade. Portanto, uma falha em um domínio de disponibilidade provavelmente não afetará os outros domínios de disponibilidade na região.

- Domínios de falha

  Domínio de falha é um agrupamento de hardware e infraestrutura dentro de um domínio de disponibilidade. Cada domínio de disponibilidade tem três domínios de falha com energia e hardware independentes. Quando você distribui recursos entre vários domínios de falha, seus aplicativos podem tolerar falhas físicas do servidor, manutenção do sistema e falhas de alimentação dentro de um domínio de falha.

- Rede virtual na nuvem (VCN) e sub-rede

  Uma VCN é uma rede personalizável definida por software que você configura em uma região do Oracle Cloud Infrastructure. Como as redes de data center tradicionais, as VCNs permitem controle total sobre seu ambiente de rede. Uma VCN pode ter vários blocos CIDR não sobrepostos que você poderá alterar após criar a VCN. Você pode segmentar uma VCN em sub-redes, que podem ter como escopo uma região ou um domínio de disponibilidade. Cada sub-rede consiste em um intervalo contíguo de endereços que não são sobrepostos com as outras sub-redes da VCN. Você pode alterar o tamanho de uma sub-rede após a criação. Uma sub-rede pode ser pública ou privada.

- Container Engine for Kubernetes

  O Oracle Cloud Infrastructure Container Engine for Kubernetes é um serviço totalmente gerenciado, escalável e altamente disponível que você pode usar para implantar seus aplicativos de contêineres na nuvem. Você especifica os recursos de computação necessários para os seus aplicativos, e o Container Engine for Kubernetes os provisiona no Oracle Cloud Infrastructure em uma tenancy existente. O Serviço Container Engine for Kubernetes usa o Kubernetes para automatizar a implantação, o dimensionamento e o gerenciamento de aplicativos de contêineres entre clusters de hosts.

- Registro

  O Oracle Cloud Infrastructure Registry é um registro gerenciado pela Oracle que permite simplificar seu workflow de desenvolvimento para produção. O registro facilita o armazenamento, o compartilhamento e o gerenciamento de artefatos de desenvolvimento, como imagens do Docker. A arquitetura altamente disponível e escalável do Oracle Cloud Infrastructure garante que você possa implantar e gerenciar seus aplicativos de forma confiável.



### Recomendações

Os requisitos podem diferir da arquitetura descrita aqui. Use as recomendações a seguir como ponto de partida.

- VCN

  Quando você cria uma VCN, determine o número de blocos CIDR necessários e o tamanho de cada bloco com base no número de recursos que você planeja anexar a sub-redes na VCN. Use blocos CIDR que estejam dentro do espaço de endereço IP privado padrão.

  Selecione blocos CIDR que não se sobreponham a nenhuma outra rede (no Oracle Cloud Infrastructure, seu data center local ou outro provedor de nuvem) para a qual você pretende configurar conexões privadas.

  Depois de criar uma VCN, você poderá alterar, adicionar e remover seus blocos CIDR.

  Ao projetar as sub-redes, considere seu fluxo de tráfego e os requisitos de segurança. Anexe todos os recursos dentro de uma camada ou função específica à mesma sub-rede, que pode servir como limite de segurança.

  Use sub-redes regionais.

  Para simplificar, essa arquitetura usa uma sub-rede pública para hospedar o Container Engine for Kubernetes. Você também pode usar uma sub-rede privada. Nesse caso, use um gateway NAT para permitir o acesso à internet pública do cluster.

- Container Engine for Kubernetes

  Nesta arquitetura, os nós de trabalho usam a forma VM.Standard2.1 e são executados no Oracle Linux. Dois nós de trabalho são usados para hospedar dois microsserviços diferentes, mas você pode criar até 1000 nós em cada cluster.

- Registro

  Usamos o Oracle Cloud Infrastructure Registry como um registro privado do Docker para uso interno, enviando imagens do Docker e extraindo-as do Registro. Você também pode usá-lo como registro público do Docker, permitindo que qualquer usuário com acesso à Internet e o URL apropriado extraia imagens de repositórios públicos no registro.



### Considerações

- Escalabilidade

  Você pode expandir seu aplicativo atualizando o número de nós de trabalho no cluster do Kubernetes, dependendo da carga. Da mesma forma, é possível reduzir o número de nós de trabalho no cluster. Ao criar um serviço no cluster do Kubernetes, você pode criar um balanceador de carga para distribuir o tráfego de serviço entre os nós designados para esse serviço.

- Disponibilidade do aplicativo

  Os domínios de falha fornecem a melhor resiliência em um único domínio de disponibilidade. Você também pode implantar instâncias ou nós que executam as mesmas tarefas em vários domínios de disponibilidade. Este design remove um único ponto de falha introduzindo redundância.

- Capacidade

  Essa arquitetura usa dois microsserviços. Um é um microsserviço Python Flask, um aplicativo web simples que executa operações CRUD. O outro microsserviço é um banco de dados na memória Redis. O microsserviço Python-Flask se comunica com o microsserviço Redis para recuperar os dados.

- Segurança

  Use políticas que restringem quem pode acessar os recursos do Oracle Cloud Infrastructure que sua empresa tem e como.

  O Container Engine for Kubernetes está integrado com o Oracle Cloud Infrastructure Identity and Access Management (IAM). O IAM fornece autenticação fácil com funcionalidade nativa de identidade do Oracle Cloud Infrastructure.



### Implantar

O código necessário para implantar um cluster do OKE está disponível em GitHub. Este código não implanta os microsserviços.

Você pode inserir o código no Oracle Cloud Infrastructure Resource Manager com um único clique, criar a pilha e implantá-lo. Como alternativa, faça download do código GitHub para seu computador, personalize o código e implante a arquitetura usando a CLI do Terraform.

- Implante usando o

   

  Oracle Cloud Infrastructure Resource Manager

  :

  1. Clique em

      

     ![Implantar no Oracle Cloud](https://docs.oracle.com/pt-br/solutions/deploy-microservices/img/deploy-oracle-cloud-button-png.png)

     Se você ainda não estiver conectado, informe a tenancy e as credenciais do usuário.

  2. Examinar e aceitar os termos e condições.

  3. Selecione a região na qual você deseja implantar a pilha.

  4. Siga os prompts na tela e as instruções para criar a pilha.

  5. Após criar a pilha, clique em **Ações do Terraform** e selecione **Planejar**.

  6. Aguarde a conclusão do job e revise o plano.

     Para fazer alterações, retorne à página Detalhes da Pilha, clique em **Editar Pilha** e faça as alterações necessárias. Em seguida, execute a ação **Plano** novamente.

  7. Se nenhuma alteração adicional for necessária, retorne à página Detalhes da Pilha, clique em **Ações do Terraform** e selecione **Aplicar**.

- Implante usando a CLI do Terraform:

  1. Vá para [GitHub](https://docs.oracle.com/pls/topic/lookup?ctx=en/solutions/deploy-microservices&id=github-oci-arch-microservice-oke).
  2. Faça download do código ou clone-o no computador local.
  3. Siga as instruções em `README.md`.



### Explorar Mais

Saiba mais sobre como implantar aplicativos em contêineres no Kubernetes na nuvem.

- [Visão Geral do Serviço Registry](https://docs.oracle.com/pls/topic/lookup?ctx=en/solutions/deploy-microservices&id=oci-registry)
- [Visão Geral do Serviço Container Engine for Kubernetes](https://docs.oracle.com/pls/topic/lookup?ctx=en/solutions/deploy-microservices&id=oci-oke)
- [Conheça a arquitetura de aplicativos baseados em microsserviços no Oracle Cloud](https://docs.oracle.com/pls/topic/lookup?ctx=en/solutions/deploy-microservices&id=MSASL)
- [Saiba mais sobre o design de uma topologia do Kubernetes para aplicativos de contêineres na nuvem](https://docs.oracle.com/pls/topic/lookup?ctx=en/solutions/deploy-microservices&id=LPEBF)
- [Implante um aplicativo baseado em microsserviços no Kubernetes conectado a um banco de dados autônomo](https://docs.oracle.com/pls/topic/lookup?ctx=en/solutions/deploy-microservices&id=RKKAV)
- [Implantar microsserviços com um banco de dados convergido e Helidon](https://docs.oracle.com/pls/topic/lookup?ctx=en/solutions/deploy-microservices&id=BKOCD)



### Alterar Log

Este log lista alterações significativas:

| 15 de setembro de 2022 | Esclareça se o Terraform implantável inclui um cluster do OKE. Ele não implanta os microsserviços. |
| ---------------------- | ------------------------------------------------------------ |
| 10 de novembro de 2021 | Atualizado a seção Implantar para implantar a última revisão da pilha no GitHub diretamente no Oracle Cloud Infrastructure Resource Manager com um único clique.Opção adicionada para fazer download de versões editáveis (.SVG e .DRAWIO) do diagrama de arquitetura. |
| 5 de janeiro de 2021   | Instruções adicionadas para implantar a arquitetura usando o Oracle Cloud Infrastructure Resource Manager. |
| 9 de setembro de 2020  | Seção Implantação adicionada.                                |

[Título e Informações de Copyright](https://docs.oracle.com/pt-br/solutions/deploy-microservices/index.html#copyright-information)

**Automação Disponível**

You can deploy this pattern using downloadable code or automated provisioning, as described in the Download or Deploy section.[Learn more](https://docs.oracle.com/pt-br/solutions/deploy-microservices/index.html#GUID-1F41C913-13C5-4AA7-B6D5-10763BF08C37)[ Implantar no Oracle Cloud](https://docs.oracle.com/pls/topic/lookup?ctx=en/solutions/deploy-microservices&id=github-oci-arch-microservice-oke-zip)[ Ir para Github](https://docs.oracle.com/pls/topic/lookup?ctx=en/solutions/deploy-microservices&id=github-oci-arch-microservice-oke)

Deploying on OCI requires an account.[Sign up for Free Tier](https://signup.cloud.oracle.com/?sourceType=:ow:o:p:feb:::RC_WWMK220311P00032:DocsRABanner&intcmp=:ow:o:p:feb:::RC_WWMK220311P00032:DocsRABanner) if you don't have an account.

- [© Oracle](https://docs.oracle.com/pls/topic/lookup?ctx=pt-br/legal&id=cpyr)
- [Sobre a Oracle](https://www.oracle.com/br/corporate/)
- [Fale Conosco](https://www.oracle.com/br/corporate/contact/)
- 
- [Products A-Z](https://docs.oracle.com/en/browseall.html)
- [Terms of Use & Privacy](https://www.oracle.com/br/legal/privacy/)
- Preferências de Cookies
- [Escolhas de Anúncios](https://www.oracle.com/br/legal/privacy/marketing-cloud-data-cloud-privacy-policy.html#12)
- [Feedback para Tradução Automática](https://ora-java.custhelp.com/ci/documents/detail/5/2523/12/6c815f312d7890bee99d231c883f3f3e3e40eafa?locale=PTB)