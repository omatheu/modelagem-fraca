# Modelos de Integração de Sistemas
## Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza

## Sumário
- [Introdução](#introdução)
- [Requisitos de Integração](#requisitos-de-integração)
- [Visão Geral da Estratégia de Integração](#visão-geral-da-estratégia-de-integração)
- [Modelos de Integração](#modelos-de-integração)
  - [Integração Síncrona](#integração-síncrona)
  - [Integração Assíncrona](#integração-assíncrona)
  - [Integração por Lote](#integração-por-lote)
  - [Integração por API](#integração-por-api)
- [Integrações Específicas](#integrações-específicas)
  - [Integração com Sistema de Autenticação (LDAP/SSO)](#integração-com-sistema-de-autenticação-ldapsso)
  - [Integração com Sistema de Email](#integração-com-sistema-de-email)
  - [Integração com Sistema Acadêmico](#integração-com-sistema-acadêmico)
- [Padrões de Design para Integração](#padrões-de-design-para-integração)
- [Segurança na Integração](#segurança-na-integração)
- [Resiliência e Tratamento de Falhas](#resiliência-e-tratamento-de-falhas)
- [Monitoramento e Observabilidade](#monitoramento-e-observabilidade)
- [Governança e Controle de Versão](#governança-e-controle-de-versão)

## Introdução

Este documento descreve os modelos de integração de sistemas para o Sistema de Gestão da Assessoria de Inclusão do Centro Paula Souza. A integração com sistemas externos é um aspecto crítico da arquitetura, pois permite ao sistema interagir com a infraestrutura existente do CPS e aproveitar funcionalidades já implementadas em outros sistemas.

O documento aborda os diferentes métodos de integração, padrões de design aplicados, considerações de segurança e resiliência, bem como recomendações específicas para cada sistema externo com o qual será necessário integrar. Enquanto outros documentos de arquitetura descrevem a estrutura interna do sistema, este documento foca nas interfaces e interações com componentes externos.

## Requisitos de Integração

Com base nos requisitos funcionais e não funcionais do sistema, foram identificadas as seguintes necessidades de integração:

1. **Autenticação e Autorização**:
   - Integração com o sistema LDAP/SSO do CPS para autenticação centralizada
   - Sincronização de perfis e permissões de acesso

2. **Comunicação**:
   - Integração com o sistema de email institucional para envio de notificações
   - Possível integração com sistemas de mensageria para comunicações internas

3. **Dados Institucionais**:
   - Integração com o Sistema Acadêmico para obtenção de informações sobre alunos, cursos e unidades
   - Sincronização de dados cadastrais de unidades e profissionais

4. **Integrações Futuras**:
   - Capacidade de integração com sistemas de BI para análise avançada de dados
   - Possibilidade de expor APIs para outros sistemas consumirem informações relevantes

## Visão Geral da Estratégia de Integração

A estratégia de integração do sistema segue os seguintes princípios:

1. **Desacoplamento**: Minimizar dependências diretas com sistemas externos através de camadas de abstração
2. **Padronização**: Utilizar protocolos e formatos de dados padronizados para facilitar a manutenção
3. **Segurança**: Garantir a proteção dos dados em trânsito e controle de acesso adequado
4. **Resiliência**: Implementar mecanismos para lidar com falhas e indisponibilidade de sistemas externos
5. **Adaptabilidade**: Facilitar a integração com novos sistemas no futuro

A arquitetura de integração é organizada em três níveis:

- **Nível de Adaptador**: Componentes específicos para cada sistema externo
- **Nível de Abstração**: Interfaces e contratos padronizados para serviços externos
- **Nível de Domínio**: Uso de serviços abstraídos pelos níveis inferiores

Esta abordagem em camadas permite modificar detalhes de implementação de integrações específicas com impacto mínimo no restante do sistema.

## Modelos de Integração

### Integração Síncrona

**Descrição**: Comunicação em tempo real onde o sistema aguarda a resposta do sistema externo antes de prosseguir.

**Aplicações**:
- Verificação de credenciais durante login
- Consulta de dados cadastrais de alunos ou unidades
- Validação de informações em tempo real

**Tecnologias e Protocolos**:
- REST (HTTP/HTTPS)
- SOAP (para sistemas legados)
- gRPC (para comunicações de alta performance)

**Considerações**:
- Garante consistência imediata
- Pode impactar performance e disponibilidade do sistema
- Requer timeout adequado e tratamento de erros
- Necessário implementar circuit breaker para evitar falhas em cascata

**Exemplo de Implementação**:
```java
public class LdapAuthenticationService implements AuthenticationService {
    private final LdapClient ldapClient;
    private final CircuitBreaker circuitBreaker;
    
    @Override
    public AuthenticationResult authenticate(Credentials credentials) {
        return circuitBreaker.execute(() -> {
            try {
                LdapResponse response = ldapClient.authenticate(
                    credentials.getUsername(), 
                    credentials.getPassword()
                );
                return mapToAuthenticationResult(response);
            } catch (LdapException e) {
                logger.error("Falha na autenticação LDAP", e);
                throw new AuthenticationFailedException("Falha ao autenticar com serviço LDAP", e);
            }
        });
    }
}
```

### Integração Assíncrona

**Descrição**: Comunicação onde o sistema não aguarda resposta imediata, utilizando filas ou eventos.

**Aplicações**:
- Envio de notificações por email
- Processamento de grandes volumes de dados
- Operações não críticas para o fluxo principal

**Tecnologias e Protocolos**:
- Filas de mensagens (RabbitMQ, ActiveMQ)
- Publish/Subscribe (Kafka)
- WebHooks

**Considerações**:
- Melhora a resiliência e escalabilidade do sistema
- Requer mecanismos para garantir entrega eventual
- Necessário gerenciar estado e idempotência
- Pode complicar o rastreamento de problemas

**Exemplo de Implementação**:
```java
public class EmailNotificationService implements NotificationService {
    private final MessageQueue messageQueue;
    
    @Override
    public void sendNotification(Notification notification) {
        EmailMessage message = new EmailMessage(
            notification.getRecipient(),
            notification.getSubject(),
            notification.getBody()
        );
        
        // Envia para fila assíncrona
        messageQueue.send("email.notifications", message);
        
        // Registra tentativa de envio
        notificationRepository.markAsSent(notification.getId());
    }
}
```

### Integração por Lote

**Descrição**: Transferência de dados em grandes volumes, processados periodicamente em lotes.

**Aplicações**:
- Sincronização periódica de dados cadastrais
- Importação de dados históricos
- Geração de relatórios consolidados

**Tecnologias e Protocolos**:
- Transferência de arquivos (SFTP)
- ETL (Extract, Transform, Load)
- APIs em batch

**Considerações**:
- Minimiza impacto em sistemas de produção
- Adequado para grandes volumes de dados
- Requer janela de manutenção ou processamento
- Necessita tratamento para dados inconsistentes

**Exemplo de Implementação**:
```java
@Scheduled(cron = "0 0 2 * * ?") // Executa às 2h da manhã
public void synchronizeStudentData() {
    try {
        List<StudentRecord> records = academicSystemClient.downloadStudentBatch();
        batchProcessor.process(records, this::updateStudentRecord);
        auditService.logBatchOperation("student-sync", records.size(), "SUCCESS");
    } catch (Exception e) {
        auditService.logBatchOperation("student-sync", 0, "FAILED");
        notificationService.notifyAdmins("Falha na sincronização de dados de alunos", e.getMessage());
        throw e;
    }
}
```

### Integração por API

**Descrição**: Exposição de funcionalidades do sistema através de interfaces programáticas bem definidas.

**Aplicações**:
- Permitir que outros sistemas consumam dados do sistema
- Oferecer extensibilidade para módulos externos
- Facilitar integrações futuras

**Tecnologias e Protocolos**:
- REST API com Swagger/OpenAPI
- GraphQL
- API Gateway

**Considerações**:
- Necessário gerenciamento cuidadoso de versões
- Requer controle de acesso e rate limiting
- Documentação abrangente é essencial
- Deve-se considerar contratos de serviço

**Exemplo de Implementação**:
```java
@RestController
@RequestMapping("/api/v1/atendimentos")
public class AtendimentoApiController {
    
    private final AtendimentoService atendimentoService;
    private final ApiAuthorizationService authService;
    
    @GetMapping
    public ResponseEntity<List<AtendimentoDTO>> listarAtendimentos(
            @RequestParam(required = false) String status,
            @RequestParam(required = false) Long unidadeId,
            @RequestHeader("API-Key") String apiKey) {
        
        // Verifica autorização da chave de API
        ApiClient client = authService.validateApiKey(apiKey);
        authService.checkPermission(client, "atendimentos.listar");
        
        // Aplica filtros conforme parâmetros
        AtendimentoFilter filter = new AtendimentoFilter(status, unidadeId);
        
        // Executa busca e retorna resultados
        List<AtendimentoDTO> atendimentos = atendimentoService
            .buscarAtendimentos(filter)
            .stream()
            .map(AtendimentoDTO::fromEntity)
            .collect(Collectors.toList());
            
        return ResponseEntity.ok(atendimentos);
    }
}
```

## Integrações Específicas

### Integração com Sistema de Autenticação (LDAP/SSO)

**Objetivo**: Permitir autenticação centralizada utilizando as credenciais institucionais do CPS.

**Modelo de Integração**: Síncrono

**Fluxo de Integração**:
1. Usuário informa credenciais na interface do sistema
2. Sistema encaminha as credenciais para o serviço LDAP/SSO do CPS
3. Serviço LDAP/SSO verifica as credenciais e retorna resultado
4. Sistema cria sessão local e obtém perfis/permissões

**Componentes Envolvidos**:
- `shared.security`: Camada de serviços de segurança
- `infra.integration.ldap`: Adaptador específico para LDAP
- `application.usuario`: Serviço de aplicação para gestão de usuários

**Considerações Técnicas**:
- Implementar cache local de autenticação para reduzir chamadas ao LDAP
- Utilizar conexão segura (LDAPS) para comunicação com o servidor LDAP
- Configurar timeout e retry adequados
- Implementar autenticação fallback para casos de indisponibilidade

**Diagrama de Sequência**:
```
┌─────────┐    ┌──────────────┐    ┌───────────────────┐    ┌──────────────┐
│ Browser │    │ Sistema      │    │ LDAP/SSO Adapter  │    │ LDAP Server  │
└────┬────┘    └───────┬──────┘    └─────────┬─────────┘    └──────┬───────┘
     │               1.│                      │                     │
     │ Login Request   │                      │                     │
     │───────────────>│                       │                     │
     │                │                       │                     │
     │                │ 2. Authenticate       │                     │
     │                │──────────────────────>│                     │
     │                │                       │  3. LDAP Bind       │
     │                │                       │────────────────────>│
     │                │                       │                     │
     │                │                       │  4. Result          │
     │                │                       │<────────────────────│
     │                │ 5. Auth Result        │                     │
     │                │<──────────────────────│                     │
     │                │                       │                     │
     │                │ 6. Create Session     │                     │
     │                │───────┐               │                     │
     │                │       │               │                     │
     │                │<──────┘               │                     │
     │ 7. Response    │                       │                     │
     │<──────────────│                       │                     │
     │                │                       │                     │
```

### Integração com Sistema de Email

**Objetivo**: Enviar notificações por email aos usuários do sistema.

**Modelo de Integração**: Assíncrono

**Fluxo de Integração**:
1. Sistema identifica necessidade de envio de notificação
2. Sistema coloca mensagem em fila de notificações
3. Processador de notificações consome a mensagem da fila
4. Adaptador de email se conecta ao servidor SMTP do CPS
5. Email é enviado e status é registrado

**Componentes Envolvidos**:
- `shared.notification`: Serviço de notificações
- `infra.email`: Adaptador para envio de emails
- `infra.integration.smtp`: Configuração de conexão SMTP

**Considerações Técnicas**:
- Implementar sistema de filas para garantir entrega mesmo com falhas temporárias
- Estabelecer mecanismo de retry com backoff exponencial
- Configurar templates de email para facilitar manutenção
- Implementar monitoramento de taxas de entrega e falhas
- Considerar rate limits do servidor SMTP

**Diagrama de Sequência**:
```
┌──────────────┐    ┌──────────────┐    ┌────────────┐    ┌───────────────┐    ┌────────────┐
│ Application  │    │ Notification │    │ Message    │    │ Email Service │    │ SMTP       │
│ Service      │    │ Service      │    │ Queue      │    │ Adapter       │    │ Server     │
└──────┬───────┘    └──────┬───────┘    └─────┬──────┘    └───────┬───────┘    └─────┬──────┘
       │                   │                  │                   │                  │
       │ 1. Send           │                  │                   │                  │
       │ Notification      │                  │                   │                  │
       │────────────────>  │                  │                   │                  │
       │                   │ 2. Queue Message │                   │                  │
       │                   │─────────────────>│                   │                  │
       │ 3. Acknowledge    │                  │                   │                  │
       │ <────────────────  │                  │                   │                  │
       │                   │                  │                   │                  │
       │                   │                  │ 4. Consume        │                  │
       │                   │                  │────────────────>  │                  │
       │                   │                  │                   │ 5. Connect       │
       │                   │                  │                   │─────────────────>│
       │                   │                  │                   │                  │
       │                   │                  │                   │ 6. Send Email    │
       │                   │                  │                   │─────────────────>│
       │                   │                  │                   │                  │
       │                   │                  │                   │ 7. Status        │
       │                   │                  │                   │<─────────────────│
       │                   │                  │                   │                  │
       │                   │                  │ 8. Log Result     │                  │
       │                   │ <───────────────────────────────────  │                  │
       │                   │                  │                   │                  │
```

### Integração com Sistema Acadêmico

**Objetivo**: Obter dados de estudantes, cursos e unidades do sistema acadêmico do CPS.

**Modelo de Integração**: Híbrido (Lote + API)

**Fluxo de Integração**:
1. **Sincronização em Lote**:
   - Job agendado inicia processo de sincronização periódica
   - Sistema consulta API do sistema acadêmico para obter dados atualizados
   - Dados são processados e armazenados localmente
   - Conflitos são registrados para revisão manual

2. **Consultas Pontuais**:
   - Sistema precisa de informação específica não armazenada localmente
   - API do sistema acadêmico é consultada diretamente
   - Resultado é cacheado temporariamente para consultas subsequentes

**Componentes Envolvidos**:
- `infra.integration.academic`: Adaptador para o sistema acadêmico
- `application.sincronizacao`: Serviço de sincronização de dados
- `shared.scheduler`: Agendador de tarefas

**Considerações Técnicas**:
- Priorizar sincronização em lote para reduzir chamadas à API
- Implementar cache local com TTL apropriado
- Estabelecer estratégia para resolução de conflitos
- Criar mecanismos para reconciliação de dados
- Considerar volume de dados e impacto em performance

**Diagrama de Fluxo**:
```
┌───────────────────┐           ┌──────────────────────┐
│                   │           │                      │
│  Scheduler        │───────────▶ Synchronization Job  │
│                   │           │                      │
└───────────────────┘           └──────────┬───────────┘
                                           │
                                           ▼
┌───────────────────┐           ┌──────────────────────┐
│                   │◀──────────┤                      │
│  Academic System  │           │  Integration Adapter │
│  API              │───────────▶                      │
│                   │           └──────────┬───────────┘
└───────────────────┘                      │
                                           │
                                           ▼
┌───────────────────┐           ┌──────────────────────┐
│                   │           │                      │
│  Local Database   │◀──────────┤  Data Processor      │
│                   │           │                      │
└───────────────────┘           └──────────┬───────────┘
                                           │
                                           ▼
                               ┌──────────────────────┐
                               │                      │
                               │  Conflict Resolution │
                               │  Queue               │
                               └──────────────────────┘
```

## Padrões de Design para Integração

### 1. Adapter Pattern

**Descrição**: Converte a interface de uma classe em outra interface esperada pelo cliente.

**Aplicação**: Utilizado para encapsular as peculiaridades de cada sistema externo atrás de uma interface consistente.

**Exemplo**:
```java
// Interface comum
public interface AuthenticationProvider {
    AuthResult authenticate(String username, String password);
}

// Implementação específica para LDAP
public class LdapAuthenticationAdapter implements AuthenticationProvider {
    private final LdapClient ldapClient;
    
    @Override
    public AuthResult authenticate(String username, String password) {
        LdapResponse response = ldapClient.bind(username, password);
        return mapLdapResponseToAuthResult(response);
    }
}
```

### 2. Facade Pattern

**Descrição**: Fornece uma interface unificada para um conjunto de interfaces em um subsistema.

**Aplicação**: Simplifica o acesso a sistemas externos complexos através de uma interface limpa e coesa.

**Exemplo**:
```java
public class AcademicSystemFacade {
    private final StudentApiClient studentClient;
    private final CourseApiClient courseClient;
    private final UnitApiClient unitClient;
    
    public StudentInfo getStudentInfo(String studentId) {
        return studentClient.getStudentDetails(studentId);
    }
    
    public List<Course> getCoursesForUnit(String unitId) {
        return courseClient.listCoursesByUnit(unitId);
    }
    
    public UnitDetails getUnitDetails(String unitId) {
        return unitClient.getUnitInfo(unitId);
    }
}
```

### 3. Circuit Breaker Pattern

**Descrição**: Previne falhas em cascata ao detectar falhas e redirecionar para caminhos alternativos.

**Aplicação**: Proteger o sistema contra falhas prolongadas em sistemas externos.

**Exemplo**:
```java
public class EmailServiceWithCircuitBreaker {
    private final EmailClient emailClient;
    private final CircuitBreaker circuitBreaker;
    
    public void sendEmail(Email email) {
        circuitBreaker.executeWithFallback(
            () -> emailClient.send(email),
            fallbackAction -> {
                logFailedEmail(email);
                queueForRetryLater(email);
                return null;
            }
        );
    }
}
```

### 4. Repository Pattern

**Descrição**: Encapsula a lógica necessária para acessar fontes de dados.

**Aplicação**: Abstrair o acesso a dados externos, permitindo mudar a implementação sem afetar o domínio.

**Exemplo**:
```java
public interface StudentRepository {
    Student findById(String studentId);
    List<Student> findByUnit(String unitId);
    void save(Student student);
}

public class ExternalStudentRepository implements StudentRepository {
    private final AcademicSystemClient client;
    private final StudentCache cache;
    
    @Override
    public Student findById(String studentId) {
        return cache.get(studentId)
            .orElseGet(() -> {
                Student student = client.getStudent(studentId);
                cache.put(studentId, student);
                return student;
            });
    }
    // Outras implementações...
}
```

## Segurança na Integração

### Autenticação e Autorização

1. **Autenticação de Sistemas**:
   - Utilizar credenciais de sistema ou tokens de serviço para autenticar em APIs externas
   - Rotacionar credenciais regularmente
   - Armazenar credenciais em cofre seguro, não hardcoded

2. **Controle de Acesso**:
   - Implementar controle granular sobre quais partes do sistema podem acessar sistemas externos
   - Seguir princípio do menor privilégio
   - Auditar todos os acessos a sistemas externos

3. **API Keys e Tokens**:
   - Utilizar OAuth 2.0 quando disponível
   - Implementar expiração automática de tokens
   - Validar origem das requisições

### Proteção de Dados

1. **Dados em Trânsito**:
   - Utilizar TLS 1.3 ou superior para todas as comunicações
   - Implementar validação de certificados
   - Aplicar cifragem adicional para dados sensíveis

2. **Dados em Uso**:
   - Sanitizar dados antes de processamento
   - Evitar logging de dados sensíveis
   - Implementar mascaramento de dados quando necessário

3. **Dados Armazenados**:
   - Aplicar criptografia para armazenamento local de dados de sistemas externos
   - Definir políticas de retenção claras
   - Considerar pseudonimização para dados sensíveis

### Validação e Sanitização

1. **Entrada de Dados**:
   - Validar todos os dados recebidos de sistemas externos
   - Implementar deserialização segura
   - Tratar adequadamente tipos de dados e formatos

2. **Saída de Dados**:
   - Validar todos os dados enviados para sistemas externos
   - Implementar serialização segura
   - Aplicar controles de limite de taxa

## Resiliência e Tratamento de Falhas

### Estratégias de Resiliência

1. **Circuit Breaker**:
   - Interromper temporariamente chamadas a sistemas externos após falhas consecutivas
   - Implementar half-open state para tentativas de recuperação
   - Configurar thresholds adequados ao contexto

2. **Timeout**:
   - Definir timeouts adequados para cada integração
   - Evitar bloqueios prolongados aguardando resposta
   - Considerar diferenças entre read timeout e connection timeout

3. **Retry com Backoff Exponencial**:
   - Implementar tentativas automáticas com intervalos crescentes
   - Definir número máximo de tentativas
   - Considerar jitter para evitar tempestades de requisições

4. **Bulkhead**:
   - Isolar recursos para diferentes integrações
   - Evitar que falha em um sistema afete outros
   - Aplicar limites de concorrência específicos

### Fallbacks

1. **Cache Local**:
   - Utilizar dados em cache quando sistema externo estiver indisponível
   - Implementar estratégia de invalidação adequada
   - Indicar ao usuário quando dados não estão atualizados

2. **Degradação Graciosa**:
   - Desabilitar funcionalidades não essenciais quando dependências estiverem indisponíveis
   - Oferecer funcionalidade limitada em vez de falha completa
   - Comunicar claramente limitações temporárias aos usuários

3. **Fila de Dead Letter**:
   - Armazenar requisições que não puderam ser processadas
   - Implementar processo para retry manual ou automático posterior
   - Monitorar e alertar sobre crescimento da fila

## Monitoramento e Observabilidade

### Métricas-chave

1. **Métricas de Disponibilidade**:
   - Uptime de cada sistema integrado
   - Taxa de sucesso de requisições
   - Tempo entre falhas (MTBF)

2. **Métricas de Performance**:
   - Tempo de resposta (mínimo, médio, máximo, percentis)
   - Throughput (requisições por segundo)
   - Uso de recursos (conexões, threads)

3. **Métricas de Negócio**:
   - Taxa de sincronização bem-sucedida
   - Volume de dados transferidos
   - Taxa de utilização de cada integração

### Logging

1. **Eventos para Registro**:
   - Início e término de operações de integração
   - Falhas e recuperações
   - Alterações de estado (circuit breaker open/closed)
   - Dados transferidos (sem informações sensíveis)

2. **Contexto de Log**:
   - Incluir identificadores de correlação
   - Registrar metadados relevantes
   - Estruturar logs para facilitar busca e análise

3. **Níveis de Log**:
   - Utilizar níveis apropriados (DEBUG, INFO, WARN, ERROR)
   - Configurar verbosidade diferente por ambiente
   - Implementar filtragem para evitar sobrecarga

### Rastreamento Distribuído

1. **Implementação**:
   - Utilizar bibliotecas como OpenTelemetry
   - Propagar contexto entre sistemas
   - Correlacionar chamadas relacionadas

2. **Visualização**:
   - Implementar dashboards para visualizar fluxos de integração
   - Identificar gargalos e pontos de falha
   - Analisar tempos de processamento em cada etapa

## Governança e Controle de Versão

### Gestão de Contratos

1. **Contratos de API**:
   - Documentar formalmente interfaces com sistemas externos
   - Utilizar especificações como OpenAPI/Swagger
   - Implementar testes de contrato

2. **Versionamento**:
   - Adotar versionamento semântico para interfaces
   - Implementar estratégias de convivência entre versões
   - Planejar descontinuação ordenada de versões antigas

3. **Compatibilidade**:
   - Priorizar mudanças compatíveis com versões anteriores
   - Implementar adaptadores para lidar com mudanças incompatíveis
   - Testar compatibilidade regularmente

### Processos de Mudança

1. **Gestão de Mudanças**:
   - Documentar processo para alterações em integrações
   - Implementar revisão por pares para mudanças
   - Coordenar alterações com equipes responsáveis pelos sistemas externos

2. **Ambientes de Teste**:
   - Manter ambientes de integração separados
   - Implementar mocks e simuladores para testes isolados
   - Realizar testes de integração completos antes de mudanças em produção

3. **Rollback e Contingência**:
   - Planejar estratégias de rollback para cada integração
   - Documentar procedimentos de contingência
   - Realizar simulações de falha periodicamente