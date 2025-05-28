# Mapas de Jornada do Usuário
## Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza

## Sumário
- [Introdução](#introdução)
- [Objetivos dos Mapas de Jornada](#objetivos-dos-mapas-de-jornada)
- [Personas](#personas)
- [Jornadas Mapeadas](#jornadas-mapeadas)
  - [Jornada 1: Registro e Acompanhamento de Atendimento](#jornada-1-registro-e-acompanhamento-de-atendimento)
  - [Jornada 2: Gestão de Atendimentos](#jornada-2-gestão-de-atendimentos)
  - [Jornada 3: Execução de Atendimento](#jornada-3-execução-de-atendimento)
  - [Jornada 4: Geração de Relatórios](#jornada-4-geração-de-relatórios)
  - [Jornada 5: Gestão do Repositório de Materiais](#jornada-5-gestão-do-repositório-de-materiais)
- [Considerações de Design](#considerações-de-design)
- [Pontos de Melhoria Identificados](#pontos-de-melhoria-identificados)

## Introdução

Este documento apresenta os Mapas de Jornada do Usuário para o Sistema de Gestão da Assessoria de Inclusão do Centro Paula Souza. Os mapas de jornada são uma ferramenta visual que documenta a experiência completa de um usuário ao interagir com o sistema, incluindo suas motivações, ações, pensamentos, sentimentos e pontos de contato com a interface.

Enquanto os diagramas BPMN e de atividades focam nos processos e fluxos lógicos, os mapas de jornada centram-se na perspectiva humana da interação, destacando aspectos emocionais, pontos de dor e oportunidades de melhoria da experiência do usuário.

## Objetivos dos Mapas de Jornada

- **Empatia**: Desenvolver uma compreensão profunda das necessidades, motivações e frustrações dos diferentes usuários do sistema.
- **Identificação de Pontos de Dor**: Revelar momentos de dificuldade ou insatisfação na interação com o sistema.
- **Otimização de Experiência**: Fornecer insights para o desenvolvimento de interfaces e interações que atendam melhor às expectativas dos usuários.
- **Design Centrado no Usuário**: Garantir que as funcionalidades sejam desenvolvidas considerando o contexto real de uso.
- **Alinhamento de Equipe**: Criar um entendimento comum sobre como os usuários interagem com o sistema.

## Personas

Para contextualizar os mapas de jornada, desenvolvemos as seguintes personas representativas dos principais tipos de usuários do sistema:

### Persona 1: Coordenador de Unidade

**Nome**: Carlos Silva  
**Cargo**: Coordenador Pedagógico de uma Etec  
**Idade**: 42 anos  
**Experiência Tecnológica**: Intermediária  
**Motivações**: Garantir que os alunos com necessidades especiais de sua unidade recebam o suporte adequado  
**Frustrações**: Demora nas respostas, falta de visibilidade sobre o andamento dos atendimentos  
**Contexto de Uso**: Acessa o sistema principalmente durante o horário de trabalho, muitas vezes com tempo limitado entre reuniões e atendimentos

### Persona 2: Gestor da Assessoria de Inclusão

**Nome**: Marina Gomes  
**Cargo**: Coordenadora da Assessoria de Inclusão do CPS  
**Idade**: 48 anos  
**Experiência Tecnológica**: Intermediária  
**Motivações**: Garantir o funcionamento eficiente da Assessoria, monitorar o desempenho da equipe, reportar resultados à direção  
**Frustrações**: Dificuldade em obter panoramas gerais, perda de tempo com tarefas administrativas  
**Contexto de Uso**: Usa o sistema diariamente, tanto para gestão da equipe quanto para análise de dados e preparação de relatórios

### Persona 3: Técnico da Assessoria de Inclusão

**Nome**: Rafael Menezes  
**Cargo**: Especialista em Educação Inclusiva  
**Idade**: 35 anos  
**Experiência Tecnológica**: Avançada  
**Motivações**: Resolver efetivamente os casos atribuídos, compartilhar conhecimentos especializados  
**Frustrações**: Documentação duplicada, falta de acesso a informações completas sobre os casos  
**Contexto de Uso**: Acessa o sistema constantemente durante o dia para registro e acompanhamento de múltiplos atendimentos simultâneos

### Persona 4: Administrador do Sistema

**Nome**: Juliana Costa  
**Cargo**: Analista de Sistemas do CPS  
**Idade**: 39 anos  
**Experiência Tecnológica**: Avançada  
**Motivações**: Garantir o funcionamento técnico correto do sistema, implementar melhorias, resolver problemas  
**Frustrações**: Pedidos mal definidos, expectativas irrealistas, problemas técnicos difíceis de reproduzir  
**Contexto de Uso**: Acesso periódico para manutenção e suporte, com uso intensivo durante implementações de atualizações

## Jornadas Mapeadas

### Jornada 1: Registro e Acompanhamento de Atendimento

**Persona**: Carlos Silva (Coordenador de Unidade)  
**Objetivo**: Registrar uma solicitação de atendimento para um aluno com deficiência visual e acompanhar seu andamento

#### Fases da Jornada

| Fase | Ações | Pensamentos | Sentimentos | Pontos de Contato | Oportunidades de Melhoria |
|------|-------|-------------|-------------|-------------------|---------------------------|
| **Descoberta** | Identifica necessidade de solicitar apoio da Assessoria de Inclusão para um novo aluno com deficiência visual | "Preciso de orientação especializada para garantir que este aluno tenha uma boa experiência" | Preocupação, responsabilidade | Email institucional, conversa com colegas | Tornar mais visível como acessar o sistema de solicitação |
| **Acesso** | Loga no sistema usando credenciais institucionais | "Espero lembrar minha senha" | Ligeira ansiedade | Tela de login | Implementar login integrado com outros sistemas do CPS |
| **Navegação** | Procura a opção para registrar nova solicitação | "Onde está a opção para criar uma solicitação?" | Concentração, possível frustração | Menu de navegação, dashboard | Destacar visualmente as ações mais frequentes dos coordenadores |
| **Registro** | Preenche formulário com dados do aluno, tipo de necessidade e descrição detalhada | "Estou fornecendo todas as informações relevantes?" | Foco, preocupação com precisão | Formulário de solicitação | Incluir dicas contextuais sobre quais informações são mais úteis para cada tipo de caso |
| **Submissão** | Anexa documentos relevantes e envia a solicitação | "Estes documentos são suficientes?" | Esperança, alívio parcial | Botão de submissão, confirmação | Fornecer feedback claro sobre documentos recomendados para cada tipo de necessidade |
| **Espera** | Aguarda análise e resposta da Assessoria | "Quanto tempo vai demorar?" | Incerteza, impaciência | Notificações por email, status no sistema | Adicionar estimativa de tempo de resposta baseada em casos similares |
| **Acompanhamento** | Verifica periodicamente o status da solicitação | "Houve algum progresso no caso?" | Curiosidade, possível ansiedade | Página de status, notificações | Implementar sistema proativo de notificações sobre atualizações |
| **Interação** | Responde a pedidos de informações adicionais | "Espero ter todas as informações que eles precisam" | Engajamento, desejo de ajudar | Formulário de atualização, mensagens | Permitir comunicação direta com o técnico responsável |
| **Conclusão** | Recebe notificação de conclusão do atendimento | "A solução vai realmente funcionar?" | Satisfação, mas também ansiedade sobre implementação | Notificação, relatório de conclusão | Incluir instruções claras de próximos passos para implementação |
| **Avaliação** | Avalia o atendimento recebido | "O atendimento realmente resolveu o problema?" | Reflexivo, crítico | Formulário de feedback | Mostrar como o feedback contribui para a melhoria do serviço |

#### Insights e Recomendações
- Implementar uma visualização clara do progresso do atendimento (similar a rastreamento de encomendas)
- Enviar notificações proativas sobre atualizações do caso, não apenas em mudanças de status
- Criar uma biblioteca de casos similares para consulta, respeitando a privacidade dos dados

### Jornada 2: Gestão de Atendimentos

**Persona**: Marina Gomes (Gestor da Assessoria de Inclusão)  
**Objetivo**: Gerenciar os atendimentos em andamento e distribuir tarefas entre a equipe técnica

#### Fases da Jornada

| Fase | Ações | Pensamentos | Sentimentos | Pontos de Contato | Oportunidades de Melhoria |
|------|-------|-------------|-------------|-------------------|---------------------------|
| **Início do Dia** | Acessa o sistema e verifica o dashboard principal | "Quais são as prioridades de hoje?" | Preparação mental, organização | Dashboard gerencial | Personalizar dashboard conforme preferências individuais |
| **Triagem** | Revisa novas solicitações de atendimento | "Quais casos são mais urgentes?" | Foco, análise | Lista de solicitações pendentes | Destacar automaticamente casos prioritários conforme critérios predefinidos |
| **Classificação** | Categoriza os atendimentos por tipo e prioridade | "Este caso precisa de atenção imediata" | Concentração, responsabilidade | Formulário de categorização | Implementar sugestão automática de categorização baseada em palavras-chave |
| **Atribuição** | Distribui atendimentos entre os técnicos disponíveis | "Quem tem a expertise adequada e disponibilidade?" | Decisão, preocupação com equilíbrio | Tela de atribuição de responsáveis | Mostrar carga atual de trabalho e especialidades de cada técnico |
| **Monitoramento** | Acompanha o progresso dos atendimentos em andamento | "Os casos estão progredindo adequadamente?" | Vigilância, possível preocupação | Painel de controle, filtros | Implementar códigos de cores para status e alertas para atrasos |
| **Intervenção** | Toma ações em casos que apresentam problemas | "Preciso realocar recursos ou redefinir prioridades" | Resolução de problemas, pressão | Detalhes do atendimento, histórico | Facilitar a reatribuição ou ajuste de prazos com menos cliques |
| **Comunicação** | Interage com coordenadores ou técnicos sobre casos específicos | "Preciso esclarecer alguns pontos deste caso" | Colaboração, clareza | Sistema de mensagens, comentários | Integrar comunicação no contexto específico de cada atendimento |
| **Aprovação** | Revisa e aprova atendimentos concluídos | "A solução proposta é adequada?" | Avaliação crítica | Tela de aprovação | Incluir checklist de verificação de qualidade |
| **Análise** | Examina estatísticas e tendências | "Quais padrões estão emergindo?" | Curiosidade analítica | Relatórios e gráficos | Permitir drill-down em dados para análises mais profundas |
| **Planejamento** | Ajusta processos e recursos com base nas análises | "Como podemos melhorar nossa eficiência?" | Estratégico, prospectivo | Configurações do sistema | Incluir ferramentas de simulação para testar diferentes alocações de recursos |

#### Insights e Recomendações
- Criar um sistema de pontuação de complexidade para equilibrar melhor a distribuição de trabalho
- Implementar visualizações de gargalos no fluxo de trabalho
- Desenvolver alertas inteligentes que prevejam potenciais atrasos antes que ocorram

### Jornada 3: Execução de Atendimento

**Persona**: Rafael Menezes (Técnico da Assessoria de Inclusão)  
**Objetivo**: Receber, trabalhar e concluir um atendimento atribuído

#### Fases da Jornada

| Fase | Ações | Pensamentos | Sentimentos | Pontos de Contato | Oportunidades de Melhoria |
|------|-------|-------------|-------------|-------------------|---------------------------|
| **Recebimento** | Recebe notificação de novo atendimento atribuído | "Mais um caso para minha lista" | Responsabilidade, curiosidade | Notificação, email | Fornecer contexto inicial no próprio alerta |
| **Análise Inicial** | Revisa os detalhes do caso atribuído | "Tenho todas as informações necessárias?" | Foco, investigação | Página de detalhes do atendimento | Permitir anotações privadas para reflexões iniciais |
| **Planejamento** | Define abordagem e etapas para o atendimento | "Qual é a melhor maneira de resolver este caso?" | Estratégico, criativo | Ferramenta de planejamento | Incluir modelos de planos de ação para casos comuns |
| **Coleta de Informações** | Solicita informações adicionais quando necessário | "Preciso de mais detalhes para compreender completamente" | Investigativo, meticuloso | Formulário de solicitação | Tornar a comunicação mais direta e menos formal quando apropriado |
| **Pesquisa** | Consulta o repositório de materiais e casos anteriores | "Já resolvemos algo parecido antes?" | Investigativo, colaborativo | Repositório de recursos | Melhorar o sistema de busca com filtros mais precisos |
| **Elaboração da Solução** | Desenvolve recomendações ou soluções para o caso | "Esta solução será viável no contexto da unidade?" | Criativo, pragmático | Editor de documentos | Fornecer templates adaptáveis para diferentes tipos de solução |
| **Registro de Ações** | Documenta todas as ações e decisões tomadas | "Estou registrando tudo de forma clara?" | Meticuloso, responsável | Formulário de registro de ações | Implementar registro de voz para anotações rápidas |
| **Consultoria** | Discute casos complexos com colegas quando necessário | "Preciso de uma segunda opinião sobre esta abordagem" | Colaborativo, aberto | Chat interno, marcação de colegas | Facilitar consultas rápidas a especialistas específicos |
| **Implementação** | Executa ou orienta a implementação da solução | "Como posso garantir que a solução seja implementada corretamente?" | Prático, focado | Documentação de orientações | Incluir checklists de implementação para a unidade |
| **Conclusão** | Finaliza o atendimento e prepara documentação de encerramento | "Documentei adequadamente para referência futura?" | Realização, alívio | Formulário de conclusão | Automatizar parte da documentação com base em registros anteriores |
| **Acompanhamento** | Define e agenda atividades de acompanhamento quando necessário | "Preciso verificar se a solução está funcionando a longo prazo" | Comprometido, responsável | Agendamento de follow-up | Criar lembretes automáticos para verificações periódicas |

#### Insights e Recomendações
- Implementar ferramenta de gerenciamento de tempo para melhor planejamento das atividades diárias
- Criar biblioteca pessoal de soluções frequentemente utilizadas
- Desenvolver sistema de reconhecimento para técnicos que resolvem casos complexos ou compartilham conhecimentos úteis

### Jornada 4: Geração de Relatórios

**Persona**: Marina Gomes (Gestor da Assessoria de Inclusão)  
**Objetivo**: Criar um relatório trimestral de atividades da Assessoria para apresentação à diretoria do CPS

#### Fases da Jornada

| Fase | Ações | Pensamentos | Sentimentos | Pontos de Contato | Oportunidades de Melhoria |
|------|-------|-------------|-------------|-------------------|---------------------------|
| **Planejamento** | Define quais informações serão necessárias para o relatório | "Quais dados são mais relevantes para a diretoria?" | Estratégico, analítico | Notas pessoais, requisitos da diretoria | Oferecer templates de relatórios para diferentes finalidades |
| **Acesso** | Navega até o módulo de relatórios | "Espero que seja fácil encontrar os dados necessários" | Expectativa, possível apreensão | Menu de navegação | Destacar o acesso a relatórios no dashboard principal |
| **Seleção de Parâmetros** | Define período, tipos de atendimento e outras variáveis | "Estou selecionando os parâmetros corretos?" | Concentração, precisão | Formulário de parâmetros | Salvar configurações frequentemente usadas |
| **Geração** | Executa a geração do relatório | "Quanto tempo isso vai demorar?" | Impaciência, antecipação | Botão de geração, indicador de progresso | Mostrar estimativa de tempo para relatórios mais complexos |
| **Revisão** | Examina os resultados preliminares | "Os números parecem corretos? Falta algo?" | Análise crítica, atenção aos detalhes | Visualização preliminar | Destacar automaticamente anomalias ou tendências significativas |
| **Ajustes** | Refina parâmetros ou adiciona filtros adicionais | "Preciso ver estes dados de uma forma diferente" | Iterativo, perfeccionista | Opções de filtro, ajuste de parâmetros | Permitir ajustes sem precisar regenerar todo o relatório |
| **Personalização** | Adiciona gráficos, destaca informações importantes | "Como posso apresentar isso da maneira mais clara?" | Criativo, comunicativo | Editor de relatórios | Oferecer sugestões de visualizações com base no tipo de dados |
| **Exportação** | Seleciona formato e exporta o documento final | "Qual formato é melhor para apresentação?" | Decisão, praticidade | Opções de exportação | Pré-visualizar como ficará o relatório em cada formato |
| **Compartilhamento** | Distribui o relatório para os interessados | "Todos os destinatários relevantes estão incluídos?" | Responsabilidade, conclusão | Opções de compartilhamento | Criar listas de distribuição salvas para diferentes tipos de relatórios |
| **Apresentação** | Utiliza o relatório em reuniões ou apresentações | "Estas informações comunicam efetivamente nosso trabalho?" | Confiança, orgulho | Apresentação, discussão | Incluir notas explicativas automáticas para contextualizar os dados |

#### Insights e Recomendações
- Implementar dashboards interativos que permitam explorar os dados em tempo real durante apresentações
- Criar uma biblioteca de relatórios anteriores facilmente acessível
- Desenvolver assistente de IA para sugerir insights baseados nos dados do relatório

### Jornada 5: Gestão do Repositório de Materiais

**Persona**: Rafael Menezes (Técnico da Assessoria de Inclusão)  
**Objetivo**: Adicionar um novo material de referência ao repositório e organizar recursos existentes

#### Fases da Jornada

| Fase | Ações | Pensamentos | Sentimentos | Pontos de Contato | Oportunidades de Melhoria |
|------|-------|-------------|-------------|-------------------|---------------------------|
| **Preparação** | Seleciona e organiza o material a ser incluído | "Este material será útil para outros casos similares" | Colaborativo, útil | Arquivos locais | Oferecer dicas sobre formatos e tamanhos ideais de arquivos |
| **Acesso** | Navega até o módulo de repositório | "Onde está a opção para adicionar novos materiais?" | Determinação, foco | Menu de navegação | Criar atalhos para funções frequentemente utilizadas |
| **Cadastro** | Preenche metadados do material (título, descrição, categorias) | "Quais tags ajudarão outros a encontrar este material?" | Meticuloso, organizado | Formulário de cadastro | Sugerir tags e categorias baseadas no conteúdo detectado |
| **Upload** | Carrega o arquivo para o sistema | "Espero que não demore muito" | Paciente, ansioso | Interface de upload | Mostrar progresso detalhado e permitir continuar trabalhando durante o upload |
| **Categorização** | Define permissões de acesso e relacionamentos com outros materiais | "Quem deve ter acesso a este material?" | Responsável, criterioso | Configurações de acesso | Oferecer templates de permissões para diferentes tipos de material |
| **Submissão** | Envia o material para aprovação | "Será que incluí todas as informações necessárias?" | Expectante, responsável | Botão de submissão | Incluir checklist de verificação pré-submissão |
| **Espera** | Aguarda aprovação do material | "Quanto tempo levará para ser aprovado?" | Paciente, ansioso | Status de aprovação | Mostrar estimativa de tempo baseada em submissões anteriores |
| **Ajustes** | Realiza correções caso solicitadas pelo aprovador | "Como posso melhorar este material?" | Receptivo, colaborativo | Feedback do revisor | Permitir edição direta ao invés de reenvio completo |
| **Organização** | Revisa e reorganiza materiais existentes relacionados | "Como posso melhorar a organização da nossa base de conhecimento?" | Organizado, sistemático | Interface de gerenciamento | Implementar visualização de relacionamentos entre materiais |
| **Divulgação** | Comunica aos colegas sobre o novo material disponível | "Quem poderia se beneficiar deste recurso?" | Colaborativo, prestativo | Ferramentas de comunicação | Criar sistema de recomendação automática para usuários relevantes |

#### Insights e Recomendações
- Implementar sistema de versionamento para materiais que são atualizados periodicamente
- Criar dashboards de uso que mostrem quais materiais são mais acessados e por quem
- Desenvolver um sistema de revisão periódica para identificar materiais obsoletos

## Considerações de Design

Com base nas jornadas mapeadas, identificamos os seguintes princípios de design que devem guiar o desenvolvimento do sistema:

1. **Simplicidade e Eficiência**: Os usuários frequentemente estão sob pressão de tempo e precisam concluir tarefas rapidamente.

2. **Contextualização**: Informações e controles devem estar disponíveis no contexto em que são necessários, minimizando a navegação entre telas.

3. **Transparência**: Os usuários precisam de visibilidade clara sobre status, próximos passos e responsabilidades.

4. **Adaptabilidade**: Diferentes perfis possuem necessidades distintas de informação e controle.

5. **Comunicação Integrada**: A comunicação entre as partes interessadas deve ser facilitada e integrada ao fluxo de trabalho.

6. **Aprendizado Progressivo**: O sistema deve ser fácil para iniciantes, mas oferecer recursos avançados para usuários experientes.

7. **Redução de Esforço Manual**: Automatizar tarefas repetitivas e oferecer assistência inteligente para decisões complexas.

## Pontos de Melhoria Identificados

A análise das jornadas de usuário revelou os seguintes pontos de melhoria que devem ser considerados durante o desenvolvimento do sistema:

1. **Notificações Contextuais**: Implementar notificações inteligentes que forneçam o contexto necessário para ação imediata, reduzindo a necessidade de navegação adicional.

2. **Personalização de Interface**: Permitir que usuários configurem dashboards e visualizações de acordo com suas necessidades específicas.

3. **Acessibilidade**: Garantir que o sistema seja utilizável por pessoas com diferentes tipos de deficiência, praticando o que a própria Assessoria de Inclusão preconiza.

4. **Suporte a Decisões**: Implementar recursos de análise e recomendação para auxiliar em decisões complexas, como atribuição de casos e definição de prioridades.

5. **Integração de Comunicação**: Centralizar as comunicações relacionadas a cada caso/atendimento, evitando fragmentação de informações em emails e outros canais.

6. **Onboarding Contextual**: Desenvolver tutoriais e dicas contextuais que auxiliem novos usuários a se familiarizarem com o sistema.

7. **Automação Inteligente**: Identificar processos repetitivos que podem ser automatizados, liberando tempo dos usuários para atividades que exigem expertise humana.