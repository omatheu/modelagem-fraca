# Modelo Lógico e Físico de Banco de Dados
## Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza

## Sumário
- [Introdução](#introdução)
- [Modelo Lógico](#modelo-lógico)
  - [Diagrama Lógico](#diagrama-lógico)
  - [Normalização Aplicada](#normalização-aplicada)
  - [Tipos de Dados Padronizados](#tipos-de-dados-padronizados)
  - [Domínios e Restrições](#domínios-e-restrições)
- [Modelo Físico](#modelo-físico)
  - [Scripts DDL](#scripts-ddl)
  - [Índices](#índices)
  - [Constraints](#constraints)
  - [Triggers e Funções](#triggers-e-funções)
- [Estratégias de Implementação](#estratégias-de-implementação)
  - [Particionamento](#particionamento)
  - [Otimizações para PostgreSQL](#otimizações-para-postgresql)
  - [Estratégia para Dados Históricos](#estratégia-para-dados-históricos)
- [Migração e Versionamento](#migração-e-versionamento)
  - [Estratégia de Migração](#estratégia-de-migração)
  - [Versionamento do Banco](#versionamento-do-banco)
- [Segurança no Nível de Banco de Dados](#segurança-no-nível-de-banco-de-dados)
- [Considerações Operacionais](#considerações-operacionais)

## Introdução

Este documento apresenta a implementação detalhada do banco de dados que suportará o Sistema de Gestão da Assessoria de Inclusão do Centro Paula Souza. Ele faz a transição do modelo conceitual/entidade-relacionamento para modelos lógico e físico concretos, fornecendo os detalhes necessários para implementação e operação do banco de dados.

O modelo lógico traduz o diagrama entidade-relacionamento em um conjunto de relações normalizadas, definindo claramente a estrutura de tabelas, seus relacionamentos e restrições, independente de qualquer SGBD específico. O modelo físico, por sua vez, define exatamente como essas estruturas serão implementadas no PostgreSQL, incluindo scripts DDL, índices, otimizações específicas e outras considerações técnicas.

Enquanto o diagrama entidade-relacionamento já apresentado focou na representação conceitual das entidades e seus relacionamentos, este documento se aprofunda nos aspectos técnicos de implementação do banco de dados, incluindo detalhes de otimização, particionamento, indexação e estratégias operacionais.

## Modelo Lógico

O modelo lógico apresenta a estrutura normalizada das tabelas do sistema, definindo formalmente as relações, atributos, chaves e relacionamentos, sem depender de características específicas de um SGBD.

### Diagrama Lógico

O diagrama lógico já foi apresentado no documento de Diagrama Entidade-Relacionamento. Este modelo está predominantemente na 3ª Forma Normal (3FN), com algumas desnormalizações estratégicas para otimização de consultas frequentes.

As principais relações (tabelas) do modelo lógico são:

1. **USUARIO** (usuario_id, nome, email, senha_hash, perfil_id, data_cadastro, ultimo_acesso, status)
2. **PERFIL** (perfil_id, nome, descricao, permissoes)
3. **COORDENADOR** (usuario_id, unidade_id)
4. **TECNICO** (usuario_id, especialidade, disponibilidade)
5. **UNIDADE** (unidade_id, tipo, nome, cidade, endereco, telefone, email)
6. **TIPO_NECESSIDADE** (tipo_necessidade_id, nome, descricao, recomendacoes)
7. **ATENDIMENTO** (atendimento_id, protocolo, tipo_necessidade_id, unidade_id, responsavel_id, data_abertura, ultima_atualizacao, status, descricao, prioridade)
8. **HISTORICO_ATENDIMENTO** (historico_id, atendimento_id, usuario_id, data_hora, acao_realizada, estado_anterior, estado_novo, observacoes)
9. **TAREFA** (tarefa_id, atendimento_id, responsavel_id, titulo, descricao, data_criacao, prazo, status, prioridade)
10. **MATERIAL** (material_id, titulo, descricao, categoria_id, autor_id, formato, data_inclusao, versao, arquivo_path, tags, estado)
11. **CATEGORIA_MATERIAL** (categoria_id, nome, descricao)
12. **HISTORICO_MATERIAL** (historico_id, material_id, usuario_id, data_hora, acao, versao_anterior, versao_nova, observacoes)
13. **RELATORIO** (relatorio_id, titulo, descricao, tipo, criador_id, campos_disponiveis, predefinido)
14. **CONFIG_RELATORIO** (config_id, relatorio_id, parametros, data_ultima_execucao, agendamento)
15. **NOTIFICACAO** (notificacao_id, destinatario_id, tipo, titulo, conteudo, data_envio, lida)
16. **LOG** (log_id, data_hora, usuario_id, acao, entidade, entidade_id, detalhes)

### Normalização Aplicada

1. **Primeira Forma Normal (1FN)**: Todas as tabelas estão na 1FN, com valores atômicos e chaves primárias definidas.

2. **Segunda Forma Normal (2FN)**: Todas as tabelas estão na 2FN, com atributos não-chave dependendo completamente da chave primária.

3. **Terceira Forma Normal (3FN)**: A maioria das tabelas está na 3FN, eliminando dependências transitivas. Exemplos:
   - Informações de perfil foram separadas da tabela USUARIO
   - Tipos de necessidade foram separados da tabela ATENDIMENTO
   - Categorias de materiais foram separadas da tabela MATERIAL

4. **Exceções à 3FN (Desnormalizações Estratégicas)**:
   - Armazenamento de "ultima_atualizacao" em ATENDIMENTO para evitar consultas constantes ao histórico
   - Armazenamento de campos derivados ou calculados para relatórios frequentes
   - Uso de arrays para tags em MATERIAL para facilitar buscas por palavras-chave

### Tipos de Dados Padronizados

| Tipo de Dado        | Uso                           | Tipo no PostgreSQL      |
|---------------------|-------------------------------|-------------------------|
| Identificadores     | Chaves primárias              | SERIAL ou INTEGER       |
| Textos curtos       | Nomes, títulos                | VARCHAR(100)            |
| Textos longos       | Descrições, observações       | TEXT                    |
| Data e hora         | Timestamps                    | TIMESTAMP WITH TIME ZONE|
| Booleanos           | Flags, status binários        | BOOLEAN                 |
| Enumerados          | Status, prioridades           | VARCHAR ou tabelas de domínio |
| JSON                | Dados flexíveis, configurações| JSONB                   |
| Arrays              | Listas como tags              | VARCHAR[] ou TEXT[]     |
| Referências         | Chaves estrangeiras           | INTEGER                 |

### Domínios e Restrições

1. **Status de Atendimento**:
   - "Aberto" - Caso registrado, aguardando triagem
   - "Em Análise" - Em avaliação pela equipe
   - "Em Atendimento" - Atendimento em andamento
   - "Aguardando Informações" - Aguardando dados adicionais
   - "Concluído" - Atendimento finalizado com sucesso
   - "Cancelado" - Atendimento cancelado

2. **Prioridade**:
   - "Baixa" - Pode ser tratado quando houver disponibilidade
   - "Média" - Requer atenção em tempo hábil
   - "Alta" - Requer atenção prioritária
   - "Crítica" - Requer atenção imediata

3. **Status de Tarefa**:
   - "Pendente" - Ainda não iniciada
   - "Em Andamento" - Trabalho iniciado
   - "Concluída" - Finalizada com sucesso
   - "Bloqueada" - Impedida por algum motivo
   - "Cancelada" - Não será executada

4. **Estado de Material**:
   - "Rascunho" - Em preparação
   - "Revisão" - Aguardando aprovação
   - "Publicado" - Disponível para consulta
   - "Arquivado" - Não mais em uso ativo

## Modelo Físico

O modelo físico detalha a implementação concreta do banco de dados no PostgreSQL, incluindo scripts de criação, tipos de dados específicos, índices e outras otimizações.

### Scripts DDL

```sql
-- Criação das tabelas principais

-- Perfis de usuário
CREATE TABLE perfil (
    perfil_id SERIAL PRIMARY KEY,
    nome VARCHAR(50) NOT NULL UNIQUE,
    descricao TEXT NOT NULL,
    permissoes JSONB NOT NULL DEFAULT '{}'
);

-- Usuários do sistema
CREATE TABLE usuario (
    usuario_id SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    senha_hash VARCHAR(255) NOT NULL,
    perfil_id INTEGER NOT NULL,
    data_cadastro TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT CURRENT_TIMESTAMP,
    ultimo_acesso TIMESTAMP WITH TIME ZONE,
    status VARCHAR(10) NOT NULL DEFAULT 'Ativo' CHECK (status IN ('Ativo', 'Inativo')),
    FOREIGN KEY (perfil_id) REFERENCES perfil(perfil_id)
);

-- Unidades educacionais
CREATE TABLE unidade (
    unidade_id SERIAL PRIMARY KEY,
    tipo VARCHAR(10) NOT NULL CHECK (tipo IN ('Etec', 'Fatec')),
    nome VARCHAR(100) NOT NULL,
    cidade VARCHAR(100) NOT NULL,
    endereco TEXT NOT NULL,
    telefone VARCHAR(20),
    email VARCHAR(100) NOT NULL,
    UNIQUE (tipo, nome, cidade)
);

-- Coordenadores de unidade (especialização de usuário)
CREATE TABLE coordenador (
    usuario_id INTEGER PRIMARY KEY,
    unidade_id INTEGER NOT NULL,
    FOREIGN KEY (usuario_id) REFERENCES usuario(usuario_id),
    FOREIGN KEY (unidade_id) REFERENCES unidade(unidade_id)
);

-- Técnicos da assessoria (especialização de usuário)
CREATE TABLE tecnico (
    usuario_id INTEGER PRIMARY KEY,
    especialidade VARCHAR(100) NOT NULL,
    disponibilidade TEXT,
    FOREIGN KEY (usuario_id) REFERENCES usuario(usuario_id)
);

-- Tipos de necessidade educacional
CREATE TABLE tipo_necessidade (
    tipo_necessidade_id SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL UNIQUE,
    descricao TEXT NOT NULL,
    recomendacoes TEXT
);

-- Tabela de domínio para status de atendimento
CREATE TABLE status_atendimento (
    status_id SERIAL PRIMARY KEY,
    nome VARCHAR(30) NOT NULL UNIQUE,
    descricao TEXT NOT NULL,
    permite_transicao_para INTEGER[] -- IDs dos status permitidos após este
);

-- Tabela de domínio para prioridades
CREATE TABLE prioridade (
    prioridade_id SERIAL PRIMARY KEY,
    nome VARCHAR(20) NOT NULL UNIQUE,
    nivel INTEGER NOT NULL,
    descricao TEXT NOT NULL
);

-- Atendimentos
CREATE TABLE atendimento (
    atendimento_id SERIAL PRIMARY KEY,
    protocolo VARCHAR(20) NOT NULL UNIQUE,
    tipo_necessidade_id INTEGER NOT NULL,
    unidade_id INTEGER NOT NULL,
    responsavel_id INTEGER,
    data_abertura TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT CURRENT_TIMESTAMP,
    ultima_atualizacao TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT CURRENT_TIMESTAMP,
    status_id INTEGER NOT NULL,
    descricao TEXT NOT NULL,
    prioridade_id INTEGER NOT NULL,
    FOREIGN KEY (tipo_necessidade_id) REFERENCES tipo_necessidade(tipo_necessidade_id),
    FOREIGN KEY (unidade_id) REFERENCES unidade(unidade_id),
    FOREIGN KEY (responsavel_id) REFERENCES tecnico(usuario_id),
    FOREIGN KEY (status_id) REFERENCES status_atendimento(status_id),
    FOREIGN KEY (prioridade_id) REFERENCES prioridade(prioridade_id)
);

-- Histórico de atendimentos
CREATE TABLE historico_atendimento (
    historico_id SERIAL PRIMARY KEY,
    atendimento_id INTEGER NOT NULL,
    usuario_id INTEGER NOT NULL,
    data_hora TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT CURRENT_TIMESTAMP,
    acao_realizada VARCHAR(100) NOT NULL,
    status_anterior_id INTEGER,
    status_novo_id INTEGER,
    observacoes TEXT,
    FOREIGN KEY (atendimento_id) REFERENCES atendimento(atendimento_id),
    FOREIGN KEY (usuario_id) REFERENCES usuario(usuario_id),
    FOREIGN KEY (status_anterior_id) REFERENCES status_atendimento(status_id),
    FOREIGN KEY (status_novo_id) REFERENCES status_atendimento(status_id)
);

-- Tarefas
CREATE TABLE tarefa (
    tarefa_id SERIAL PRIMARY KEY,
    atendimento_id INTEGER NOT NULL,
    responsavel_id INTEGER,
    titulo VARCHAR(100) NOT NULL,
    descricao TEXT NOT NULL,
    data_criacao TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT CURRENT_TIMESTAMP,
    prazo TIMESTAMP WITH TIME ZONE,
    status VARCHAR(20) NOT NULL DEFAULT 'Pendente' CHECK (status IN ('Pendente', 'Em Andamento', 'Concluída', 'Bloqueada', 'Cancelada')),
    prioridade_id INTEGER NOT NULL,
    FOREIGN KEY (atendimento_id) REFERENCES atendimento(atendimento_id) ON DELETE CASCADE,
    FOREIGN KEY (responsavel_id) REFERENCES usuario(usuario_id),
    FOREIGN KEY (prioridade_id) REFERENCES prioridade(prioridade_id)
);

-- Categorias de materiais
CREATE TABLE categoria_material (
    categoria_id SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL UNIQUE,
    descricao TEXT NOT NULL
);

-- Materiais
CREATE TABLE material (
    material_id SERIAL PRIMARY KEY,
    titulo VARCHAR(200) NOT NULL,
    descricao TEXT NOT NULL,
    categoria_id INTEGER NOT NULL,
    autor_id INTEGER NOT NULL,
    formato VARCHAR(20) NOT NULL,
    data_inclusao TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT CURRENT_TIMESTAMP,
    versao INTEGER NOT NULL DEFAULT 1,
    arquivo_path VARCHAR(255) NOT NULL,
    tags TEXT[] DEFAULT '{}',
    estado VARCHAR(20) NOT NULL DEFAULT 'Rascunho' CHECK (estado IN ('Rascunho', 'Revisão', 'Publicado', 'Arquivado')),
    FOREIGN KEY (categoria_id) REFERENCES categoria_material(categoria_id),
    FOREIGN KEY (autor_id) REFERENCES usuario(usuario_id)
);

-- Histórico de materiais
CREATE TABLE historico_material (
    historico_id SERIAL PRIMARY KEY,
    material_id INTEGER NOT NULL,
    usuario_id INTEGER NOT NULL,
    data_hora TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT CURRENT_TIMESTAMP,
    acao VARCHAR(100) NOT NULL,
    versao_anterior INTEGER,
    versao_nova INTEGER,
    observacoes TEXT,
    FOREIGN KEY (material_id) REFERENCES material(material_id) ON DELETE CASCADE,
    FOREIGN KEY (usuario_id) REFERENCES usuario(usuario_id)
);

-- Relatórios
CREATE TABLE relatorio (
    relatorio_id SERIAL PRIMARY KEY,
    titulo VARCHAR(100) NOT NULL,
    descricao TEXT NOT NULL,
    tipo VARCHAR(50) NOT NULL,
    criador_id INTEGER NOT NULL,
    campos_disponiveis JSONB NOT NULL,
    predefinido BOOLEAN NOT NULL DEFAULT false,
    FOREIGN KEY (criador_id) REFERENCES usuario(usuario_id)
);

-- Configurações de relatórios
CREATE TABLE config_relatorio (
    config_id SERIAL PRIMARY KEY,
    relatorio_id INTEGER NOT NULL,
    parametros JSONB NOT NULL,
    data_ultima_execucao TIMESTAMP WITH TIME ZONE,
    agendamento TEXT,
    FOREIGN KEY (relatorio_id) REFERENCES relatorio(relatorio_id) ON DELETE CASCADE
);

-- Notificações
CREATE TABLE notificacao (
    notificacao_id SERIAL PRIMARY KEY,
    destinatario_id INTEGER NOT NULL,
    tipo VARCHAR(50) NOT NULL,
    titulo VARCHAR(100) NOT NULL,
    conteudo TEXT NOT NULL,
    data_envio TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT CURRENT_TIMESTAMP,
    lida BOOLEAN NOT NULL DEFAULT false,
    FOREIGN KEY (destinatario_id) REFERENCES usuario(usuario_id)
);

-- Logs do sistema
CREATE TABLE log (
    log_id BIGSERIAL PRIMARY KEY,
    data_hora TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT CURRENT_TIMESTAMP,
    usuario_id INTEGER,
    acao VARCHAR(100) NOT NULL,
    entidade VARCHAR(50) NOT NULL,
    entidade_id INTEGER,
    detalhes JSONB,
    FOREIGN KEY (usuario_id) REFERENCES usuario(usuario_id)
);
```

### Índices

```sql
-- Índices para usuários
CREATE INDEX idx_usuario_email ON usuario(email);
CREATE INDEX idx_usuario_perfil ON usuario(perfil_id);
CREATE INDEX idx_usuario_status ON usuario(status);

-- Índices para atendimentos
CREATE INDEX idx_atendimento_protocolo ON atendimento(protocolo);
CREATE INDEX idx_atendimento_unidade ON atendimento(unidade_id);
CREATE INDEX idx_atendimento_responsavel ON atendimento(responsavel_id);
CREATE INDEX idx_atendimento_status ON atendimento(status_id);
CREATE INDEX idx_atendimento_tipo ON atendimento(tipo_necessidade_id);
CREATE INDEX idx_atendimento_data ON atendimento(data_abertura);
CREATE INDEX idx_atendimento_composto ON atendimento(unidade_id, status_id, prioridade_id);

-- Índices para histórico
CREATE INDEX idx_historico_atendimento ON historico_atendimento(atendimento_id);
CREATE INDEX idx_historico_usuario ON historico_atendimento(usuario_id);
CREATE INDEX idx_historico_data ON historico_atendimento(data_hora);

-- Índices para tarefas
CREATE INDEX idx_tarefa_atendimento ON tarefa(atendimento_id);
CREATE INDEX idx_tarefa_responsavel ON tarefa(responsavel_id);
CREATE INDEX idx_tarefa_status ON tarefa(status);
CREATE INDEX idx_tarefa_prazo ON tarefa(prazo);
CREATE INDEX idx_tarefa_responsavel_status ON tarefa(responsavel_id, status);

-- Índices para materiais
CREATE INDEX idx_material_categoria ON material(categoria_id);
CREATE INDEX idx_material_autor ON material(autor_id);
CREATE INDEX idx_material_estado ON material(estado);
CREATE INDEX idx_material_data ON material(data_inclusao);
CREATE INDEX idx_material_titulo_trgm ON material USING gin (titulo gin_trgm_ops);
CREATE INDEX idx_material_tags ON material USING gin (tags);

-- Índices para notificações
CREATE INDEX idx_notificacao_destinatario ON notificacao(destinatario_id);
CREATE INDEX idx_notificacao_lida ON notificacao(destinatario_id, lida);
CREATE INDEX idx_notificacao_data ON notificacao(data_envio);

-- Índices para logs
CREATE INDEX idx_log_usuario ON log(usuario_id);
CREATE INDEX idx_log_entidade ON log(entidade, entidade_id);
CREATE INDEX idx_log_data ON log(data_hora);
CREATE INDEX idx_log_acao ON log(acao);
```

### Constraints

```sql
-- Constraints adicionais (além das definidas na criação das tabelas)

-- Garante que protocolos de atendimento sigam um formato específico
ALTER TABLE atendimento ADD CONSTRAINT ck_protocolo_formato 
    CHECK (protocolo ~ '^ATD-[0-9]{6}$');

-- Validação de email institucional
ALTER TABLE usuario ADD CONSTRAINT ck_email_institucional 
    CHECK (email ~ '.*@(cps\.sp\.gov\.br|etec\.sp\.gov\.br|fatec\.sp\.gov\.br)$');

-- Garante que prazos de tarefas sejam datas futuras no momento da criação
ALTER TABLE tarefa ADD CONSTRAINT ck_tarefa_prazo_futuro 
    CHECK (prazo > data_criacao);

-- Restrição para evitar que um técnico tenha múltiplas tarefas críticas simultâneas
-- (Implementada via trigger)

-- Restrição para garantir que operações de alteração de status sigam as transições permitidas
-- (Implementada via trigger)
```

### Triggers e Funções

```sql
-- Função para gerar protocolo automático para atendimentos
CREATE OR REPLACE FUNCTION gerar_protocolo_atendimento()
RETURNS TRIGGER AS $$
BEGIN
    NEW.protocolo := 'ATD-' || to_char(NOW(), 'YYMMDD') || 
                    LPAD(CAST(nextval('seq_protocolo_atendimento') AS TEXT), 3, '0');
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE SEQUENCE seq_protocolo_atendimento START 1;

CREATE TRIGGER trg_gerar_protocolo_atendimento
BEFORE INSERT ON atendimento
FOR EACH ROW EXECUTE FUNCTION gerar_protocolo_atendimento();

-- Função para atualizar timestamp de última atualização
CREATE OR REPLACE FUNCTION atualizar_timestamp_atendimento()
RETURNS TRIGGER AS $$
BEGIN
    NEW.ultima_atualizacao := CURRENT_TIMESTAMP;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_atualizar_timestamp_atendimento
BEFORE UPDATE ON atendimento
FOR EACH ROW EXECUTE FUNCTION atualizar_timestamp_atendimento();

-- Função para registrar histórico automático quando status de atendimento é alterado
CREATE OR REPLACE FUNCTION registrar_historico_atendimento()
RETURNS TRIGGER AS $$
BEGIN
    IF OLD.status_id IS DISTINCT FROM NEW.status_id THEN
        INSERT INTO historico_atendimento(
            atendimento_id, usuario_id, acao_realizada, 
            status_anterior_id, status_novo_id, observacoes
        ) VALUES (
            NEW.atendimento_id,
            current_setting('app.current_user_id')::integer,
            'Alteração de Status',
            OLD.status_id,
            NEW.status_id,
            'Alteração automática via sistema'
        );
    END IF;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_registrar_historico_atendimento
AFTER UPDATE ON atendimento
FOR EACH ROW EXECUTE FUNCTION registrar_historico_atendimento();

-- Função para verificar transições de status válidas
CREATE OR REPLACE FUNCTION validar_transicao_status()
RETURNS TRIGGER AS $$
DECLARE
    transicoes_validas INTEGER[];
BEGIN
    -- Se for inserção, qualquer status inicial é válido
    IF TG_OP = 'INSERT' THEN
        RETURN NEW;
    END IF;
    
    -- Se não houver mudança de status, não precisa validar
    IF OLD.status_id = NEW.status_id THEN
        RETURN NEW;
    END IF;
    
    -- Busca as transições válidas para o status atual
    SELECT permite_transicao_para INTO transicoes_validas
    FROM status_atendimento
    WHERE status_id = OLD.status_id;
    
    -- Se o novo status não está nas transições válidas
    IF NOT NEW.status_id = ANY(transicoes_validas) THEN
        RAISE EXCEPTION 'Transição de status inválida de % para %', 
            OLD.status_id, NEW.status_id;
    END IF;
    
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_validar_transicao_status
BEFORE UPDATE ON atendimento
FOR EACH ROW EXECUTE FUNCTION validar_transicao_status();

-- Função para incrementar versão de material ao atualizar
CREATE OR REPLACE FUNCTION incrementar_versao_material()
RETURNS TRIGGER AS $$
BEGIN
    IF OLD.titulo != NEW.titulo OR 
       OLD.descricao != NEW.descricao OR 
       OLD.arquivo_path != NEW.arquivo_path THEN
        
        NEW.versao := OLD.versao + 1;
        
        -- Registrar no histórico
        INSERT INTO historico_material(
            material_id, usuario_id, acao, 
            versao_anterior, versao_nova, observacoes
        ) VALUES (
            NEW.material_id,
            current_setting('app.current_user_id')::integer,
            'Atualização',
            OLD.versao,
            NEW.versao,
            'Atualização automática via sistema'
        );
    END IF;
    
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_incrementar_versao_material
BEFORE UPDATE ON material
FOR EACH ROW EXECUTE FUNCTION incrementar_versao_material();

-- Função para registrar log automático
CREATE OR REPLACE FUNCTION registrar_log_operacao()
RETURNS TRIGGER AS $$
DECLARE
    id_valor INTEGER;
    operacao VARCHAR;
BEGIN
    -- Determina a operação
    IF TG_OP = 'INSERT' THEN
        operacao := 'Inserção';
        id_valor := NEW.atendimento_id; -- Ajustar conforme a tabela
    ELSIF TG_OP = 'UPDATE' THEN
        operacao := 'Atualização';
        id_valor := NEW.atendimento_id;
    ELSIF TG_OP = 'DELETE' THEN
        operacao := 'Exclusão';
        id_valor := OLD.atendimento_id;
    END IF;

    -- Insere o log
    INSERT INTO log(
        usuario_id, acao, entidade, entidade_id, detalhes
    ) VALUES (
        NULLIF(current_setting('app.current_user_id', true), '')::integer,
        operacao,
        TG_TABLE_NAME,
        id_valor,
        -- Detalhes específicos conforme a tabela
        CASE 
            WHEN TG_TABLE_NAME = 'atendimento' AND TG_OP = 'UPDATE' THEN
                jsonb_build_object(
                    'antes', jsonb_build_object(
                        'status_id', OLD.status_id,
                        'responsavel_id', OLD.responsavel_id
                    ),
                    'depois', jsonb_build_object(
                        'status_id', NEW.status_id,
                        'responsavel_id', NEW.responsavel_id
                    )
                )
            ELSE NULL
        END
    );
    
    RETURN NULL;
END;
$$ LANGUAGE plpgsql;

-- Aplicar o trigger de log em tabelas importantes
CREATE TRIGGER trg_log_atendimento
AFTER INSERT OR UPDATE OR DELETE ON atendimento
FOR EACH STATEMENT EXECUTE FUNCTION registrar_log_operacao();
```

## Estratégias de Implementação

### Particionamento

Para tabelas que crescem rapidamente, como LOG e HISTORICO_ATENDIMENTO, será implementado o particionamento por intervalo de tempo:

```sql
-- Criar tabela de log particionada por mês
CREATE TABLE log (
    log_id BIGSERIAL,
    data_hora TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT CURRENT_TIMESTAMP,
    usuario_id INTEGER,
    acao VARCHAR(100) NOT NULL,
    entidade VARCHAR(50) NOT NULL,
    entidade_id INTEGER,
    detalhes JSONB,
    FOREIGN KEY (usuario_id) REFERENCES usuario(usuario_id)
) PARTITION BY RANGE (data_hora);

-- Criar partições iniciais por trimestre
CREATE TABLE log_y2025q1 PARTITION OF log
    FOR VALUES FROM ('2025-01-01') TO ('2025-04-01');

CREATE TABLE log_y2025q2 PARTITION OF log
    FOR VALUES FROM ('2025-04-01') TO ('2025-07-01');

-- Criar função para gerar partições automaticamente
CREATE OR REPLACE FUNCTION criar_particao_log_mensal()
RETURNS VOID AS $$
DECLARE
    data_inicio DATE;
    data_fim DATE;
    nome_tabela TEXT;
BEGIN
    -- Define o início do próximo trimestre
    data_inicio := date_trunc('quarter', CURRENT_DATE + INTERVAL '3 months');
    data_fim := data_inicio + INTERVAL '3 months';
    nome_tabela := 'log_y' || to_char(data_inicio, 'YYYY') || 'q' || to_char(data_inicio, 'Q');
    
    -- Verifica se a partição já existe
    PERFORM 1
    FROM   pg_class c
    JOIN   pg_namespace n ON n.oid = c.relnamespace
    WHERE  c.relname = nome_tabela
    AND    n.nspname = 'public';
    
    -- Se não existir, cria a partição
    IF NOT FOUND THEN
        EXECUTE format(
            'CREATE TABLE %I PARTITION OF log
             FOR VALUES FROM (%L) TO (%L)',
            nome_tabela, data_inicio, data_fim
        );
        RAISE NOTICE 'Criada partição %', nome_tabela;
    END IF;
END;
$$ LANGUAGE plpgsql;

-- Configurar para execução periódica
-- Nota: Na implementação real, isso seria configurado como um job no banco de dados
-- ou como uma tarefa agendada na aplicação
```

Estratégia similar será aplicada à tabela HISTORICO_ATENDIMENTO.

### Otimizações para PostgreSQL

```sql
-- Habilitar extensões necessárias
CREATE EXTENSION IF NOT EXISTS pg_trgm; -- Para busca por similaridade em textos
CREATE EXTENSION IF NOT EXISTS pgcrypto; -- Para funções criptográficas
CREATE EXTENSION IF NOT EXISTS btree_gin; -- Para índices GIN em colunas não-textuais
CREATE EXTENSION IF NOT EXISTS tablefunc; -- Para funções de tabela cruzada (útil para relatórios)

-- Configurar busca textual para materiais
ALTER TABLE material ADD COLUMN texto_busca tsvector;
CREATE INDEX idx_material_texto_busca ON material USING gin(texto_busca);

-- Função para atualizar automaticamente o texto_busca
CREATE OR REPLACE FUNCTION atualizar_texto_busca_material()
RETURNS TRIGGER AS $$
BEGIN
    NEW.texto_busca := 
        setweight(to_tsvector('portuguese', COALESCE(NEW.titulo, '')), 'A') ||
        setweight(to_tsvector('portuguese', COALESCE(NEW.descricao, '')), 'B') ||
        setweight(to_tsvector('portuguese', array_to_string(NEW.tags, ' ')), 'C');
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_atualizar_texto_busca_material
BEFORE INSERT OR UPDATE ON material
FOR EACH ROW EXECUTE FUNCTION atualizar_texto_busca_material();

-- Otimizar consultas frequentes com visões materializadas
-- Exemplo: Visão materializada para dashboard de atendimentos
CREATE MATERIALIZED VIEW mv_dashboard_atendimentos AS
SELECT 
    u.unidade_id,
    u.nome AS unidade_nome,
    u.cidade,
    COUNT(a.atendimento_id) AS total_atendimentos,
    COUNT(CASE WHEN a.status_id = 1 THEN 1 END) AS atendimentos_abertos,
    COUNT(CASE WHEN a.status_id = 5 THEN 1 END) AS atendimentos_concluidos,
    AVG(EXTRACT(EPOCH FROM (a.ultima_atualizacao - a.data_abertura))/86400)::numeric(10,2) AS tempo_medio_dias
FROM 
    unidade u
LEFT JOIN 
    atendimento a ON u.unidade_id = a.unidade_id
WHERE 
    a.data_abertura >= CURRENT_DATE - INTERVAL '12 months'
GROUP BY 
    u.unidade_id, u.nome, u.cidade
WITH DATA;

CREATE UNIQUE INDEX ON mv_dashboard_atendimentos (unidade_id);

-- Função para atualizar visão materializada
CREATE OR REPLACE FUNCTION atualizar_mv_dashboard()
RETURNS VOID AS $$
BEGIN
    REFRESH MATERIALIZED VIEW CONCURRENTLY mv_dashboard_atendimentos;
END;
$$ LANGUAGE plpgsql;

-- Configurar vacuum automático para tabelas com muita rotatividade
ALTER TABLE log SET (autovacuum_vacuum_scale_factor = 0.1);
ALTER TABLE log SET (autovacuum_analyze_scale_factor = 0.05);
ALTER TABLE log SET (autovacuum_vacuum_threshold = 1000);
ALTER TABLE log SET (autovacuum_analyze_threshold = 500);
```

### Estratégia para Dados Históricos

```sql
-- Função para arquivar dados históricos antigos
CREATE OR REPLACE FUNCTION arquivar_logs_antigos(data_limite TIMESTAMP WITH TIME ZONE)
RETURNS INTEGER AS $$
DECLARE
    numero_registros INTEGER;
    tabela_arquivo TEXT;
BEGIN
    -- Define o nome da tabela de arquivo por ano
    tabela_arquivo := 'log_arquivo_' || to_char(data_limite, 'YYYY');
    
    -- Cria a tabela de arquivo se não existir
    EXECUTE format(
        'CREATE TABLE IF NOT EXISTS %I (LIKE log INCLUDING ALL)',
        tabela_arquivo
    );
    
    -- Move os registros para a tabela de arquivo
    EXECUTE format(
        'WITH registros_movidos AS (
            DELETE FROM log 
            WHERE data_hora < %L
            RETURNING *
        )
        INSERT INTO %I SELECT * FROM registros_movidos',
        data_limite, tabela_arquivo
    );
    
    -- Obtém o número de registros movidos
    GET DIAGNOSTICS numero_registros = ROW_COUNT;
    
    -- Cria índice na tabela de arquivo
    EXECUTE format(
        'CREATE INDEX IF NOT EXISTS %I ON %I (data_hora)',
        'idx_' || tabela_arquivo || '_data', tabela_arquivo
    );
    
    RETURN numero_registros;
END;
$$ LANGUAGE plpgsql;

-- Exemplo de uso:
-- SELECT arquivar_logs_antigos(CURRENT_DATE - INTERVAL '1 year');
```

## Migração e Versionamento

### Estratégia de Migração

Para gerenciar a evolução do esquema do banco de dados, será utilizada uma ferramenta de migração como Flyway ou Liquibase. Cada alteração no esquema será versionada e aplicada de forma controlada.

Estrutura típica de arquivos de migração no Flyway:

```
db/migration/
  V1__initial_schema.sql
  V2__add_status_table.sql
  V3__create_notification_triggers.sql
  V4__add_search_indexes.sql
  ...
```

Cada script deve ser idempotente quando possível, usando cláusulas como IF NOT EXISTS para criar objetos e verificações antes de alterar dados.

### Versionamento do Banco

1. **Nomenclatura de migrações**:
   - Usar o formato V{versão}__{descrição}.sql
   - Versão segue o padrão ano.mês.dia.sequência (ex: V2025.05.15.1)

2. **Controle de migrações aplicadas**:
   - Usar tabela especial para controlar qual migração foi aplicada
   - Validar checksums para garantir que os scripts não foram alterados

3. **Reversão**:
   - Criar scripts de rollback (U{versão}__{descrição}.sql) para cada migração
   - Documentar limitações onde rollback não é totalmente possível

4. **Ambientes**:
   - Garantir que as migrações são testadas em ambiente de homologação antes de produção
   - Manter sincronizados os esquemas entre ambientes (dev, QA, produção)

## Segurança no Nível de Banco de Dados

1. **Usuários e Roles**:

```sql
-- Criar roles para diferentes níveis de acesso
CREATE ROLE app_read_only;
CREATE ROLE app_read_write;
CREATE ROLE app_admin;

-- Conceder permissões apropriadas
GRANT SELECT ON ALL TABLES IN SCHEMA public TO app_read_only;

GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO app_read_write;
REVOKE DELETE ON log FROM app_read_write; -- Proibir deleção de logs

GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO app_admin;
GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO app_admin;

-- Criar usuários específicos da aplicação
CREATE USER app_service WITH PASSWORD 'senha_segura_service';
GRANT app_read_write TO app_service;

CREATE USER app_report WITH PASSWORD 'senha_segura_report';
GRANT app_read_only TO app_report;

CREATE USER app_maintenance WITH PASSWORD 'senha_segura_maintenance';
GRANT app_admin TO app_maintenance;
```

2. **Row-Level Security (RLS)**:

```sql
-- Habilitar RLS para garantir que usuários só vejam dados de suas unidades
ALTER TABLE atendimento ENABLE ROW LEVEL SECURITY;

-- Criar políticas
CREATE POLICY atendimento_unidade_policy ON atendimento
    USING (
        unidade_id IN (
            SELECT unidade_id FROM coordenador 
            WHERE usuario_id = current_setting('app.current_user_id')::integer
        )
        OR
        responsavel_id = current_setting('app.current_user_id')::integer
        OR
        current_setting('app.current_user_role')::text = 'ADMIN'
    );

-- Função para definir contexto do usuário
CREATE OR REPLACE FUNCTION set_user_context(user_id integer, user_role text)
RETURNS void AS $$
BEGIN
    PERFORM set_config('app.current_user_id', user_id::text, false);
    PERFORM set_config('app.current_user_role', user_role, false);
END;
$$ LANGUAGE plpgsql;
```

3. **Criptografia de Dados Sensíveis**:

```sql
-- Função para criptografar dados sensíveis
CREATE OR REPLACE FUNCTION encrypt_sensitive_data(data text)
RETURNS text AS $$
BEGIN
    RETURN encode(pgp_sym_encrypt(data, current_setting('app.encryption_key')), 'base64');
END;
$$ LANGUAGE plpgsql SECURITY DEFINER;

-- Função para descriptografar dados sensíveis
CREATE OR REPLACE FUNCTION decrypt_sensitive_data(encrypted_data text)
RETURNS text AS $$
BEGIN
    RETURN pgp_sym_decrypt(decode(encrypted_data, 'base64'), 
                          current_setting('app.encryption_key'))::text;
END;
$$ LANGUAGE plpgsql SECURITY DEFINER;
```

## Considerações Operacionais

1. **Manutenção Regular**:
   - Configurar VACUUM FULL periodicamente para tabelas com muita rotatividade
   - Atualizar estatísticas frequentemente com ANALYZE
   - Monitorar e reindexar conforme necessário

2. **Monitoramento de Performance**:
   - Configurar alertas para queries lentas
   - Monitorar uso de índices
   - Revisar planos de execução para queries críticas

3. **Backup e Recuperação**:
   - Backup completo diário
   - Backup incremental a cada 4 horas
   - Testes periódicos de recuperação
   - Políticas de retenção: 7 dias para backups diários, 30 dias para backups semanais, 365 dias para backups mensais

4. **Alta Disponibilidade**:
   - Configurar replicação master-slave
   - Implementar failover automático
   - Monitorar lag de replicação

5. **Auditoria**:
   - Habilitar registro de consultas para operações críticas
   - Manter trilhas de auditoria para todas as alterações em dados sensíveis
   - Revisar regularmente os logs de acesso ao banco

6. **Escalabilidade**:
   - Planejar particionamento para tabelas que crescem além de 10 milhões de registros
   - Considerar read replicas para distribuir carga de consultas
   - Implementar mecanismos de cache para consultas frequentes