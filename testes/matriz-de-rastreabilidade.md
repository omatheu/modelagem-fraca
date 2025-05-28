# Matriz de Rastreabilidade
## Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza

## Sumário
- [Introdução](#introdução)
- [Objetivos](#objetivos)
- [Metodologia](#metodologia)
- [Matriz de Rastreabilidade de Requisitos x Casos de Teste](#matriz-de-rastreabilidade-de-requisitos-x-casos-de-teste)
- [Matriz de Rastreabilidade de Histórias de Usuário x Casos de Teste](#matriz-de-rastreabilidade-de-histórias-de-usuário-x-casos-de-teste)
- [Matriz de Requisitos Não-Funcionais x Casos de Teste](#matriz-de-requisitos-não-funcionais-x-casos-de-teste)
- [Análise de Cobertura](#análise-de-cobertura)
- [Manutenção da Matriz](#manutenção-da-matriz)

## Introdução

Este documento apresenta a matriz de rastreabilidade para o Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza. A matriz de rastreabilidade é uma ferramenta essencial para garantir que todos os requisitos especificados sejam adequadamente testados, ajudando a verificar a cobertura dos testes e facilitando o gerenciamento de mudanças.

A matriz estabelece relações claras entre os diferentes artefatos do projeto, como requisitos funcionais, requisitos não-funcionais, histórias de usuário e casos de teste. Isso permite uma visibilidade completa de como os requisitos estão sendo verificados e validados durante o ciclo de desenvolvimento do sistema.

## Objetivos

Os principais objetivos desta matriz de rastreabilidade são:

1. Garantir que todos os requisitos funcionais e não-funcionais sejam cobertos por casos de teste apropriados
2. Facilitar a análise de impacto quando ocorrerem mudanças nos requisitos
3. Identificar possíveis lacunas na estratégia de teste
4. Proporcionar uma visão clara das relações entre diferentes artefatos do projeto
5. Apoiar o processo de verificação e validação do sistema
6. Documentar a conformidade entre o que foi especificado e o que está sendo testado

## Metodologia

A matriz de rastreabilidade foi desenvolvida seguindo estas etapas:

1. **Coleta e análise de requisitos**: Todos os requisitos funcionais e não-funcionais foram extraídos da Especificação de Requisitos de Software (SRS).
2. **Identificação de histórias de usuário**: As histórias de usuário foram coletadas do documento de Histórias de Usuário e Casos de Uso.
3. **Definição de casos de teste**: Os casos de teste foram definidos para verificar a implementação adequada dos requisitos.
4. **Mapeamento**: Cada caso de teste foi associado aos requisitos correspondentes que ele verifica.
5. **Análise de cobertura**: Foi realizada uma análise para identificar requisitos sem cobertura adequada de teste.

## Matriz de Rastreabilidade de Requisitos x Casos de Teste

Esta seção apresenta a matriz de rastreabilidade que relaciona os requisitos funcionais do sistema com os casos de teste correspondentes.

| Requisito | Descrição | Casos de Teste |
|-----------|-----------|----------------|
| **RF-01.1** | Login no Sistema | CT-AAC-001, CT-AAC-002, CT-AAC-003, CT-AAC-004 |
| **RF-01.2** | Gerenciamento de Permissões | CT-AAC-005, CT-AAC-006 |
| **RF-01.3** | Recuperação de Senha | CT-AAC-004 |
| **RF-01.4** | Gerenciamento de Usuários | CT-AAC-007, CT-AAC-008 |
| **RF-02.1** | Registro de Atendimentos | CT-GA-001 |
| **RF-02.2** | Categorização de Atendimentos | CT-GA-002 |
| **RF-02.3** | Acompanhamento de Status | CT-GA-003, CT-GA-004, CT-GA-008 |
| **RF-02.4** | Histórico de Atendimentos | CT-GA-005 |
| **RF-02.5** | Atribuição de Responsáveis | CT-GA-006, CT-GA-007 |
| **RF-03.1** | Dashboard Personalizado | CT-OF-001 |
| **RF-03.2** | Gestão de Tarefas | CT-OF-002, CT-OF-003, CT-OF-004 |
| **RF-03.3** | Notificações e Alertas | CT-OF-005, CT-OF-006 |
| **RF-03.4** | Configuração de Fluxos de Trabalho | CT-OF-007 |
| **RF-04.1** | Relatórios Predefinidos | CT-GR-001, CT-GR-002 |
| **RF-04.2** | Gerador de Relatórios Customizados | CT-GR-003 |
| **RF-04.3** | Exportação de Dados | CT-GR-004, CT-GR-005 |
| **RF-04.4** | Visualizações Gráficas | CT-GR-006 |
| **RF-05.1** | Cadastro de Unidades | CT-GI-001, CT-GI-002 |
| **RF-05.2** | Repositório de Materiais | CT-GI-003, CT-GI-004, CT-GI-005, CT-GI-006 |
| **RF-05.3** | Cadastro de Profissionais Especializados | - |
| **RF-05.4** | Registro de Adaptações Curriculares | - |

## Matriz de Rastreabilidade de Histórias de Usuário x Casos de Teste

Esta matriz relaciona as histórias de usuário com os casos de teste que validam os comportamentos descritos nessas histórias.

| História de Usuário | Descrição | Casos de Teste |
|---------------------|-----------|----------------|
| **HU-01** | Acesso ao Sistema | CT-AAC-001, CT-AAC-002, CT-AAC-003 |
| **HU-02** | Gerenciamento de Usuários | CT-AAC-007, CT-AAC-008 |
| **HU-03** | Recuperação de Senha | CT-AAC-004 |
| **HU-04** | Registro de Atendimentos | CT-GA-001 |
| **HU-05** | Categorização de Atendimentos | CT-GA-002 |
| **HU-06** | Acompanhamento de Status | CT-GA-003, CT-GA-004 |
| **HU-07** | Visualização de Histórico | CT-GA-005 |
| **HU-09** | Visualização de Dashboard | CT-OF-001 |
| **HU-10** | Gestão de Tarefas | CT-OF-002, CT-OF-003, CT-OF-004 |
| **HU-11** | Recebimento de Notificações | CT-OF-005, CT-OF-006 |
| **HU-12** | Atribuição de Responsabilidades | CT-GA-006, CT-GA-007 |
| **HU-13** | Relatórios Predefinidos | CT-GR-001, CT-GR-002 |
| **HU-14** | Relatórios Personalizados | CT-GR-003 |
| **HU-15** | Exportação de Relatórios | CT-GR-004, CT-GR-005 |
| **HU-16** | Visualizações Gráficas | CT-GR-006 |
| **HU-17** | Cadastro de Unidades | CT-GI-001, CT-GI-002 |
| **HU-18** | Repositório de Materiais | CT-GI-003, CT-GI-004, CT-GI-005, CT-GI-006 |
| **HU-19** | Cadastro de Profissionais | - |
| **HU-20** | Registro de Adaptações Curriculares | - |

## Matriz de Requisitos Não-Funcionais x Casos de Teste

Esta matriz relaciona os requisitos não-funcionais com os casos de teste específicos que verificam o atendimento desses requisitos.

| Requisito | Descrição | Casos de Teste |
|-----------|-----------|----------------|
| **RNF-01.1** | Autenticação e Autorização | CT-AAC-001, CT-AAC-002, CT-AAC-005, CT-AAC-006 |
| **RNF-01.2** | Proteção de Dados | - |
| **RNF-01.3** | Auditoria | - |
| **RNF-02.1** | Facilidade de Uso | - |
| **RNF-02.2** | Design Responsivo | - |
| **RNF-02.3** | Acessibilidade | CT-NF-003 |
| **RNF-03.1** | Tempo de Resposta | CT-NF-001 |
| **RNF-03.2** | Capacidade | CT-NF-002 |
| **RNF-03.3** | Disponibilidade | - |
| **RNF-04.1** | Modularidade | - |
| **RNF-04.2** | Documentação | - |
| **RNF-05** | Portabilidade | CT-NF-004 |
| **AQ-01.1** | Tolerância a Falhas | CT-NF-005 |
| **AQ-01.2** | Precisão | - |
| **AQ-02.1** | Crescimento de Dados | - |
| **AQ-02.2** | Novos Módulos | - |

## Análise de Cobertura

### Requisitos Funcionais sem Cobertura de Teste

Os seguintes requisitos funcionais não possuem casos de teste associados ou possuem cobertura insuficiente:

1. **RF-05.3**: Cadastro de Profissionais Especializados
   - *Recomendação*: Desenvolver casos de teste para validar o cadastro, edição, consulta e exclusão de profissionais especializados.

2. **RF-05.4**: Registro de Adaptações Curriculares
   - *Recomendação*: Desenvolver casos de teste para validar o registro, consulta e manutenção de adaptações curriculares.

### Requisitos Não-Funcionais sem Cobertura de Teste

Os seguintes requisitos não-funcionais não possuem casos de teste associados ou possuem cobertura insuficiente:

1. **RNF-01.2**: Proteção de Dados
   - *Recomendação*: Desenvolver testes específicos para verificar a proteção de dados sensíveis, incluindo criptografia e conformidade com a LGPD.

2. **RNF-01.3**: Auditoria
   - *Recomendação*: Criar casos de teste para verificar se o sistema está registrando corretamente as ações dos usuários em logs de auditoria.

3. **RNF-02.1**: Facilidade de Uso
   - *Recomendação*: Realizar testes de usabilidade com diferentes perfis de usuários.

4. **RNF-02.2**: Design Responsivo
   - *Recomendação*: Desenvolver casos de teste para verificar o comportamento da interface em diferentes tamanhos de tela e dispositivos.

5. **RNF-03.3**: Disponibilidade
   - *Recomendação*: Implementar testes de disponibilidade contínua e recuperação após falhas.

6. **RNF-04.1**: Modularidade
   - *Recomendação*: Desenvolver testes para verificar a independência dos módulos do sistema.

7. **RNF-04.2**: Documentação
   - *Recomendação*: Estabelecer critérios de verificação para a documentação técnica.

8. **AQ-01.2**: Precisão
   - *Recomendação*: Criar casos de teste para verificar a precisão e integridade dos dados em todas as operações.

9. **AQ-02.1**: Crescimento de Dados
   - *Recomendação*: Desenvolver testes de carga para simular o crescimento do volume de dados ao longo do tempo.

10. **AQ-02.2**: Novos Módulos
    - *Recomendação*: Realizar testes de integração com módulos fictícios para validar a capacidade de extensão da arquitetura.

## Manutenção da Matriz

A matriz de rastreabilidade deve ser um documento vivo, atualizado ao longo do ciclo de desenvolvimento do projeto. As seguintes diretrizes devem ser seguidas para manter a matriz atualizada:

1. **Atualizações regulares**:
   - A matriz deve ser atualizada sempre que novos requisitos forem adicionados
   - Novos casos de teste devem ser incorporados à matriz quando criados
   - Modificações em requisitos existentes devem refletir atualizações correspondentes na matriz

2. **Responsabilidades**:
   - O Gerente de QA é responsável pela manutenção geral da matriz
   - Analistas de requisitos devem notificar sobre mudanças nos requisitos
   - Analistas de teste devem atualizar os mapeamentos quando novos casos de teste forem criados

3. **Revisões periódicas**:
   - A matriz deve ser revisada mensalmente para garantir sua precisão
   - Análises de cobertura devem ser realizadas antes de cada ciclo de teste
   - Relatórios de lacunas devem ser gerados e distribuídos à equipe

4. **Versionamento**:
   - A matriz deve seguir o mesmo controle de versão dos demais artefatos do projeto
   - Mudanças significativas devem ser documentadas no histórico de revisões
   - A versão mais recente deve estar sempre disponível no repositório do projeto