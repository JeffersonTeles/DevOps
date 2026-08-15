# DevOps - Aula 03: Testes automatizados e feedback local

Este projeto continua o sistema de pedidos usado nas aulas anteriores, mas evita Git e GitHub. O objetivo e criar um ciclo local de feedback tecnico com testes, cobertura, lint e um comando unico de qualidade.

## Preparar o ambiente

### Linux/macOS

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements-dev.txt
```

### Windows PowerShell

```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements-dev.txt
```

## Executar manualmente

```bash
python -m app.cli --preco 100 --quantidade 2 --desconto 10 --cupom DEVOPS10
```

Saida esperada:

```text
176.90
```

## Executar verificacoes

```bash
pytest -q
pytest --cov=app --cov-report=term-missing
ruff check .
make quality
```

Em Windows sem `make`, execute:

```powershell
ruff check .
pytest --cov=app --cov-report=term-missing
```

## Estrutura

```text
app/
  pedidos.py       regra de negocio testavel
  cli.py           entrada manual pelo terminal
tests/
  test_pedidos.py  testes unitarios e de regressao
  test_cli.py      teste de integracao simples da CLI
pyproject.toml     configuracao de pytest, coverage e ruff
requirements-dev.txt dependencias de desenvolvimento
Makefile           atalhos de automacao local
```


## Pipeline visual no GitHub Actions

A pasta `.github/workflows/testes.yml` contém uma pipeline ordenada para visualizar o fluxo de qualidade no GitHub Actions:

1. `1 - Testes unitarios` executa os testes de regra de negócio.
2. `2 - Cobertura de testes` roda o relatório de cobertura.
3. `3 - Analise estatica Ruff` verifica problemas de estilo e erros simples.
4. `4 - Smoke test CLI` executa o programa pelo terminal.
5. `5 - Gate final de qualidade` consolida o resultado dos jobs anteriores.

Para visualizar o grafo, envie o projeto para um repositório GitHub e abra:

```text
Actions -> Testes e Qualidade -> execução mais recente
```

O objetivo didático é enxergar a ordem: testar regra de negócio, medir cobertura, verificar qualidade estática, fazer smoke test e liberar o gate final.

## Comandos recomendados

```powershell
python -m pytest -q
python -m pytest --cov=app --cov-report=term-missing
ruff check .
python -m app.cli --preco 100 --quantidade 2 --desconto 10 --cupom DEVOPS10
```
