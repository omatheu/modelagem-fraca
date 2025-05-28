# Diagramas de Contexto
## Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza

## Sumário
- [Introdução](#introdução)
- [Notação Utilizada](#notação-utilizada)
- [Diagrama de Contexto de Sistema](#diagrama-de-contexto-de-sistema)
- [Diagrama de Contexto Organizacional](#diagrama-de-contexto-organizacional)
- [Atores e Sistemas Externos](#atores-e-sistemas-externos)
- [Fluxos de Informação Principais](#fluxos-de-informação-principais)
- [Delimitação de Escopo do Sistema](#delimitação-de-escopo-do-sistema)
- [Considerações de Implementação](#considerações-de-implementação)

## Introdução

Os diagramas de contexto são representações visuais de alto nível que mostram o sistema como uma única entidade e suas interações com elementos externos. Esses diagramas são particularmente úteis nas fases iniciais do projeto para estabelecer claramente os limites do sistema, identificar os atores externos e sistemas que interagem com ele, e definir as principais trocas de informação.

Enquanto os diagramas de componentes, pacotes e implantação focam nos aspectos internos e técnicos do sistema, os diagramas de contexto concentram-se nas interações externas e no posicionamento do sistema dentro do ambiente organizacional mais amplo do Centro Paula Souza.

Este documento apresenta dois tipos de diagramas de contexto para o Sistema de Gestão da Assessoria de Inclusão:

1. **Diagrama de Contexto de Sistema**: Mostra o sistema como uma única entidade e suas interfaces com atores externos e outros sistemas.
2. **Diagrama de Contexto Organizacional**: Ilustra como o sistema se encaixa na estrutura organizacional do Centro Paula Souza e nos processos de trabalho da Assessoria de Inclusão.

## Notação Utilizada

Os diagramas de contexto apresentados neste documento seguem uma notação simplificada e de fácil compreensão:

- **Retângulo com bordas arredondadas**: Representa o sistema em foco (Sistema de Gestão da Assessoria de Inclusão)
- **Retângulos**: Representam sistemas externos ou departamentos organizacionais
- **Círculos/Elipses**: Representam pessoas ou grupos de pessoas (atores externos)
- **Setas direcionais**: Representam fluxos de informação ou interações, com descrições do tipo de informação sendo trocada
- **Linhas tracejadas**: Delimitam fronteiras organizacionais ou de responsabilidade
- **Cores**: Usadas para diferenciar categorias de elementos (sistemas internos, sistemas externos, atores humanos, etc.)

## Diagrama de Contexto de Sistema

```
                               +-------------------+
                               |                   |
                               |  Sistema de Email |
                               |     do CPS        |<------------+
                               |                   |             |
                               +-------------------+             |
                                       ^                         |
                                       | Envia                   | Envia
                                       | notificações            | relatórios
                                       |                         |
                               +-------v---------------+         |
+----------------------------->+                       |         |
|                              |                       |         |
|  Fornece                     |   Sistema de Gestão   +-----+   |
|  credenciais                 |   da Assessoria de    |     |   |
|                              |        Inclusão       |     |   |
+-------------+                |                       |     |   |
              |                |                       |     |   |
              |                +-----------+-----------+     |   |
  +---------------+                        ^                 |   |
  |               |                        |                 |   |
  | Sistema LDAP/ |                        | Registra/       |   |
  |   SSO do CPS  |                        | Consulta        |   |
  |               |                        | atendimentos    |   |
  +---------------+                        |                 |   |
                           +---------------v--+              |   |
                           |                  |              |   |
                           |  Coordenadores   |              |   |
                           |  de Unidades     |              |   |
                           |  (Etecs/Fatecs)  |              |   |
                           |                  |              |   |
                           +------------------+              |   |
                                                             |   |
                                                             |   |
       +------------------+                  +--------------+|   |
       |                  |                  |              ||   |
       |  Administradores |                  |   Técnicos   ||   |
       |  do Sistema      |                  |   da Equipe  +----+
       |                  |<---------------->|   Assessoria |
       +------------------+  Gerencia        |              |
                             usuários/       +--------------+
                             configurações      ^
                                               /|\
                                                |
                                                | Consulta/atualiza
                                                | materiais
                                           +----+----+
                                           |         |
                                           | Gestores|
                                           |         |
                                           +---------+
```

## Diagrama de Contexto Organizacional

```
+-----------------------------------------------------------------------+
|                                                                       |
|                     Centro Paula Souza (CPS)                          |
|                                                                       |
| +---------------------------+        +---------------------------+    |
| |                           |        |                           |    |
| |   Unidades Educacionais   |        |   Administração Central   |    |
| |   (Etecs e Fatecs)        |        |                           |    |
| |                           |        |                           |    |
| +-------------+-------------+        +-----------+---------------+    |
|               |                                  |                    |
|               v                                  |                    |
|    +----------+------------+          +---------v---------------+    |
|    |                       |          |                         |    |
|    |  NAPNE                |          |  Assessoria de Inclusão |    |
|    |  (Núcleo de Apoio às  +--------->+                         |    |
|    |  Pessoas com          |Encaminha |                         |    |
|    |  Necessidades Especiais) casos   |                         |    |
|    |                       |          |                         |    |
|    +-----------------------+          |  +-------------------+  |    |
|                                       |  |                   |  |    |
|                                       |  | Sistema de Gestão |  |    |
|                                       |  | da Assessoria de  |  |    |
|                                       |  | Inclusão          |  |    |
|                                       |  |                   |  |    |
|                                       |  +-------------------+  |    |
|                                       |                         |    |
|                                       +-------------------------+    |
|                                                |                     |
|                                                v                     |
|                              +----------------+----------------+     |
|                              |                                 |     |
|                              | Órgãos Gestores                 |     |
|                              | (Superintendência, Diretorias)  |     |
|                              |                                 |     |
|                              +---------------------------------+     |
|                                                                       |
+-----------------------------------------------------------------------+

                +----------------------------------+
                |                                  |
                |  Sistemas e Serviços Externos    |
                |  - Sistema Acadêmico             |
                |  - Sistema de Email              |
                |  - LDAP/SSO                      |
                |                                  |
                +----------------------------------+
```

## Atores e Sistemas Externos

### Atores Humanos

1. **Coordenadores de Unidades (Etecs/Fatecs)**
   - **Descrição**: Profissionais responsáveis pela coordenação pedagógica nas unidades educacionais
   - **Interações**: Registram solicitações de atendimento, acompanham status, consultam materiais
   - **Necessidades**: Informações claras sobre os casos, ferramentas para acompanhamento dos atendimentos

2. **Técnicos da Assessoria**
   - **Descrição**: Especialistas que executam os atendimentos e providenciam soluções
   - **Interações**: Recebem tarefas, atualizam status, registram ações, produzem materiais
   - **Necessidades**: Visão clara de suas responsabilidades, acesso rápido às informações dos casos

3. **Gestores da Assessoria**
   - **Descrição**: Coordenadores da Assessoria de Inclusão que supervisionam os atendimentos
   - **Interações**: Atribuem responsáveis, monitoram indicadores, geram relatórios
   - **Necessidades**: Visão consolidada da operação, ferramentas para análise e tomada de decisão

4. **Administradores do Sistema**
   - **Descrição**: Profissionais de TI responsáveis pela manutenção do sistema
   - **Interações**: Configuram parâmetros, gerenciam usuários, monitoram desempenho
   - **Necessidades**: Ferramentas de administração, logs de auditoria, alertas de problemas

### Sistemas Externos

1. **Sistema LDAP/SSO do CPS**
   - **Propósito**: Autenticação centralizada de usuários
   - **Interações**: Valida credenciais, fornece informações básicas do usuário
   - **Requisitos de Integração**: Protocolo LDAP ou API para SSO

2. **Sistema de Email do CPS**
   - **Propósito**: Envio de notificações e comunicações
   - **Interações**: Recebe requisições para envio de mensagens
   - **Requisitos de Integração**: SMTP ou API específica

3. **Sistema Acadêmico**
   - **Propósito**: Gerencia informações sobre alunos e cursos
   - **Interações**: Fornece dados de estudantes e unidades quando necessário
   - **Requisitos de Integração**: API ou exportação/importação de dados

## Fluxos de Informação Principais

1. **Registro de Atendimentos**
   - **Origem**: Coordenadores de Unidades
   - **Destino**: Sistema de Gestão da Assessoria
   - **Conteúdo**: Informações sobre casos que necessitam de atendimento
   - **Frequência**: Sob demanda, conforme necessidades identificadas nas unidades

2. **Notificações de Atribuição**
   - **Origem**: Sistema de Gestão da Assessoria
   - **Destino**: Técnicos da Assessoria
   - **Conteúdo**: Novos casos atribuídos, prazos, tarefas
   - **Frequência**: Imediata após atribuição de responsabilidade

3. **Atualização de Status**
   - **Origem**: Técnicos da Assessoria
   - **Destino**: Sistema de Gestão da Assessoria
   - **Conteúdo**: Progresso, ações tomadas, dificuldades encontradas
   - **Frequência**: Conforme avanço do atendimento

4. **Consulta de Materiais**
   - **Origem**: Todos os atores
   - **Destino**: Sistema de Gestão da Assessoria (Repositório)
   - **Conteúdo**: Busca por documentos de referência, materiais de apoio
   - **Frequência**: Conforme necessidade

5. **Geração de Relatórios**
   - **Origem**: Gestores da Assessoria
   - **Destino**: Sistema de Gestão da Assessoria
   - **Conteúdo**: Parâmetros para geração de relatórios analíticos ou operacionais
   - **Frequência**: Periódica (mensal, trimestral) ou sob demanda

6. **Distribuição de Relatórios**
   - **Origem**: Sistema de Gestão da Assessoria
   - **Destino**: Órgãos Gestores, Gestores da Assessoria
   - **Conteúdo**: Indicadores de desempenho, estatísticas, análises
   - **Frequência**: Conforme cronograma institucional ou sob demanda

## Delimitação de Escopo do Sistema

### Dentro do Escopo

1. **Gestão de Atendimentos**
   - Registro, categorização, acompanhamento e finalização de atendimentos
   - Histórico completo de ações realizadas
   - Atribuição de responsabilidades e prazos

2. **Fluxo de Trabalho**
   - Criação e gerenciamento de tarefas
   - Notificações e alertas de prazos
   - Monitoramento de atividades

3. **Repositório de Materiais**
   - Armazenamento e categorização de documentos
   - Versionamento de materiais
   - Busca avançada por metadados

4. **Relatórios e Analytics**
   - Geração de relatórios predefinidos e customizados
   - Visualizações gráficas de indicadores
   - Exportação em diferentes formatos

5. **Gestão de Usuários e Permissões**
   - Integração com LDAP/SSO para autenticação
   - Controle granular de acessos baseado em perfis
   - Auditoria de ações dos usuários

### Fora do Escopo

1. **Gestão Acadêmica**
   - O sistema não substitui o sistema acadêmico existente
   - Dados de alunos serão referenciados, não gerenciados integralmente

2. **Comunicação Direta com Alunos**
   - O sistema não inclui interfaces para uso direto pelos estudantes
   - Comunicações com estudantes ocorrem por meio dos canais institucionais existentes

3. **Gestão Financeira e Orçamentária**
   - O sistema não gerencia aspectos financeiros da Assessoria
   - Recursos necessários para os atendimentos são tratados em sistemas específicos

4. **Funcionalidades de Ensino a Distância**
   - O sistema não implementa ferramentas de EAD
   - Materiais educacionais são armazenados e categorizados, mas não há funcionalidade de curso online

## Considerações de Implementação

1. **Fronteiras de Sistemas**
   - Definir interfaces claras e padronizadas para integração com sistemas externos
   - Implementar adaptadores para cada sistema externo que possam ser evoluídos independentemente
   - Considerar mecanismos de fallback para quando sistemas externos estiverem indisponíveis

2. **Estratégias de Integração**
   - Para LDAP/SSO: Utilizar bibliotecas padronizadas e implementar cache local de autenticação
   - Para Sistema de Email: Implementar filas de mensagens para garantir entrega mesmo em caso de falhas temporárias
   - Para Sistema Acadêmico: Considerar sincronização periódica de dados relevantes em vez de consulta em tempo real

3. **Considerações de Segurança**
   - Implementar autenticação em múltiplos níveis para acesso aos dados sensíveis
   - Estabelecer políticas claras de acesso para cada tipo de informação
   - Registrar todas as interações com sistemas externos para auditoria
   - Garantir transmissão criptografada de dados entre sistemas

4. **Extensibilidade**
   - Projetar o sistema prevendo a adição de novas integrações futuras
   - Utilizar padrões e protocolos abertos para facilitar incorporação de novos sistemas
   - Documentar detalhadamente as interfaces externas para facilitar expansão

5. **Resiliência**
   - Desenvolver o sistema para operar com funcionalidade limitada mesmo quando sistemas externos estão indisponíveis
   - Implementar filas para operações assíncronas que dependem de sistemas externos
   - Estabelecer estratégias de retry e circuit breaker para falhas de integração