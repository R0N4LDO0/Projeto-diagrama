# ☁️ Projeto Final do PB da Compass UOL | AWS e DevSecOps 

   ![logo-compass](https://github.com/user-attachments/assets/37b0ded0-e990-4228-8295-b063c8197782) 





--- 
# 🔎 CASE:
"Nós somos da empresa "Fast Engineering S/A" e gostaríamos de uma solução dos senhores(as), que fazem parte da empresa terceira "TI SOLUÇÕES INCRÍVEIS". Nosso eCommerce está crescendo e a solução atual não está atendendo mais a alta demanda de acessos e compras que estamos tendo. Desde o início do ano os acessos e compras estão crescendo 20% a cada mês.

Atualmente usamos:
* 01 servidor para Banco de Dados Mysql;
* 01 servidor para a aplicação utilizando REACT;
* 01 servidor de web Server e que armazena estáticos como fotos e links.

Nosso pedido é um ORÇAMENTO com:

- ESCOPO;
- ARQUITETURA DA NOVA SOLUÇÃO;
- VALORES;
  

Sobre a construção da arquitetura para o futuro website da nossa empresa, precisamos seguir as melhores práticas DevOps." 

O que é requerido na atividade:

+ Ambiente Kubernetes;
+ Banco de dados PaaS;
+ MultiAZ;
+ Segurança de backup de dados;
+ Persistência dos dados;
+ Balanceamento de carga com healthcheck;
+ Segurança (liberar somente o necessário/mínimo acesso possível).

_Objetivo: Monte a proposta e a arquitetura do que o grupo propõe entregar._

##

A arquitetura atual da Fast Engineering não está acompanhando o aumento de acessos e compras no eCommerce devido a um crescimento mensal de 20%.


![arq antiga](https://github.com/user-attachments/assets/d113665e-bb31-459a-ac4f-e1729b459298)

*Arquitetura atual da Fast Engineering*

##

A arquitetura da nova solução busca resolver os desafios atuais da Fast Engineering, oferecendo alta disponibilidade e escalabilidade através de serviços AWS. Essa solução garantirá o acompanhamento de crescimento contínuo do eCommerce, seguindo as melhores práticas DevOps.

![Gravar_2025_01_14_18_14_05_353](https://github.com/user-attachments/assets/d5455651-1175-47ba-a59b-907fc57acae4)

  *Arquitetura nova* 
  ## 
# 🔧 Implementação
#### Integração Contínua e Implantação Contínua (CI/CD)

  Na fase de implementação da arquitetura na AWS, utilizaremos uma abordagem de Integração Contínua e Implantação Contínua (CI/CD) para otimizar o desenvolvimento e a entrega. 

  Isso é essencial para a adoção das práticas DevOps, que visam a colaboração e a automação entre equipes de desenvolvimento e operações.

  Nessa abordagem, integramos os seguintes serviços da AWS, que desempenham papéis fundamentais em automatizar o ciclo de vida de desenvolvimento, construção e implantação de aplicações:

  + AWS CodeCommit
  + AWS CodeBuild
  + AWS CodeDeploy
  + AWS CodePipeline

  **AWS CodeCommit**: É um serviço de hospedagem de repositórios de controle de versão privados. Ele fornece um ambiente seguro e escalável para armazenar e gerenciar código-fonte. Com recursos de controle de acesso baseados em IAM, permite colaborações de maneira eficiente, controle de versões e rastreio de alterações ao longo do tempo.

  **AWS CodeBuild**: É um serviço de compilação gerenciada que automatiza a construção, teste e geração de artefatos de código-fonte. Ele oferece ambientes de compilação sob demanda e escaláveis, permitindo criação e testes do código em paralelo. Tem suporte a vários ambientes de execução, podendo criar e empacotar aplicações para várias plataformas e arquiteturas.

  **AWS CodeDeploy**: Serviço que automatiza a implantação de aplicativos em ambientes de teste e produção de forma consistente e controlada. Ele suporta implantações em instâncias EC2, serviços ECS e até mesmo ambientes on-premises. Com a automação de implantação, permite reduzir erros manuais e garante uma implantação uniforme em ambientes diferentes.

  **AWS CodePipeline**: É um serviço de automação de CI/CD que cria fluxos automatizados para desenvolver, testar, implantar e entregar aplicações. Ele reage automaticamente a mudanças de código no repositório CodeCommit, permitindo entregas frequentes e confiáveis. Isso otimiza o processo de desenvolvimento e implantação.

#### Migração do Banco de Dados

Para fazer uma boa migração do banco de dados MySQL para o AWS RDS, usaremos o AWS Database Migration Service (DMS) para garantir uma transição suave e eficiente. 

O DMS nos permite replicar os dados para a AWS de maneira contínua, garantindo a integridade e a consistência dos dados. 

Com essa abordagem, podemos realizar a migração com o mínimo de impacto para as operações e, ao mesmo tempo, colher os benefícios da escalabilidade, confiabilidade e segurança que a nuvem AWS oferece.





  
  A nova solução de arquitetura também possui alinhamento com os pilares da AWS Well-Architected Framework:

* Excelência Operacional
* Segurança
* Confiabilidade
* Eficiência de Performance
* Otimização de Custos
* Sustentabilidade

  
![aws sh](https://github.com/user-attachments/assets/cf07e45d-5f83-42dd-b208-2e97be270be7)

*Pilares da AWS Well-Architected Framework*
 
  # 🧰 Serviços e Recursos Usados na Arquitetura

  * Amazon CloudFront**: 
  É um serviço de entrega de conteúdo usado para distribuir conteúdo estático, como imagens e arquivos, de forma eficiente e rápida.
    *Desafio*: Alto crescimento de acessos e compras. 
    *Solução*: Distribuição global, eficiente e veloz de conteúdo estático

  * AWS WAF**:
  O AWS Web Application Firewall é um serviço de firewall que ajuda a proteger aplicações web contra ataques comuns.
    *Desafio*: Proteger o eCommerce.
    *Solução*: Proteção avançada contra ameaças, garantindo a segurança da aplicação web.

  * Amazon Route 53**:
  Serviço DNS para registro e gerenciamento de domínios, com roteamento de tráfego para recursos AWS (ELB, CloudFront).
    *Desafio*: Disponibilidade e resiliência insuficientes. 
    *Solução*: Roteamento de tráfego eficiente e alta disponibilidade, direcionando acessos para instâncias e serviços AWS.

  * Amazon S3**:
  Uso de Buckets do S3 para armazenar e distribuir conteúdo estático, como imagens, vídeos e arquivos. Integrado ao CloudFront faz uma entrega muito eficiente de conteúdo.
    *Desafio*: Armazenamento escalável para conteúdo estático. 
    *Solução*: Armazenamento seguro e escalável de imagens, vídeos e arquivos.

  * AWS IAM**:
  O AWS Identity and Access Management é fundamental para garantir a segurança e o controle de acesso aos recursos da AWS.
    *Desafio*: Controle de acesso a recursos da AWS.
    *Solução*: Gerenciamento granular de permissões, garantindo apenas acesso autorizado a recursos críticos.

  * Amazon EKS**:
  O Elastic Kubernetes Service será a base da arquitetura, fornecendo a orquestração de contêineres usando Kubernetes. 
    *Desafio*: Infraestrutura limitada para suportar crescimento.
    *Solução*: Orquestração de contêineres escalável, permitindo expansão dinâmica e alta disponibilidade da aplicação.

  * VPC**:
  A Virtual Private Cloud isolará a infraestrutura na nuvem e fornecerá controle granular sobre a rede, com a criação de uma rede privada virtual, melhorará a segurança e o isolamento.
    *Desafio*: Isolamento e segurança de rede.
    *Solução*: Criação de uma rede VPC, trazendo controle de tráfego e segurança aprimorados.

  * Amazon RDS**:
  O Relational Database Service será usado para hospedar o banco de dados MySQL, garantindo alta disponibilidade, escalabilidade e backup automático dos dados.
    *Desafio*: Desempenho e escalabilidade insuficientes do banco de dados. 
    *Solução*: BD gerenciado com alta disponibilidade, escalabilidade e backup automático.

  * Elastic Load Balancing**:
  O ELB distribui o tráfego entre instâncias do EKS, garantindo um balanceamento de carga eficiente e uma alta disponibilidade da aplicação.
    *Desafio*: Distribuição desigual de tráfego e baixa disponibilidade.
    *Solução*: Balanceamento de carga entre instâncias, melhorando a disponibilidade e eficiência do eCommerce.

  * mazon CloudWatch**:
  O CloudWatch traz monitoramento e observabilidade para recursos da aplicação. Usa métricas e alarmes para monitorar o desempenho dos componentes.
    *Desafio*: Falta de visibilidade e monitoramento dos componentes.
    *Solução*: Monitoramento detalhado e alertas em tempo real para identificar e resolver problemas rapidamente.

  * **AWS DMS**:
  O AWS Database Migration Service será usado para migrar dados do banco de dados MySQL existente para o RDS.
    *Desafio*: Necessidade de migrar dados do banco de dados existente. 
    *Solução*: Migração suave e consistente dos dados para o RDS, minimizando o tempo de inatividade.

  * **AWS CloudFormation**:
  O Amazon CloudFormation permite criar e gerenciar sua infraestrutura como código. Use-o para definir e provisionar recursos de maneira automatizada e rastreável.
    *Desafio*: Gerenciamento manual e complexo da infraestrutura.
    *Solução*: Automação e rastreabilidade na criação e atualização de recursos de infraestrutura.
