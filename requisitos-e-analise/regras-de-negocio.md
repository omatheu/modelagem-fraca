# Regras de Negócio
## Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza

## Sumário
- [Introdução](#introdução)
- [Regras de Atendimento](#regras-de-atendimento)
- [Regras de Usuários e Permissões](#regras-de-usuários-e-permissões)
- [Regras de Fluxo de Trabalho](#regras-de-fluxo-de-trabalho)
- [Regras de Gestão de Informações](#regras-de-gestão-de-informações)
- [Regras de Confidencialidade e Privacidade](#regras-de-confidencialidade-e-privacidade)
- [Regras de Relatórios e Estatísticas](#regras-de-relatórios-e-estatísticas)

## Introdução

Este documento descreve as regras de negócio que governam o funcionamento do Sistema de Gestão para a Assessoria de Inclusão do Centro Paula Souza. As regras de negócio são declarações que definem ou restringem aspectos do negócio, independente de como serão implementadas tecnicamente. Elas refletem políticas, procedimentos e restrições que a Assessoria de Inclusão deve seguir durante sua operação.

O objetivo é garantir que o sistema atenda corretamente aos processos de trabalho da Assessoria de Inclusão, assegurando a qualidade, consistência e conformidade com normas institucionais e legislação aplicável.

## Regras de Atendimento

### RA-01: Registro de Atendimentos
Todo atendimento registrado no sistema deve conter, no mínimo, as seguintes informações: data de abertura, unidade solicitante, tipo de necessidade, descrição da demanda e responsável pelo registro.

### RA-02: Atribuição de Responsável
**Descrição:** Todo atendimento deve ser atribuído a um responsável da Assessoria de Inclusão.  
**Justificativa:** Garantir que nenhum atendimento fique sem acompanhamento e que haja clara definição de responsabilidades.  
**Impacto:** Não deve ser possível finalizar o registro de um atendimento sem a designação de um responsável.

### RA-03: Priorização de Atendimentos
**Descrição:** A priorização dos atendimentos deve seguir critérios estabelecidos pela Assessoria de Inclusão, considerando urgência e impacto.  
**Justificativa:** Assegurar que casos mais críticos sejam tratados prioritariamente.  
**Impacto:** O sistema deve permitir a classificação de atendimentos por níveis de prioridade (baixa, média, alta, crítica).

### RA-04: Prazos de Atendimento
**Descrição:** Os prazos para resolução de atendimentos devem seguir as seguintes diretrizes:
- Prioridade Crítica: até 2 dias úteis
- Prioridade Alta: até 5 dias úteis
- Prioridade Média: até 10 dias úteis
- Prioridade Baixa: até 20 dias úteis  
**Justificativa:** Estabelecer expectativas claras para resolução de demandas.  
**Impacto:** O sistema deve calcular automaticamente as datas previstas de conclusão com base na prioridade atribuída.

### RA-05: Categorização de Atendimentos
**Descrição:** Todo atendimento deve ser categorizado por tipo de necessidade (física, visual, auditiva, intelectual, múltipla, etc.).  
**Justificativa:** Permitir análises estatísticas e direcionamento adequado dos atendimentos.  
**Impacto:** O sistema deve exigir a seleção de pelo menos uma categoria no momento do registro.

### RA-06: Histórico de Alterações
**Descrição:** Toda alteração de status em um atendimento deve ser registrada no histórico, incluindo data, hora e usuário responsável pela alteração.  
**Justificativa:** Garantir rastreabilidade e transparência no processo de atendimento.  
**Impacto:** O sistema deve manter log automático de todas as alterações em atendimentos.

## Regras de Usuários e Permissões

### RUP-01: Perfis de Acesso
**Descrição:** O sistema deve implementar, no mínimo, os seguintes perfis de acesso:
- Administrador: acesso total ao sistema
- Gestor da Assessoria: acesso a todas as funcionalidades operacionais
- Técnico da Assessoria: acesso às funcionalidades de registro e acompanhamento
- Coordenador de Unidade: acesso limitado aos dados de sua unidade  
**Justificativa:** Garantir que os usuários acessem apenas as funcionalidades pertinentes às suas responsabilidades.  
**Impacto:** Menus e funcionalidades devem ser exibidos conforme o perfil do usuário logado.

### RUP-02: Solicitações por Unidade
**Descrição:** Coordenadores de Unidade podem registrar solicitações de atendimento apenas para suas próprias unidades.  
**Justificativa:** Manter a organização e evitar registros indevidos.  
**Impacto:** O sistema deve restringir automaticamente a visualização e registro de solicitações à unidade do coordenador.

### RUP-03: Atribuição de Responsáveis
**Descrição:** Somente usuários com perfil de Gestor ou Administrador podem atribuir responsáveis aos atendimentos.  
**Justificativa:** Centralizar a distribuição de tarefas em funções gerenciais.  
**Impacto:** A funcionalidade de atribuição de responsáveis deve estar disponível apenas para esses perfis.

### RUP-04: Inativação de Usuários
**Descrição:** Usuários não podem ser excluídos do sistema, apenas inativados.  
**Justificativa:** Manter histórico e integridade referencial dos registros.  
**Impacto:** Registros associados a usuários inativados devem manter a referência original.

## Regras de Fluxo de Trabalho

### RFT-01: Estados de Atendimento
**Descrição:** Um atendimento deve passar pelos seguintes estados:
1. Aberto/Registrado
2. Em Análise
3. Em Atendimento
4. Aguardando Informações
5. Concluído
6. Cancelado  
**Justificativa:** Padronizar o fluxo de tratamento dos atendimentos.  
**Impacto:** Transições entre estados devem seguir essa sequência lógica, com exceção do estado "Aguardando Informações" que pode ocorrer após "Em Análise" ou "Em Atendimento".

### RFT-02: Reabertura de Atendimentos
**Descrição:** Atendimentos concluídos podem ser reabertos em até 30 dias após seu fechamento.  
**Justificativa:** Permitir correções em casos de resolução inadequada, mantendo limite temporal.  
**Impacto:** Após 30 dias, a reabertura deve requerer autorização específica de um Gestor.

### RFT-03: Notificações Automáticas
**Descrição:** O sistema deve enviar notificações automáticas nos seguintes casos:
- Atribuição de novo atendimento a um técnico
- Alteração de status de atendimento
- Atendimento próximo do prazo de conclusão (2 dias antes)
- Atendimento com prazo vencido  
**Justificativa:** Manter os envolvidos informados sobre o andamento dos atendimentos.  
**Impacto:** Configuração de e-mails e alertas internos no sistema.

### RFT-04: Encerramento de Atendimentos
**Descrição:** Para encerrar um atendimento como "Concluído", é obrigatório o preenchimento de uma descrição da solução implementada.  
**Justificativa:** Documentar adequadamente as soluções para referência futura.  
**Impacto:** O sistema deve exigir este campo antes de permitir a conclusão do atendimento.

### RFT-05: Vinculação de Tarefas
**Descrição:** Cada atendimento pode ter múltiplas tarefas vinculadas, mas cada tarefa deve estar associada a exatamente um atendimento.  
**Justificativa:** Organizar ações específicas necessárias à resolução do atendimento.  
**Impacto:** O modelo de dados deve refletir esta relação de um-para-muitos.

## Regras de Gestão de Informações

### RGI-01: Cadastro de Unidades
**Descrição:** Todas as unidades do CPS (Etecs e Fatecs) devem estar cadastradas no sistema com suas características de acessibilidade.  
**Justificativa:** Manter informações atualizadas sobre a infraestrutura de acessibilidade disponível.  
**Impacto:** O cadastro de unidades deve incluir campos específicos para registro de recursos de acessibilidade.

### RGI-02: Repositório de Materiais
**Descrição:** Materiais incluídos no repositório devem ser categorizados e podem ter restrições de acesso conforme sensibilidade do conteúdo.  
**Justificativa:** Organizar os recursos e proteger informações sensíveis.  
**Impacto:** O sistema deve implementar controle de acesso granular para documentos e recursos.

### RGI-03: Versão de Materiais
**Descrição:** O sistema deve manter o histórico de versões de materiais e documentos armazenados.  
**Justificativa:** Permitir o acompanhamento da evolução dos documentos e retorno a versões anteriores se necessário.  
**Impacto:** Implementação de controle de versão para arquivos armazenados.

### RGI-04: Adaptações Curriculares
**Descrição:** O registro de adaptações curriculares deve incluir o curso, disciplina, tipo de adaptação, justificativa e resultados observados.  
**Justificativa:** Documentar adequadamente as adaptações para referência e aplicação em casos similares.  
**Impacto:** Formulário específico para registro estruturado destas informações.

## Regras de Confidencialidade e Privacidade

### RCP-01: Tratamento de Dados Pessoais
**Descrição:** Dados pessoais de alunos com NEE devem ser tratados conforme as diretrizes da LGPD.  
**Justificativa:** Conformidade legal e proteção da privacidade.  
**Impacto:** Implementação de medidas de segurança como criptografia, controle de acesso e registro de auditoria.

### RCP-02: Anonimização em Relatórios
**Descrição:** Relatórios estatísticos não devem expor informações que permitam identificação individual de alunos.  
**Justificativa:** Proteger a privacidade dos alunos enquanto permite análises agregadas.  
**Impacto:** Mecanismos de anonimização em relatórios públicos ou compartilháveis.

### RCP-03: Consentimento
**Descrição:** O registro de informações sobre necessidades específicas de um aluno deve seguir os princípios de transparência e finalidade da LGPD.  
**Justificativa:** Garantir que o aluno ou seu responsável esteja ciente e consinta com o armazenamento de seus dados para fins específicos de inclusão.  
**Impacto:** Implementação de termos de consentimento e registro de aceitação.

### RCP-04: Retenção de Dados
**Descrição:** Os dados pessoais dos alunos devem ser mantidos apenas pelo período necessário ao atendimento e posteriormente anonimizados ou excluídos.  
**Justificativa:** Conformidade com o princípio de minimização de dados da LGPD.  
**Impacto:** Implementação de políticas de retenção e exclusão automática de dados.

## Regras de Relatórios e Estatísticas

### RRE-01: Periodicidade de Relatórios
**Descrição:** O sistema deve gerar automaticamente relatórios mensais, trimestrais e anuais sobre os atendimentos realizados.  
**Justificativa:** Acompanhar periodicamente o desempenho da Assessoria de Inclusão.  
**Impacto:** Configuração de jobs para geração automática de relatórios em datas específicas.

### RRE-02: Indicadores de Performance
**Descrição:** O sistema deve calcular e apresentar os seguintes indicadores:
- Tempo médio de resolução por tipo de atendimento
- Percentual de atendimentos concluídos dentro do prazo
- Distribuição de atendimentos por tipo de necessidade
- Distribuição de atendimentos por unidade  
**Justificativa:** Permitir análise de eficiência e identificação de áreas que necessitam atenção.  
**Impacto:** Implementação de algoritmos para cálculo automático destes indicadores.

### RRE-03: Exportação de Dados
**Descrição:** Todos os relatórios devem ser exportáveis nos formatos PDF, Excel e CSV.  
**Justificativa:** Flexibilidade no uso dos dados para apresentações e análises externas.  
**Impacto:** Implementação de rotinas de exportação nos formatos especificados.

### RRE-04: Dados Históricos
**Descrição:** O sistema deve manter dados históricos completos por um período mínimo de 5 anos para fins de análises comparativas.  
**Justificativa:** Permitir análises de tendências e evolução dos atendimentos ao longo do tempo.  
**Impacto:** Planejamento de infraestrutura para armazenamento de dados históricos.