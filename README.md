# 🚀 Pipeline DevOps & Automação de Qualidade (Python)

[![Python](https://img.shields.io/badge/Python-3.12-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![CI/CD](https://img.shields.io/badge/CI%2FCD-GitHub_Actions-2088FF?logo=githubactions&logoColor=white)](https://github.com/JeffersonTeles/DevOps/actions)
[![Tests](https://img.shields.io/badge/Tests-pytest-0A9EDC?logo=pytest&logoColor=white)](https://docs.pytest.org/)
[![Coverage](https://img.shields.io/badge/Coverage-pytest--cov-brightgreen)](https://pytest-cov.readthedocs.io/)
[![Linter](https://img.shields.io/badge/Linter-Ruff-D7FF64?logo=ruff&logoColor=black)](https://docs.astral.sh/ruff/)
[![Quality Gate](https://img.shields.io/badge/Quality_Gate-Enforced-success)](https://github.com/JeffersonTeles/DevOps)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Projeto de automação e integração contínua (CI) em Python com pipeline de testes unitários, medição de cobertura de código, linting estático com Ruff, smoke testing de CLI e portão de qualidade (*quality gate*) automatizado via GitHub Actions.

---

## 📋 Visão Geral

O projeto implementa um módulo de cálculo de pedidos comerciais (`app.pedidos`), com suporte a múltiplos itens, faixas de desconto, cupons promocionais cumulativos e interface de linha de comando (`app.cli`).

Toda alteração de código passa por uma esteira de entrega contínua estruturada em 5 estágios interdependentes no GitHub Actions:

1. **Testes Unitários:** Execução isolada dos testes de regras de negócio com `pytest`.
2. **Cobertura de Código:** Validação de cobertura mínima e relatório de linhas descobertas via `pytest-cov`.
3. **Análise Estática & Linting:** Verificação de formatação e boas práticas com `Ruff` (regras E, F, I, B, UP).
4. **Smoke Test de CLI:** Execução ponta a ponta da interface de linha de comando simulando uso real.
5. **Quality Gate:** Estágio final que valida o sucesso mandatório de todos os jobs anteriores antes da aprovação do pull request.

---

## 🛠️ Tecnologias Utilizadas

- **Linguagem:** Python 3.12
- **Testes & Cobertura:** `pytest` (>= 8.0) e `pytest-cov` (>= 5.0)
- **Linter & Formatter:** `Ruff` (>= 0.6)
- **Automação de Tarefas:** `Makefile`
- **CI/CD:** GitHub Actions (`.github/workflows/testes.yml` e `ci.yml`)

---

## 📁 Estrutura do Repositório

```text
.
├── .github/
│   └── workflows/
│       ├── ci.yml                 # Pipeline simplificado de CI
│       └── testes.yml             # Pipeline em 5 estágios com Quality Gate
├── app/
│   ├── __init__.py
│   ├── cli.py                     # Interface de linha de comando (argparse)
│   └── pedidos.py                 # Regras de negócio, cupons e cálculo de totais
├── tests/
│   ├── test_cli.py                # Testes automatizados da CLI
│   └── test_pedidos.py            # Testes unitários de regras de negócio
├── calculadora.py                 # Módulo complementar de funções utilitárias
├── test_calculadora.py            # Testes complementares do módulo calculadora
├── Makefile                       # Atalhos para comandos de desenvolvimento e qualidade
├── pyproject.toml                 # Configurações do pytest, coverage e ruff
├── requirements.txt               # Dependências de execução
└── requirements-dev.txt           # Dependências de desenvolvimento e testes
```

---

## 🚀 Como Executar Localmente

### Pré-requisitos
- Python 3.12+ instalado

### 1. Clonar o repositório e criar ambiente virtual

```bash
git clone https://github.com/JeffersonTeles/DevOps.git
cd DevOps
python3 -m venv .venv
source .venv/bin/activate  # Linux/macOS
# ou .venv\Scripts\activate no Windows
```

### 2. Instalar dependências

Com o Makefile:
```bash
make install
```

Ou diretamente com pip:
```bash
pip install -r requirements-dev.txt
```

### 3. Executar a suíte de testes

```bash
make test
# Ou:
python -m pytest -q
```

### 4. Executar verificação de cobertura de código

```bash
make cov
# Gera o relatório no terminal exibindo o percentual coberto por módulo
```

### 5. Executar análise estática de código (Lint)

```bash
make lint
# Ou:
ruff check .
```

### 6. Executar todos os testes de qualidade de uma vez

```bash
make quality
```

---

## 💻 Uso da Linha de Comando (CLI)

O projeto expõe um ponto de entrada CLI para cálculo rápido de pedidos:

```bash
python -m app.cli --preco 100 --quantidade 2 --desconto 10 --cupom DEVOPS10
```

Saída esperada:
```text
176.90
```

### Parâmetros suportados:
- `--preco` (float, obrigatório): Preço unitário do produto.
- `--quantidade` (int, padrão: 1): Quantidade de unidades.
- `--desconto` (float, padrão: 0): Percentual de desconto base aplicado.
- `--cupom` (string, opcional): Cupom promocional válido (`DEVOPS10`, `BOASVINDAS5`).

---

## 🔒 Regras de Negócio Testadas

- Desconto cumulativo com limite seguro (soma de desconto manual + cupom restrita ao intervalo de 0% a 100%).
- Rejeição de cupons inválidos com exceção clara (`ValueError`).
- Validação de preços e quantidades negativas ou nulas.
- Arredondamento monetário preciso com duas casas decimais.
