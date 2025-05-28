# Diagramas de Infraestrutura

Este documento apresenta os diagramas de infraestrutura do sistema de gestão para a Assessoria de Inclusão do Centro Paula Souza, detalhando a arquitetura física, os recursos de nuvem, redes, segurança, monitoramento e práticas de DevOps adotadas para garantir disponibilidade, escalabilidade, segurança e facilidade de manutenção.

---

## 1. Visão Geral da Infraestrutura

A solução é baseada em arquitetura cloud-first, utilizando serviços gerenciados para garantir alta disponibilidade, escalabilidade e segurança. O ambiente contempla ambientes de desenvolvimento, homologação e produção, com separação lógica e controles de acesso adequados.

- **Provedor de Nuvem:** AWS (alternativamente Azure ou GCP, conforme política institucional)
- **Ambientes:**
  - Desenvolvimento (DEV)
  - Homologação (HML)
  - Produção (PRD)
- **Principais recursos:**
  - Servidores de aplicação (containers)
  - Banco de dados relacional gerenciado
  - Storage para arquivos
  - Serviços de autenticação (LDAP/AD ou SSO institucional)
  - Balanceador de carga
  - CDN para conteúdo estático
  - Monitoramento e logging centralizados

---

## 2. Diagrama de Infraestrutura Lógica

```mermaid
graph TD
  subgraph Usuários
    U1[Usuário Interno]
    U2[Usuário Externo]
  end
  U1 & U2 --> FW[Firewall / WAF]
  FW --> LB[Load Balancer]
  LB --> APP[Cluster de Aplicação (ECS/EKS/Azure AKS)]
  APP --> DB[(Banco de Dados Gerenciado)]
  APP --> FS[Storage de Arquivos]
  APP --> AUTH[Serviço de Autenticação]
  APP --> LOG[Monitoramento & Logging]
  LB --> CDN[CDN]
  CDN --> FS
```

**Descrição:**
- O acesso dos usuários é protegido por firewall/WAF.
- O balanceador de carga distribui requisições para o cluster de aplicação (containers).
- O cluster acessa banco de dados, storage, autenticação e serviços de monitoramento.
- Conteúdo estático é servido via CDN.

---

## 3. Diagrama de Rede e Segurança

```mermaid
graph TD
  subgraph VPC (Nuvem)
    IGW[Internet Gateway]
    FW[Firewall/WAF]
    LB[Load Balancer]
    subgraph Sub-rede Pública
      LB
      FW
    end
    subgraph Sub-rede Privada
      APP[Servidores de Aplicação]
      DB[(Banco de Dados)]
      FS[Storage]
      LOG[Monitoramento]
    end
    IGW --> FW
    FW --> LB
    LB --> APP
    APP --> DB
    APP --> FS
    APP --> LOG
  end
```

**Práticas de Segurança:**
- Sub-redes privadas para recursos sensíveis (DB, storage, aplicação).
- Acesso externo restrito ao load balancer e firewall.
- Comunicação interna via VPC.
- Criptografia em trânsito (TLS) e em repouso.
- Políticas de IAM e segregação de funções.

---

## 4. Diagrama de Pipeline DevOps (CI/CD)

```mermaid
flowchart LR
  DEV[Desenvolvedor] --> PR[Pull Request]
  PR --> CI[Pipeline CI (Build/Test)]
  CI --> CD[Pipeline CD (Deploy)]
  CD --> DEV_ENV[Ambiente DEV]
  CD --> HML_ENV[Ambiente HML]
  CD --> PRD_ENV[Ambiente PRD]
  CI --> QA[Validação Automática]
  QA --> CD
```

**Descrição:**
- O pipeline CI/CD automatiza build, testes, deploy e validações.
- Deploys são realizados de forma progressiva (DEV → HML → PRD).
- Integração com ferramentas de versionamento, testes automatizados e monitoramento.

---

## 5. Monitoramento, Backup e Recuperação

- **Monitoramento:**
  - Logs centralizados (CloudWatch, ELK, Azure Monitor)
  - Alertas de disponibilidade, performance e segurança
  - Dashboards customizados
- **Backup:**
  - Backups automáticos do banco de dados
  - Snapshots periódicos do storage
  - Testes regulares de restauração
- **Recuperação de Desastres:**
  - Procedimentos documentados
  - Replicação entre zonas de disponibilidade
  - RTO/RPO definidos conforme criticidade

---

## 6. Considerações de Escalabilidade e Custos

- Uso de auto scaling para aplicação e banco de dados
- Otimização de custos com instâncias sob demanda e reservadas
- Políticas de retenção de logs e backups
- Monitoramento de consumo e alertas de budget

---

## 7. Referências e Boas Práticas

- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [Azure Architecture Center](https://learn.microsoft.com/azure/architecture/)
- [Google Cloud Architecture Framework](https://cloud.google.com/architecture/framework)
- [DevOps - CI/CD Best Practices](https://martinfowler.com/bliki/ContinuousDelivery.html)

---

**Observação:**
Os diagramas apresentados são referenciais e devem ser adaptados conforme a infraestrutura real do Centro Paula Souza e políticas institucionais de TI.
