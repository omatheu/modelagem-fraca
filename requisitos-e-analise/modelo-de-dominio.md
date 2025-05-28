# Modelo de Domínio
## Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza

## Sumário
- [Introdução](#introdução)
- [Diagrama do Modelo de Domínio](#diagrama-do-modelo-de-domínio)
- [Descrição das Entidades](#descrição-das-entidades)
- [Relacionamentos](#relacionamentos)
- [Regras de Negócio](#regras-de-negócio)
- [Glossário de Termos](#glossário-de-termos)

## Introdução

Este documento apresenta o modelo de domínio para o Sistema de Gestão da Assessoria de Inclusão do Centro Paula Souza (SGAI). O modelo de domínio é uma representação visual e conceitual das entidades significativas no contexto do sistema, seus atributos e relações. Ele serve como um vocabulário comum entre stakeholders técnicos e não técnicos, permitindo um entendimento compartilhado dos principais elementos do sistema.

O modelo foi desenvolvido com base nas necessidades identificadas no Documento de Visão e Escopo, bem como nos requisitos detalhados no documento de Especificação de Requisitos de Software.

## Diagrama do Modelo de Domínio

```
+---------------+        +----------------+        +-----------------+
|    Usuário    |<-------| Perfil Acesso |------->|     Unidade     |
+---------------+        +----------------+        +-----------------+
       |                                                  |
       |                                                  |
       v                                                  v
+---------------+        +----------------+        +-----------------+
|    Tarefa     |<-------|  Atendimento   |------->| Tipo Necessidade|
+---------------+        +----------------+        +-----------------+
                                |
                                |
           +-------------------+|+------------------+
           |                    |                   |
           v                    v                   v
+---------------+     +----------------+     +-----------------+
|   Material    |     |  Profissional  |     |   Adaptação    |
|   Recurso     |     | Especializado  |     |   Curricular   |
+---------------+     +----------------+     +-----------------+
```

## Descrição das Entidades

### Usuário
Representa qualquer pessoa que interage com o sistema, incluindo administradores, gestores e técnicos da Assessoria de Inclusão e coordenadores de unidades.

**Atributos principais:**
- ID (identificador único)
- Nome
- E-mail institucional
- Senha (criptografada)
- Cargo/Função
- Data de cadastro
- Status (ativo, inativo)
- Perfil de acesso

### Perfil de Acesso
Define os diferentes níveis de permissão que os usuários podem ter no sistema.

**Atributos principais:**
- ID
- Nome (Administrador, Gestor, Técnico, Coordenador)
- Descrição
- Permissões associadas

### Unidade
Representa as instituições de ensino do CPS (Etecs/Fatecs/Classes Descentralizadas).

**Atributos principais:**
- ID
- Nome
- Tipo (Etec, Fatec, Classe Descentralizada)
- Endereço
- Contato principal
- Recursos de acessibilidade disponíveis
- Coordenadores

### Atendimento
Representa os casos atendidos pela Assessoria de Inclusão.

**Atributos principais:**
- Número de protocolo
- Data de abertura
- Data de última atualização
- Status (aberto, em andamento, concluído, cancelado)
- Descrição
- Unidade solicitante
- Responsável atual
- Tipo de necessidade
- Prioridade
- Histórico de alterações

### Tipo de Necessidade
Categoriza os diferentes tipos de necessidades educacionais especiais.

**Atributos principais:**
- ID
- Nome (física, visual, auditiva, intelectual, múltipla, etc.)
- Descrição
- Protocolos de atendimento sugeridos

### Tarefa
Representa as atividades específicas que precisam ser realizadas para atender um caso.

**Atributos principais:**
- ID
- Título
- Descrição
- Responsável
- Data de criação
- Prazo de conclusão
- Status (pendente, em andamento, concluída, cancelada)
- Prioridade
- Atendimento associado

### Material/Recurso
Representa documentos, guias, legislação e outros materiais relacionados à inclusão.

**Atributos principais:**
- ID
- Título
- Descrição
- Categoria
- Tags
- Data de inclusão
- Autor/Responsável
- Formato
- Local de armazenamento/URL
- Permissões de acesso

### Profissional Especializado
Representa pessoas com conhecimentos específicos em áreas relacionadas à inclusão.

**Atributos principais:**
- ID
- Nome
- Área(s) de especialização
- Unidade
- Contato
- Disponibilidade
- Experiência

### Adaptação Curricular
Registra adaptações específicas implementadas em cursos e disciplinas.

**Atributos principais:**
- ID
- Curso
- Disciplina
- Tipo de adaptação
- Descrição
- Resultados observados
- Responsável pela implementação
- Data de implementação

## Relacionamentos

### Usuário - Perfil de Acesso
- Um usuário possui exatamente um perfil de acesso
- Um perfil de acesso pode ser associado a múltiplos usuários

### Usuário - Unidade
- Um usuário pertence a uma unidade
- Uma unidade pode ter múltiplos usuários

### Usuário - Atendimento
- Um usuário pode ser responsável por múltiplos atendimentos
- Um atendimento tem exatamente um usuário responsável em um dado momento

### Usuário - Tarefa
- Um usuário pode ser responsável por múltiplas tarefas
- Uma tarefa tem exatamente um usuário responsável em um dado momento

### Unidade - Atendimento
- Uma unidade pode solicitar múltiplos atendimentos
- Um atendimento está associado a exatamente uma unidade solicitante

### Atendimento - Tipo de Necessidade
- Um atendimento pode estar relacionado a um ou mais tipos de necessidade
- Um tipo de necessidade pode estar associado a múltiplos atendimentos

### Atendimento - Tarefa
- Um atendimento pode ter múltiplas tarefas associadas
- Uma tarefa está associada a exatamente um atendimento

### Atendimento - Material/Recurso
- Um atendimento pode ter múltiplos materiais/recursos associados
- Um material/recurso pode estar associado a múltiplos atendimentos

### Atendimento - Adaptação Curricular
- Um atendimento pode resultar em múltiplas adaptações curriculares
- Uma adaptação curricular pode estar associada a um ou mais atendimentos

## Regras de Negócio

1. **RN-01**: Todo atendimento deve ser atribuído a um responsável da Assessoria de Inclusão.

2. **RN-02**: Somente usuários com perfil de Gestor ou Administrador podem atribuir responsáveis aos atendimentos.

3. **RN-03**: Coordenadores de Unidade podem registrar solicitações de atendimento apenas para suas próprias unidades.

4. **RN-04**: Toda alteração de status em um atendimento deve ser registrada no histórico, incluindo data, hora e usuário responsável pela alteração.

5. **RN-05**: Materiais e recursos sensíveis devem ter controle de acesso conforme os perfis de usuário.

6. **RN-06**: A priorização dos atendimentos deve seguir critérios estabelecidos pela Assessoria de Inclusão, considerando urgência e impacto.

7. **RN-07**: Dados pessoais de alunos com NEE devem ser tratados conforme as diretrizes da LGPD.

8. **RN-08**: Relatórios estatísticos não devem expor informações que permitam identificação individual de alunos.

9. **RN-09**: Notificações automáticas devem ser enviadas quando um atendimento está próximo do prazo de conclusão estabelecido.

10. **RN-10**: Tarefas concluídas não podem ser reabertas após 30 dias de seu fechamento.

## Glossário de Termos

**Assessoria de Inclusão**: Departamento do Centro Paula Souza responsável por garantir a inclusão e acessibilidade para alunos com necessidades educacionais especiais.

**Atendimento**: Registro de um caso ou solicitação que requer intervenção da Assessoria de Inclusão.

**CPS (Centro Paula Souza)**: Autarquia do Governo do Estado de São Paulo responsável pela administração das Etecs e Fatecs.

**Etec**: Escola Técnica Estadual.

**Fatec**: Faculdade de Tecnologia Estadual.

**LGPD**: Lei Geral de Proteção de Dados - legislação brasileira que regula o tratamento de dados pessoais.

**NEE**: Necessidades Educacionais Especiais - termo que se refere às necessidades específicas de aprendizagem de alunos com deficiência ou dificuldades de aprendizagem.

**PcD**: Pessoa com Deficiência.

**PNE**: Pessoa com Necessidades Especiais.

**SGAI**: Sistema de Gestão da Assessoria de Inclusão - nome do sistema sendo desenvolvido.

**Tarefa**: Atividade específica a ser realizada como parte da resolução de um atendimento.

**Tipo de Necessidade**: Classificação das necessidades especiais (física, visual, auditiva, intelectual, múltipla, etc.).

**WCAG**: Web Content Accessibility Guidelines - diretrizes de acessibilidade para conteúdo web.