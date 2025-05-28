# Diagrama de Implantação
## Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza

## Sumário
- [Introdução](#introdução)
- [Notação UML Utilizada](#notação-uml-utilizada)
- [Visão Geral da Arquitetura de Implantação](#visão-geral-da-arquitetura-de-implantação)
- [Diagrama de Implantação](#diagrama-de-implantação-1)
- [Descrição dos Nós](#descrição-dos-nós)
  - [Nós de Cliente](#nós-de-cliente)
  - [Nós de Servidor de Aplicação](#nós-de-servidor-de-aplicação)
  - [Nós de Persistência](#nós-de-persistência)
  - [Nós de Serviços Externos](#nós-de-serviços-externos)
- [Ambientes do Sistema](#ambientes-do-sistema)
  - [Ambiente de Desenvolvimento](#ambiente-de-desenvolvimento)
  - [Ambiente de Homologação](#ambiente-de-homologação)
  - [Ambiente de Produção](#ambiente-de-produção)
- [Considerações de Segurança](#considerações-de-segurança)
- [Considerações de Escalabilidade](#considerações-de-escalabilidade)
- [Requisitos de Hardware e Software](#requisitos-de-hardware-e-software)
- [Estratégias de Backup e Recuperação](#estratégias-de-backup-e-recuperação)
- [Recomendações e Boas Práticas](#recomendações-e-boas-práticas)

## Introdução

Este documento apresenta o diagrama de implantação para o Sistema de Gestão da Assessoria de Inclusão do Centro Paula Souza. O diagrama de implantação é uma representação da arquitetura física do sistema, mostrando como os componentes de software são distribuídos entre os diferentes nós de hardware e como esses nós se comunicam entre si.

Enquanto os diagramas de componentes e pacotes descrevem a estrutura lógica do software, o diagrama de implantação se concentra nos aspectos físicos da infraestrutura que hospedará o sistema. Este diagrama é particularmente útil para administradores de sistemas, equipes de operações e desenvolvedores responsáveis pela implantação e manutenção do sistema em ambientes reais.

O diagrama apresentado reflete as decisões de arquitetura que visam garantir que o sistema atenda aos requisitos não funcionais identificados, como disponibilidade, escalabilidade, segurança e desempenho.

## Notação UML Utilizada

O diagrama de implantação segue a notação UML (Unified Modeling Language) padrão, incluindo os seguintes elementos:

- **Nós**: Representados por cubos tridimensionais, indicam elementos físicos ou virtuais como servidores, dispositivos ou ambientes
- **Artefatos**: Representados por retângulos com um ícone de documento no canto superior direito, indicam componentes de software implantáveis
- **Conexões**: Representadas por linhas sólidas entre nós, indicam caminhos de comunicação
- **Protocolos**: Anotações nas linhas de conexão, indicam os protocolos de comunicação utilizados
- **Estereótipos**: Texto entre «» que especifica o tipo de nó ou artefato (ex: «device», «server», «database»)
- **Ambiente**: Agrupamentos de nós que formam um ambiente completo (desenvolvimento, homologação, produção)

## Visão Geral da Arquitetura de Implantação

O Sistema de Gestão para a Assessoria de Inclusão segue uma arquitetura distribuída multicamadas, hospedada em uma combinação de infraestrutura local do Centro Paula Souza e serviços em nuvem para funcionalidades específicas. A arquitetura de implantação é composta por:

1. **Camada de Cliente**: Navegadores web em computadores e dispositivos móveis dos usuários finais
2. **Camada de Aplicação**: Servidores que hospedam o backend da aplicação, organizados em clusters para alta disponibilidade
3. **Camada de Persistência**: Servidores de banco de dados e armazenamento de arquivos
4. **Serviços Externos**: Integrações com sistemas de email, autenticação institucional e outros serviços

Os ambientes de desenvolvimento, homologação e produção seguem a mesma estrutura lógica, mas com diferentes níveis de redundância, escalabilidade e isolamento, adequados ao propósito de cada ambiente.

## Diagrama de Implantação

```
+--------------------------------------------------------------------------------------------------------------+
|                                                                                                              |
|                                           <<device>>                                                         |
|                                    Estações de Trabalho / Notebooks                                          |
|   +------------------------------------+                                                                     |
|   |      <<execution environment>>     |                                                                     |
|   |          Web Browser               |                                                                     |
|   |                                    |                                                                     |
|   |   +---------------------------+    |                                                                     |
|   |   |       Interface Web       |    |                                                                     |
|   |   +---------------------------+    |                                                                     |
|   +------------------------------------+                                                                     |
|                                                                                                              |
+-----------+--------------------------------------------------------------------------------------------+-----+
            |                                                                                            |
            | HTTPS                                                                                      |
            |                                                                                            |
+-----------v--------------------------------------------------------------------------------------------v-----+
|                                                                                                              |
|                                          <<device>>                                                          |
|                                       Dispositivos Móveis                                                    |
|   +------------------------------------+                                                                     |
|   |      <<execution environment>>     |                                                                     |
|   |          Web Browser               |                                                                     |
|   |                                    |                                                                     |
|   |   +---------------------------+    |                                                                     |
|   |   |    Interface Responsiva   |    |                                                                     |
|   |   +---------------------------+    |                                                                     |
|   +------------------------------------+                                                                     |
|                                                                                                              |
+-------------------+--------------------------------------------------------------------------------+--------+
                    |                                                                                |
                    | HTTPS                                                                          |
                    |                                                                                |
+-------------------v--------------------------------------------------------------------------------v--------+
|                                                                                                             |
|                                       <<device>>                                                            |
|                                   Load Balancer / Firewall                                                  |
|   +-----------------------------------------------------------------------------------------------+        |
|   |                                                                                               |        |
|   |                             SSL Termination, Request Distribution                             |        |
|   |                                                                                               |        |
|   +-----------------------------------------------------------------------------------------------+        |
|                                                                                                             |
+-----+-------------------------------+------------------------------+-----------------------------------+-----+
      |                               |                              |                                   |
      | HTTP                          | HTTP                         | HTTP                              |
      |                               |                              |                                   |
+-----v---------------+   +-----------v--------------+   +-----------v--------------+                    |
|                     |   |                          |   |                          |                    |
|     <<server>>      |   |       <<server>>         |   |       <<server>>         |                    |
|  Application Server |   |   Application Server     |   |   Application Server     |                    |
|      (Node 1)       |   |        (Node 2)          |   |        (Node n)          |                    |
| +-----------------+ |   | +-----------------+      |   | +-----------------+      |                    |
| |<<artifact>>     | |   | |<<artifact>>     |      |   | |<<artifact>>     |      |                    |
| |Backend Services | |   | |Backend Services |      |   | |Backend Services |      |                    |
| +-----------------+ |   | +-----------------+      |   | +-----------------+      |                    |
|                     |   |                          |   |                          |                    |
+---------------------+   +--------------------------+   +--------------------------+                    |
                                                                                                        |
                                                                                                        |
+------------------------------------------------------------------+                                   |
|                                                                  |                                   |
|                         <<server>>                               |                                   |
|                     Redis Cache Server                           |                                   |
| +------------------------------------------------------+        |                                   |
| |                                                      |        |                                   |
| |                    Session Cache                     |<-------+                                   |
| |                    Response Cache                    |        |                                   |
| |                                                      |        |                                   |
| +------------------------------------------------------+        |                                   |
|                                                                  |                                   |
+------------------------------------------------------------------+                                   |
                                                                                                       |
                                                                                                       |
+------------------------------------------------------------------+         +----------------------+  |
|                                                                  |         |                      |  |
|                         <<server>>                               |         |     <<server>>       |  |
|                        Database Server                           |         |  File Storage Server |  |
| +------------------------------------------------------+        |         |                      |  |
| |                 <<database>>                          |        |         | +----------------+   |  |
| |                 PostgreSQL Cluster                    |        |         | |                |   |  |
| |                                                       |<---------------->| |  File System   |<----+
| | +------------------------------------+                |        |         | |  Storage       |   |
| | |               Master               |                |        |         | |                |   |
| | +------------------------------------+                |        |         | +----------------+   |
| |                   |                                   |        |         |                      |
| | +------------+----v-----------+------------+          |        |         +----------------------+
| | |            |                |            |          |        |
| | |  Slave 1   |    Slave 2     |   Slave n  |          |        |
| | |            |                |            |          |        |
| | +------------+----------------+------------+          |        |
| |                                                       |        |
| +-------------------------------------------------------+        |
|                                                                  |
+------------------------------------------------------------------+


+------------------------------------------------------------------+
|                                                                  |
|                      <<external system>>                         |
|                    Email Notification Service                    |
| +------------------------------------------------------+        |
| |                                                      |        |
| |                   SMTP Service                       |<-------+
| |                                                      |        |
| +------------------------------------------------------+        |
|                                                                  |
+------------------------------------------------------------------+

+------------------------------------------------------------------+
|                                                                  |
|                      <<external system>>                         |
|                     CPS Authentication Service                   |
| +------------------------------------------------------+        |
| |                                                      |        |
| |                    LDAP/SSO                          |<-------+
| |                                                      |        |
| +------------------------------------------------------+        |
|                                                                  |
+------------------------------------------------------------------+
```

## Descrição dos Nós

### Nós de Cliente

1. **Estações de Trabalho / Notebooks**
   - **Descrição**: Computadores pessoais ou de trabalho utilizados pela equipe da Assessoria de Inclusão e coordenadores das unidades do CPS
   - **Ambiente de Execução**: Navegadores web modernos (Chrome, Firefox, Edge, Safari)
   - **Artefatos**: Interface Web da aplicação (HTML, CSS, JavaScript)
   - **Requisitos**: Navegador atualizado com suporte a HTML5, CSS3, JavaScript ES6+

2. **Dispositivos Móveis**
   - **Descrição**: Smartphones e tablets utilizados pelos usuários para acesso em trânsito
   - **Ambiente de Execução**: Navegadores web móveis
   - **Artefatos**: Interface Web responsiva
   - **Requisitos**: Navegador móvel atualizado com suporte a HTML5 e CSS3

### Nós de Servidor de Aplicação

1. **Load Balancer / Firewall**
   - **Descrição**: Dispositivo ou software responsável por distribuir as requisições entre os servidores de aplicação e aplicar regras de segurança
   - **Funções**: Balanceamento de carga, terminação SSL, filtragem de tráfego, proteção DDoS
   - **Tecnologia Recomendada**: NGINX ou HAProxy com mod_security

2. **Servidores de Aplicação**
   - **Descrição**: Servidores que executam o backend da aplicação
   - **Ambiente de Execução**: JVM (Java Virtual Machine) com Spring Boot
   - **Artefatos**: Aplicação backend empacotada como JAR ou WAR
   - **Configuração Recomendada**: Cluster com no mínimo 2 nós para alta disponibilidade
   - **Escalabilidade**: Horizontal, com adição de novos nós conforme necessário

3. **Servidor de Cache**
   - **Descrição**: Servidor dedicado para armazenamento em memória de dados frequentemente acessados
   - **Tecnologia Recomendada**: Redis
   - **Funções**: Cache de sessões, resultados de consultas frequentes, armazenamento temporário de dados de relatórios

### Nós de Persistência

1. **Servidor de Banco de Dados**
   - **Descrição**: Servidor que hospeda o sistema de gerenciamento de banco de dados
   - **Tecnologia Recomendada**: PostgreSQL em cluster com replicação
   - **Configuração**: Um nó master para escrita e múltiplos slaves para leitura
   - **Armazenamento**: Dados estruturados da aplicação, incluindo atendimentos, tarefas, usuários e metadados de materiais

2. **Servidor de Armazenamento de Arquivos**
   - **Descrição**: Servidor dedicado ao armazenamento de documentos e materiais
   - **Tecnologia Recomendada**: Sistema de arquivos em rede com redundância
   - **Função**: Armazenar arquivos físicos do repositório de materiais
   - **Consideração**: Integração com sistema de backup automático

### Nós de Serviços Externos

1. **Serviço de Email**
   - **Descrição**: Serviço para envio de notificações por email
   - **Tecnologia**: Servidor SMTP institucional ou serviço de email em nuvem
   - **Comunicação**: SMTP ou API REST

2. **Serviço de Autenticação CPS**
   - **Descrição**: Sistema de autenticação institucional do Centro Paula Souza
   - **Tecnologia**: LDAP ou Single Sign-On (SSO)
   - **Função**: Autenticar usuários utilizando as credenciais institucionais existentes

## Ambientes do Sistema

### Ambiente de Desenvolvimento

- **Propósito**: Suportar o desenvolvimento e testes unitários/integração pela equipe de desenvolvimento
- **Características**:
  - Configuração simplificada, geralmente em máquinas locais dos desenvolvedores
  - Banco de dados local ou compartilhado com dados de teste
  - Pode utilizar mocks para serviços externos
  - Geralmente não inclui balanceador de carga
  - Depuração ativada
  - Pode utilizar ferramentas como Docker para simular o ambiente completo

### Ambiente de Homologação

- **Propósito**: Validar novas versões do sistema antes da implantação em produção
- **Características**:
  - Configuração similar ao ambiente de produção, mas em escala reduzida
  - Dados anonimizados ou sintéticos que simulem dados reais
  - Integração com versões de teste dos serviços externos
  - Monitoramento ativo para validar performance
  - Acesso restrito a usuários de teste e equipe de qualidade

### Ambiente de Produção

- **Propósito**: Disponibilizar o sistema para uso real pelos usuários finais
- **Características**:
  - Configuração completa com todas as medidas de redundância e alta disponibilidade
  - Dados reais com medidas rigorosas de segurança e backup
  - Monitoramento constante de performance, disponibilidade e segurança
  - Acesso controlado para administração e manutenção
  - Procedimentos formais para implantação e rollback
  - Escalabilidade conforme demanda

## Considerações de Segurança

1. **Segurança de Comunicação**:
   - Tráfego criptografado usando HTTPS/TLS 1.3 ou superior
   - Certificados digitais válidos e atualizados
   - Implementação de cabeçalhos de segurança HTTP

2. **Segurança de Aplicação**:
   - Firewall de aplicação web para proteção contra ataques comuns (SQL Injection, XSS, CSRF)
   - Validação rigorosa de entradas em todos os níveis
   - Implementação de CAPTCHA para formulários públicos
   - Proteção contra ataques de força bruta

3. **Segurança de Dados**:
   - Criptografia de dados sensíveis em repouso
   - Acesso ao banco de dados restrito à rede interna
   - Credenciais e chaves de API armazenadas em cofre seguro
   - Implementação de auditoria para todas as operações críticas

4. **Controle de Acesso**:
   - Autenticação multifator para administradores
   - Aplicação rigorosa de controle de acesso baseado em perfis
   - Revogação automática de sessões inativas
   - Processo formal para gestão de permissões

5. **Segurança de Infraestrutura**:
   - Segregação de redes (DMZ para servidores expostos à internet)
   - Hardening de servidores seguindo benchmarks de segurança
   - Atualizações de segurança aplicadas regularmente
   - Monitoramento de vulnerabilidades e patches de segurança

## Considerações de Escalabilidade

1. **Escalabilidade Horizontal**:
   - Arquitetura sem estado (stateless) para permitir adição de novos servidores de aplicação
   - Sessões armazenadas externamente no Redis
   - Balanceador de carga configurado para distribuição equitativa

2. **Escalabilidade Vertical**:
   - Servidores dimensionados para permitir aumento de recursos (RAM, CPU) quando necessário
   - Monitoramento de utilização de recursos para identificar gargalos

3. **Escalabilidade do Banco de Dados**:
   - Separação de operações de leitura e escrita
   - Índices otimizados para consultas frequentes
   - Particionamento de tabelas para conjuntos de dados maiores
   - Configuração de pool de conexões adequado à carga

4. **Cache**:
   - Estratégia de cache em múltiplos níveis
   - Cache de consultas frequentes
   - Cache de templates e componentes da interface
   - Invalidação seletiva para manter consistência

5. **Processamento Assíncrono**:
   - Geração de relatórios complexos em background
   - Envio de notificações em massa através de filas
   - Processamento em batch para operações pesadas

## Requisitos de Hardware e Software

### Requisitos de Hardware

1. **Servidores de Aplicação (por nó)**:
   - CPU: 4 cores, 2.5GHz ou superior
   - Memória: 16GB RAM mínimo
   - Armazenamento: 100GB SSD
   - Rede: 1Gbps

2. **Servidor de Banco de Dados**:
   - CPU: 8 cores, 3.0GHz ou superior
   - Memória: 32GB RAM mínimo
   - Armazenamento: 1TB SSD em RAID para dados
   - Rede: 10Gbps

3. **Servidor de Armazenamento**:
   - CPU: 4 cores, 2.5GHz ou superior
   - Memória: 16GB RAM
   - Armazenamento: Array de discos em RAID com capacidade inicial de 2TB, expansível
   - Rede: 10Gbps

4. **Servidor de Cache**:
   - CPU: 4 cores, 2.5GHz ou superior
   - Memória: 16GB RAM dedicados
   - Armazenamento: 50GB SSD
   - Rede: 1Gbps

### Requisitos de Software

1. **Servidores de Aplicação**:
   - Sistema Operacional: Linux (Ubuntu Server LTS ou Red Hat Enterprise Linux)
   - JDK: OpenJDK 17 ou superior
   - Servidor de Aplicação: Spring Boot embedded ou Tomcat
   - Bibliotecas adicionais: Conforme especificação técnica detalhada

2. **Servidor de Banco de Dados**:
   - Sistema Operacional: Linux (Ubuntu Server LTS ou Red Hat Enterprise Linux)
   - SGBD: PostgreSQL 14 ou superior
   - Extensões: PostGIS (para dados geográficos), pgcrypto

3. **Balanceador de Carga**:
   - Sistema Operacional: Linux (distribuição hardened)
   - Software: NGINX ou HAProxy
   - Módulos: mod_security, SSL/TLS

4. **Servidor de Cache**:
   - Sistema Operacional: Linux
   - Software: Redis 6 ou superior

5. **Software de Monitoramento**:
   - Prometheus para métricas
   - Grafana para visualização
   - ELK Stack (Elasticsearch, Logstash, Kibana) para análise de logs

## Estratégias de Backup e Recuperação

1. **Backup de Banco de Dados**:
   - Backup completo diário durante janela de manutenção
   - Backup incremental a cada 4 horas
   - Retenção: 7 dias para backups diários, 30 dias para backups semanais, 365 dias para backups mensais
   - Armazenamento: Local e em local seguro off-site ou nuvem

2. **Backup de Arquivos**:
   - Backup incremental diário do repositório de materiais
   - Retenção similar à política de banco de dados
   - Verificação periódica da integridade dos backups

3. **Recuperação de Desastres**:
   - Documentação detalhada de procedimentos de recuperação
   - Testes periódicos de restauração em ambiente de homologação
   - RPO (Recovery Point Objective): Máximo de 4 horas de perda de dados
   - RTO (Recovery Time Objective): Máximo de 8 horas para restauração completa

4. **Alta Disponibilidade**:
   - Cluster de aplicação para tolerância a falhas de servidores individuais
   - Replicação síncrona do banco de dados para o slave primário
   - Monitoramento proativo com alertas automáticos

## Recomendações e Boas Práticas

1. **Containerização**:
   - Considerar a adoção de containers Docker para padronizar ambientes
   - Utilizar orquestração (Kubernetes) para gerenciar deployments em ambientes maiores
   - Implementar pipeline CI/CD para automação de implantação

2. **Monitoramento e Observabilidade**:
   - Implementar monitoramento extensivo de métricas de aplicação e infraestrutura
   - Centralizar logs para análise e correlação
   - Configurar alertas para condições anômalas
   - Implementar APM (Application Performance Monitoring) para diagnóstico de problemas

3. **Gestão de Configuração**:
   - Externalizar configurações do código-fonte
   - Utilizar variáveis de ambiente ou serviço de configuração centralizado
   - Implementar perfis para diferentes ambientes

4. **Testes de Carga**:
   - Realizar testes de carga periódicos para validar a capacidade do sistema
   - Simular picos de uso esperados durante períodos críticos (início de semestres, etc.)
   - Verificar comportamento do sistema sob estresse

5. **Documentação de Infraestrutura**:
   - Manter documentação atualizada da infraestrutura
   - Utilizar Infrastructure as Code (IaC) quando possível
   - Documentar procedimentos operacionais

6. **Treinamento e Capacitação**:
   - Garantir que a equipe de operações esteja treinada nos componentes utilizados
   - Documentar procedimentos para situações de emergência
   - Promover transferência de conhecimento entre equipes