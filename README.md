# Portfolio - Matheus RPV

[![CI/CD Pipeline](https://github.com/MatheusRPV/Portfolio/actions/workflows/ci-cd-pipeline.yml/badge.svg?branch=main)](https://github.com/MatheusRPV/Portfolio/actions/workflows/ci-cd-pipeline.yml)

## Descrição

Portfolio profissional com automação completa de CI/CD usando GitHub Actions. Este projeto implementa as 5 etapas de um pipeline DevOps robusto:

1. **Proteção de Branch (CI)** - Validações automáticas em Pull Requests
2. **Publicação Automática (CD)** - Deploy em GitHub Pages após merge
3. **Badge de Status** - Visibilidade do pipeline no README
4. **Notificações** - Email automático em caso de falha
5. **Matrix Strategy** - Testes em múltiplas versões do Node.js

## Tecnologias

- **GitHub Actions** - Automação de CI/CD
- **HTML Validate** - Linting de HTML
- **GitHub Pages** - Hospedagem estática
- **Shell Scripts** - Validações customizadas
- **Node.js** - Ambiente de desenvolvimento (18.x e 20.x)

## Funcionalidades da Pipeline

### ✅ Etapa 1 - Proteção de Branch (CI)

A pipeline valida cada Pull Request antes de permitir merge:

- **Validação de index.html**: Bloqueia PR se o arquivo for deletado ou renomeado
- **HTML Linting**: Verifica sintaxe HTML com html-validate
- **File Size Check**: Rejeita arquivos > 500KB
- **Text Scanning**: Bloqueia comentários TODO, FIXME, "senha" e "password"
- **Link Validation**: Verifica href e src de links e imagens
- **Matrix Strategy**: Testa em Node.js 18.x e 20.x simultaneamente

### ✅ Etapa 2 - Entrega Contínua (CD)

Após aprovação e merge na branch main:

- Deploy automático para GitHub Pages
- Site fica ativo em `https://[username].github.io/Portfolio/`
- Zero downtime, publicação instantânea

### ✅ Etapa 3 - Status Badge

Badge no topo do README mostra status em tempo real:

- Verde: Pipeline passando ✅
- Vermelho: Pipeline falhando ❌
- Clicável: Link para detalhes da execução

### ✅ Etapa 4 - Notificações

Email automático do GitHub quando a pipeline falha:

- Notificação imediata de falhas
- Integrado com preferências de notificação do GitHub
- Sem configuração de webhooks necessária

### ✅ Etapa 5 - Matrix Strategy

CI roda em paralelo com múltiplas versões:

- Node.js 18.x (LTS)
- Node.js 20.x (Current)

Garante compatibilidade entre versões.

## Estrutura do Projeto

```
Portfolio/
├── index.html                          # Página principal
├── styles.css                          # Estilos
├── package.json                        # Dependências npm
├── README.md                           # Este arquivo
├── images/                             # Pasta para imagens
└── .github/
    └── workflows/
        └── ci-cd-pipeline.yml          # Workflow de automação
```

## Como Usar Localmente

### Pré-requisitos

- Node.js 18.x ou 20.x instalado
- Git configurado

### Setup

```bash
# Clone o repositório
git clone https://github.com/MatheusRPV/Portfolio.git
cd Portfolio

# Instale as dependências
npm ci

# Execute o linter localmente
npm run lint

# Teste em um servidor local
npx http-server
```

### Validações Locais

```bash
# Validar HTML
npm run lint

# Conferir tamanho de arquivo
ls -lh index.html styles.css

# Buscar padrões proibidos
grep -ri "TODO\|FIXME\|senha\|password" . --include="*.html" --include="*.css"

# Validar links
grep -oP '(href|src)="\K[^"]+' index.html
```

## Workflow CI/CD

### Triggers

- **CI**: Dispara em todo push para main e em Pull Requests
- **CD**: Dispara apenas em push para main (após merge)

### Etapas do Workflow

1. **Setup**: Checkout código e Node.js
2. **Validação**: 5 checks de qualidade em paralelo (Matrix)
3. **Build**: Preparação para GitHub Pages
4. **Deploy**: Upload e publicação automática
5. **Notify**: Email se falhar

## Exemplos de Teste

### Teste 1: PR Válida (Deve Passar ✅)

```bash
git checkout -b feature/new-content
# Editar index.html mantenendo HTML válido
# Manter arquivo < 500KB
# Sem TODO, FIXME, senha ou password
git add .
git commit -m "Add new content"
git push origin feature/new-content
# Criar PR - todas as checks passarão
```

### Teste 2: HTML Inválido (Deve Falhar ❌)

```bash
git checkout -b test/invalid-html
# Remover DOCTYPE de index.html
git add .
git commit -m "Test invalid HTML"
git push origin test/invalid-html
# Criar PR - html-validate falhará
```

### Teste 3: Arquivo Grande (Deve Falhar ❌)

```bash
git checkout -b test/large-file
# Adicionar conteúdo para styles.css exceder 500KB
git add .
git commit -m "Test file size"
git push origin test/large-file
# Criar PR - size check falhará
```

### Teste 4: Deploy (Deve Funcionar ✅)

```bash
# Após PR válida ser feita merge
git checkout main
git pull origin main
# Site aparece automaticamente em GitHub Pages
# Badge no README muda para verde
```

## Configuração no GitHub

### 1. Ativar GitHub Pages

- Settings > Pages
- Source: Deploy from a branch
- Branch: gh-pages (criada automaticamente)
- Folder: / (root)

### 2. Ativar Notificações

- Settings > Notifications
- Seção "GitHub Actions"
- Marcar: "Failed workflow runs"

### 3. Workflow Permissions

- Settings > Actions > General
- Workflow permissions: "Read and write permissions"

## Status Atual

| Etapa | Status | Descrição |
|-------|--------|-----------|
| CI Protection | ✅ | Validações em PR funcionando |
| CD Deployment | ✅ | Deploy automático em GitHub Pages |
| Status Badge | ✅ | Badge visível no README |
| Notificações | ✅ | Email ao falhar |
| Matrix Testing | ✅ | Node 18 e 20 testando |

## Troubleshooting

| Problema | Solução |
|----------|---------|
| Badge mostra "unknown" | Aguardar primeiro workflow sucesso |
| Email não chega | Verificar Settings > Notifications |
| Pages não ativa | Garantir source seja "gh-pages" |
| Workflow não roda | Verificar permissões de credentials |

## Documentação Oficial

- [GitHub Actions Docs](https://docs.github.com/en/actions)
- [GitHub Pages Docs](https://docs.github.com/en/pages)
- [HTML-Validate Docs](https://html-validate.org/)

## Melhorias Futuras

- [ ] Adicionar testes CSS com stylelint
- [ ] Adicionar validação de acessibilidade (a11y)
- [ ] Integrar análise de performance
- [ ] Adicionar checagem de links externos
- [ ] Implementar relatórios de cobertura

## Licença

Este projeto está disponível sob a licença MIT.

## Contato

- GitHub: [@MatheusRPV](https://github.com/MatheusRPV)
- Email: matheus@example.com

---

**Última atualização**: 2024
**Pipeline Status**: [![CI/CD](https://github.com/MatheusRPV/Portfolio/actions/workflows/ci-cd-pipeline.yml/badge.svg?branch=main)](https://github.com/MatheusRPV/Portfolio/actions/workflows/ci-cd-pipeline.yml)
