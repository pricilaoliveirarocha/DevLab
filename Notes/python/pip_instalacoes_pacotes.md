# PIP — Guia rápido de consulta

> **PIP** é o gerenciador de pacotes do Python. Instala, atualiza e remove bibliotecas, normalmente obtidas do [PyPI](https://pypi.org/).

## Verificar instalação

```powershell
py -3 --version
py -3 -m pip --version
```

> No Windows, prefira `py -3 -m pip` para usar o PIP do Python 3 selecionado. No Linux/macOS, normalmente use `python3 -m pip`.

## Comandos essenciais

| Objetivo | Comando |
| --- | --- |
| Instalar pacote | `python -m pip install requests` |
| Instalar versão específica | `python -m pip install requests==2.31.0` |
| Atualizar pacote | `python -m pip install --upgrade requests` |
| Desinstalar pacote | `python -m pip uninstall requests` |
| Listar pacotes | `python -m pip list` |
| Consultar pacote | `python -m pip show requests` |
| Verificar dependências incompatíveis | `python -m pip check` |

**Nota:** Os comandos acima usam `python` do ambiente ativo. Sem ambiente virtual, no Windows você pode substituir `python -m pip` por `py -3 -m pip`.

## Ambiente virtual (`venv`)

Isola as dependências de cada projeto e evita conflitos entre versões.

```powershell
# Criar (Windows)
py -3 -m venv .venv

# Ativar no PowerShell
.\.venv\Scripts\Activate.ps1

# Instalar pacote no ambiente ativo
python -m pip install requests

# Desativar
deactivate
```

**Outros terminais:**

```bash
# Windows — Prompt de Comando (cmd)
.venv\Scripts\activate.bat

# Windows — Git Bash
source .venv/Scripts/activate

# Linux/macOS — criar e ativar
python3 -m venv .venv
source .venv/bin/activate
```

## Dependências: `requirements.txt`

```bash
# Exportar pacotes do ambiente ativo
python -m pip freeze > requirements.txt

# Reinstalar dependências do projeto
python -m pip install -r requirements.txt
```

Exemplo de conteúdo:

```text
requests==2.31.0
urllib3==2.0.5
```

`pip freeze` registra **todos os pacotes instalados no ambiente**, inclusive dependências transitivas. Por isso, gere o arquivo em um `.venv` dedicado e revise o resultado antes do commit.

## Boas práticas

- Crie um `.venv` por projeto e adicione `.venv/` ao `.gitignore`.
- Versione o `requirements.txt`, **não** a pasta do ambiente virtual.
- Use `python -m pip` para garantir que o PIP corresponde ao interpretador ativo.
- Evite instalar bibliotecas globalmente quando forem dependências de um projeto.
- Se `pip` ou `pip3` não for reconhecido, tente `py -3 -m pip` no Windows: pode funcionar sem alterar o `PATH`.

**Fluxo para lembrar:** criar `.venv` → ativar → instalar → exportar `requirements.txt` → versionar.
