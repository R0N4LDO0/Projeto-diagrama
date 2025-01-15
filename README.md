# ☁️ _Projeto Final do PB da Compass UOL | AWS e DevSecOps_

   ![logo-compass](https://github.com/user-attachments/assets/37b0ded0-e990-4228-8295-b063c8197782) 





--- 
# 🔎 _CASE:_
Nós somos da empresa "Fast Engineering S/A" e gostaríamos de uma solução dos 
senhores(as), que fazem parte da empresa terceira "TI SOLUÇÕES INCRÍVEIS". 
Nosso eCommerce está crescendo e a solução atual não está atendendo mais a alta 
demanda de acessos e compras que estamos tendo. 
Atualmente usamos: 

• 01 servidor para Banco de Dados Mysql (500GB de dados, 10Gb de RAM, 3 Core 
CPU); 

• 01 servidor para a aplicação utilizando REACT – frontend (5GB de dados, 2Gb de 
RAM, 1 Core CPU); 

• 01 servidor de backend com 3 APIs, com o Nginx servindo de balanceador de 
carga e que armazena estáticos como fotos e links. (5GB de dados, 4Gb de RAM, 
2 Core CPU);

Queremos modernizar esse sistema para a AWS, precisamos seguir as melhores 
práticas arquitetura em Cloud AWS, a nova arquitetura deve seguir as seguintes 
diretrizes: 

• Ambiente Kubernetes;  

• Banco de dados gerenciado (PaaS e Multi AZ); 
• Backup de dados; 

• Sistema para persistência de objetos (imagens, vídeos etc.); 
• Segurança; 

Porém antes da migração acontecer para a nova estrutura, precisamos fazer uma 
migração “lift-and-shift” ou “as-is”, o mais rápido possível, só depois que iremos 
promover a modificação para a nova estrutura em Kubernetes.

![arq antiga](https://github.com/user-attachments/assets/d113665e-bb31-459a-ac4f-e1729b459298)

*Arquitetura atual da Fast Engineering*

---
# _ETAPA 1 : Migração (Lift and Shift) ou “As-Is”_

Nesta primeira fase, a meta é migrar rapidamente os servidores on-premises para a AWS, sem modificar a arquitetura ou refatorar código. No diagrama abaixo, ilustramos o fluxo de migração: o AWS Replication Agent envia os dados on-premises para uma VPC Temporária (Staging), enquanto o AWS MGN converte as VMs em instâncias EC2. Paralelamente, o AWS DMS cuida da migração do banco de dados para Amazon RDS, garantindo integridade de dados.

# _Abordagem Geral_

 * AWS MGN (Application Migration Service): Replica os servidores (frontend, backend) para instâncias EC2 na AWS (Lift-and-Shift).

 * AWS Replication  Agent: Agente responsável por enviar dados/arquivos do ambiente on-premises para a VPC de staging, onde o Replication Server processa e armazena em EBS temporariamente.

 * AWS EBS  (Elastic Block Store): Volumes que armazenam dados persistentes (tanto no staging quanto nas instâncias EC2 finais).

 * AWS DMS (Database Migration Service): Migra o banco MySQL on-premises para Amazon RDS (MySQL).

 * Load Balancer (opcional): Distribui as requisições entre o Frontend (EC2) e o Backend (EC2), além de prover entrada segura (HTTP/HTTPS).

# _Proceditemento de  Migração_ 

### Preparação 

 1. Registro

* Catalogar os serviços rodando on-premises:
* Frontend (React)
* Backend (Nginx + APIs)
* Banco MySQL
* Versões de SO, bibliotecas, portas de rede (TCP 443, TCP 1500, TCP 3306 etc.).
* Janela de Manutenção e Riscos
* Determinar quanto tempo de parada é tolerável.
* Planejar rollback (em caso de falha).

 2. Provisionamento de Infraestrutura AWS
* Criar VPC de Staging (Temporária)
* Subnet pública para receber o Replication Server.
* Replication Server irá receber dados on-prem (via AWS Replication Agent, porta TCP 1500).
* Criar VPC Final
* Subnets Públicas: onde ficará o Frontend EC2 e Load Balancer.
* Subnets Privadas: onde ficará o Backend EC2 e o RDS.
* Internet Gateway (IGW) para as subnets públicas, NAT Gateway se as instâncias privadas precisarem acessar a internet.
* Security Groups
* Restringir porta 3306 (MySQL) para ser acessível somente pelo Backend.
* Permitir portas 80/443 (HTTP/HTTPS) vindas do Load Balancer para o Backend/Frontend.

 3. Migração do Banco de Dados
* Provisionar Amazon RDS (MySQL)
* Selecione a versão do MySQL compatível.
* Para um lift and shift TOTAL, será utilizado um RDS single-AZ
* Configurar AWS DMS
* Criar um endpoint de origem (MySQL on-premises) e um endpoint de destino (RDS MySQL).
* Migrar esquemas, tabelas e dados (full load + CDC, replicação contínua).
* Testar
* Validar performance e integridade do banco no RDS.

4. Migração de Frontend e Backend
* Instalar AWS Replication Agent on-premises
* Configurar para enviar dados/VMs para o Replication Server na VPC Staging.
* AWS MGN
* Monitora e converte os dados armazenados no Replication Server em AMIs.
* Gera instâncias EC2 correspondentes (Frontend e Backend) na VPC Final.
* Volumes EBS
* Os dados de cada servidor migrado ficam em EBS associados às instâncias EC2 resultantes.

 5. Teste e Validação
* DNS Temporário
* Aponte um subdomínio (ex.: wwww.fastengineering.com.br`) para o Load Balancer ou para o IP público do EC2 Frontend (caso não use LB).
* Verifique se o backend e o RDS estão respondendo corretamente
* Checar Logs e Performance
* Monitorar a saúde das instâncias EC2 (CPU, memória) e do RDS (latência, conexões).
* Verificar se APIs, frontend e DB estão funcionando sem erros.
 --- 
# Considerações finais
 
  Nesta etapa de Lift and Shift, os servidores (Frontend/Backend) e o banco de dados MySQL foram migrados para AWS EC2 e Amazon RDS respectivamente, sem grandes alterações na aplicação. O diagrama ilustra a transição via Replication Server (VPC Staging) e o AWS MGN para as instâncias EC2 finais, enquanto o AWS DMS garante a transferência segura dos dados do banco para o RDS.

# _Diagrama “Lift-and-Shift"_ 

![Diagrama-1](https://github.com/user-attachments/assets/32f16ab1-09f4-4b03-a856-4d7c05d4727b)


  --- 

  A arquitetura da nova solução busca resolver os desafios atuais da Fast Engineering, oferecendo alta disponibilidade e escalabilidade através de serviços AWS. Essa solução garantirá o acompanhamento de crescimento contínuo do eCommerce, seguindo as melhores práticas DevOps. 

![Diagrama 2](https://github.com/user-attachments/assets/ab23d03d-9b14-4f77-b94c-268383f610a1) 


  A nova solução de arquitetura também possui alinhamento com os pilares da AWS Well-Architected Framework:

* Excelência Operacional
* Segurança
* Confiabilidade
* Eficiência de Performance
* Otimização de Custos
* Sustentabilidade

  
![pilares](https://github.com/user-attachments/assets/ac9779de-e250-430a-9af9-3384ab20d5b9)

 
  # 🧰 Serviços e Recursos Usados na  nova Arquitetura

  * **Amazon CloudFront** :  
  É um serviço de entrega de conteúdo usado para distribuir conteúdo estático, como imagens e arquivos, de forma eficiente e rápida.
    *Desafio*: Alto crescimento de acessos e compras. 
    *Solução*: Distribuição global, eficiente e veloz de conteúdo estático

  * **AWS WAF** :
  O AWS Web Application Firewall é um serviço de firewall que ajuda a proteger aplicações web contra ataques comuns.
    *Desafio*: Proteger o eCommerce.
    *Solução*: Proteção avançada contra ameaças, garantindo a segurança da aplicação web.

  * **Amazon Route 53** :
  Serviço DNS para registro e gerenciamento de domínios, com roteamento de tráfego para recursos AWS (ELB, CloudFront).
    *Desafio*: Disponibilidade e resiliência insuficientes. 
    *Solução*: Roteamento de tráfego eficiente e alta disponibilidade, direcionando acessos para instâncias e serviços AWS.

  * **Amazon S3** :
  Uso de Buckets do S3 para armazenar e distribuir conteúdo estático, como imagens, vídeos e arquivos. Integrado ao CloudFront faz uma entrega muito eficiente de conteúdo.
    *Desafio*: Armazenamento escalável para conteúdo estático. 
    *Solução*: Armazenamento seguro e escalável de imagens, vídeos e arquivos.

  * **AWS IAM** :
  O AWS Identity and Access Management é fundamental para garantir a segurança e o controle de acesso aos recursos da AWS.
    *Desafio*: Controle de acesso a recursos da AWS.
    *Solução*: Gerenciamento granular de permissões, garantindo apenas acesso autorizado a recursos críticos.

  * **Amazon EKS** :
  O Elastic Kubernetes Service será a base da arquitetura, fornecendo a orquestração de contêineres usando Kubernetes. 
    *Desafio*: Infraestrutura limitada para suportar crescimento.
    *Solução*: Orquestração de contêineres escalável, permitindo expansão dinâmica e alta disponibilidade da aplicação.

  * **VPC** :
  A Virtual Private Cloud isolará a infraestrutura na nuvem e fornecerá controle granular sobre a rede, com a criação de uma rede privada virtual, melhorará a segurança e o isolamento.
    *Desafio*: Isolamento e segurança de rede.
    *Solução*: Criação de uma rede VPC, trazendo controle de tráfego e segurança aprimorados.

  * **Amazon RDS** :
  O Relational Database Service será usado para hospedar o banco de dados MySQL, garantindo alta disponibilidade, escalabilidade e backup automático dos dados.
    *Desafio*: Desempenho e escalabilidade insuficientes do banco de dados. 
    *Solução*: BD gerenciado com alta disponibilidade, escalabilidade e backup automático.

  * **Elastic Load Balancing** :
  O ELB distribui o tráfego entre instâncias do EKS, garantindo um balanceamento de carga eficiente e uma alta disponibilidade da aplicação.
    *Desafio*: Distribuição desigual de tráfego e baixa disponibilidade.
    *Solução*: Balanceamento de carga entre instâncias, melhorando a disponibilidade e eficiência do eCommerce.

  * **mazon CloudWatch** :
  O CloudWatch traz monitoramento e observabilidade para recursos da aplicação. Usa métricas e alarmes para monitorar o desempenho dos componentes.
    *Desafio*: Falta de visibilidade e monitoramento dos componentes.
    *Solução*: Monitoramento detalhado e alertas em tempo real para identificar e resolver problemas rapidamente.

  * **AWS DMS** :
  O AWS Database Migration Service será usado para migrar dados do banco de dados MySQL existente para o RDS.
    *Desafio*: Necessidade de migrar dados do banco de dados existente. 
    *Solução*: Migração suave e consistente dos dados para o RDS, minimizando o tempo de inatividade.

  * **AWS CloudFormation** :
  O Amazon CloudFormation permite criar e gerenciar sua infraestrutura como código. Use-o para definir e provisionar recursos de maneira automatizada e rastreável.
    *Desafio*: Gerenciamento manual e complexo da infraestrutura.
    *Solução*: Automação e rastreabilidade na criação e atualização de recursos de infraestrutura.

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

 
![DMS-migracao](https://github.com/user-attachments/assets/e43f226f-db52-4239-83ac-d3693e247930)     

_Processo do AWS DMS_

--- 

# 💰 Valores 

## _Estimativa de custo Lift-and-Shift ETAPA 1_

* Custo mensal: 1.583,75 USD
  
* Custo total de 12 months : 19.005,00 USD
  
![calculadora 1](https://github.com/user-attachments/assets/110f1060-44ee-48fd-b536-270e6d3a4d76)






## _Estimativa de custo da nova arquitetura  ETAPA 2_

* Custo mensal: 1.854,84 USD

* Custo total de 12 meses: 22.258,08 USD
  
  ![estimativa-arq (1)](https://github.com/user-attachments/assets/710dc894-d6c2-4161-9754-df183548324c)

  ---

  ## 📑 Referências:

*  https://aws.amazon.com/pt/dms/

* https://aws.amazon.com/pt/architecture/well-architected/?wa-lens-whitepapers.sort-by=item.additionalFields.sortDate&wa-lens-whitepapers.sort-order=desc&wa-guidance-whitepapers.sort-

* https://aws.amazon.com/pt/architecture/well-architected/?wa-lens-whitepapers.sort-by=item.additionalFields.sortDate&wa-lens-whitepapers.sort-order=desc&wa-guidance-whitepapers.sort-by=item.additionalFields.sortDate&wa-guidance-whitepapers.sort-order=desc

* https://docs.aws.amazon.com/pt_br/s3/index.html?nc2=h_ql_doc_s3
  
* https://docs.aws.amazon.com/pt_br/waf/index.html

* https://aws.amazon.com/pt/eks/

  
