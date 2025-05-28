# Documentação de Configuração

## 1. Objetivo
Orientar a configuração dos ambientes do sistema, detalhando parâmetros, variáveis, arquivos e práticas para garantir consistência, segurança e facilidade de manutenção.

## 2. Estrutura de Configuração
- **Configuração externa ao código**: Uso de arquivos `.env`, variáveis de ambiente ou serviços de configuração (ex: AWS Parameter Store, Azure Key Vault)
- **Perfis por ambiente**: DEV, HML, PRD
- **Segurança**: Não versionar segredos ou credenciais sensíveis

## 3. Parâmetros e Variáveis Principais
| Variável                | Descrição                              | Exemplo                      |
|------------------------|----------------------------------------|------------------------------|
| DB_HOST                | Host do banco de dados                  | db.cps.local                 |
| DB_PORT                | Porta do banco de dados                 | 5432                         |
| DB_USER                | Usuário do banco                        | cps_app                      |
| DB_PASSWORD            | Senha do banco (armazenar seguro)       | (não versionar)              |
| STORAGE_PATH           | Caminho para arquivos                   | /mnt/storage                 |
| AUTH_URL               | URL do serviço de autenticação          | https://auth.cps.sp.gov.br   |
| LOG_LEVEL              | Nível de log                            | INFO                         |
| APP_ENV                | Ambiente de execução                    | DEV, HML, PRD                |
| SECRET_KEY             | Chave secreta da aplicação              | (armazenar seguro)           |

## 4. Exemplo de Arquivo `.env`
```env
DB_HOST=db.cps.local
DB_PORT=5432
DB_USER=cps_app
DB_PASSWORD=********
STORAGE_PATH=/mnt/storage
AUTH_URL=https://auth.cps.sp.gov.br
LOG_LEVEL=INFO
APP_ENV=DEV
SECRET_KEY=********
```

## 5. Práticas Recomendadas
- Nunca versionar arquivos com segredos reais
- Utilizar cofre de segredos para produção
- Automatizar a aplicação de configurações via pipeline CI/CD
- Documentar mudanças de configuração
- Validar configurações em cada ambiente

## 6. Referências
- [Plano de Implantação](plano-de-implantacao.md)
- [Diagramas de Infraestrutura](diagramas-de-infraestrutura.md)
