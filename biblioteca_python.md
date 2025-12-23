# Como Criar Bibliotecas Python

## Estrutura Básica de uma Biblioteca

Uma biblioteca Python típica possui a seguinte estrutura de diretórios:

```
minha_biblioteca/
├── setup.py                 # Configuração de instalação (método tradicional)
├── pyproject.toml          # Configuração moderna (recomendado)
├── README.md               # Documentação do projeto
├── LICENSE                 # Arquivo de licença
├── requirements.txt        # Dependências do projeto
├── minha_biblioteca/       # Diretório principal do código fonte
│   ├── __init__.py        # Torna o diretório um pacote Python
│   ├── modulo1.py         # Módulos com funcionalidades
│   ├── modulo2.py
│   └── utils/             # Subpacotes (opcional)
│       ├── __init__.py
│       └── helpers.py
└── tests/                  # Diretório de testes
    ├── __init__.py
    ├── test_modulo1.py
    └── test_modulo2.py
```

## Componentes Essenciais

### 1. `__init__.py`

Arquivo que marca um diretório como pacote Python. Pode estar vazio ou conter inicializações:

```python
"""Minha Biblioteca - Descrição breve"""

from .modulo1 import funcao_principal
from .modulo2 import ClassePrincipal

__version__ = "0.1.0"
__author__ = "Seu Nome"
__all__ = ["funcao_principal", "ClassePrincipal"]
```

### 2. `setup.py` (Método Tradicional)

Arquivo de configuração para instalação usando setuptools:

```python
from setuptools import setup, find_packages

with open("README.md", "r", encoding="utf-8") as fh:
    long_description = fh.read()

setup(
    name="minha_biblioteca",
    version="0.1.0",
    author="Seu Nome",
    author_email="seu.email@example.com",
    description="Uma breve descrição da biblioteca",
    long_description=long_description,
    long_description_content_type="text/markdown",
    url="https://github.com/usuario/minha_biblioteca",
    packages=find_packages(),
    classifiers=[
        "Programming Language :: Python :: 3",
        "License :: OSI Approved :: MIT License",
        "Operating System :: OS Independent",
    ],
    python_requires=">=3.7",
    install_requires=[
        "numpy>=1.20.0",
        "requests>=2.25.0",
    ],
    extras_require={
        "dev": ["pytest>=6.0", "black", "flake8"],
    },
)
```

### 3. `pyproject.toml` (Método Moderno - Recomendado)

Configuração moderna seguindo PEP 517 e PEP 518:

```toml
[build-system]
requires = ["setuptools>=61.0", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "minha_biblioteca"
version = "0.1.0"
authors = [
    {name = "Seu Nome", email = "seu.email@example.com"}
]
description = "Uma breve descrição da biblioteca"
readme = "README.md"
requires-python = ">=3.7"
classifiers = [
    "Programming Language :: Python :: 3",
    "License :: OSI Approved :: MIT License",
    "Operating System :: OS Independent",
]
dependencies = [
    "numpy>=1.20.0",
    "requests>=2.25.0",
]

[project.optional-dependencies]
dev = ["pytest>=6.0", "black", "flake8"]

[project.urls]
Homepage = "https://github.com/usuario/minha_biblioteca"
Documentation = "https://minha_biblioteca.readthedocs.io"
Repository = "https://github.com/usuario/minha_biblioteca.git"
```

### 4. Exemplo de Módulo

**modulo1.py:**

```python
"""Módulo com funcionalidades principais"""

def funcao_principal(parametro):
    """
    Descrição da função.
    
    Args:
        parametro (str): Descrição do parâmetro
        
    Returns:
        str: Descrição do retorno
        
    Raises:
        ValueError: Se o parâmetro for inválido
    """
    if not parametro:
        raise ValueError("Parâmetro não pode ser vazio")
    
    return f"Processado: {parametro}"


class ClassePrincipal:
    """Classe com funcionalidades principais"""
    
    def __init__(self, valor):
        """Inicializa a classe"""
        self.valor = valor
    
    def processar(self):
        """Processa o valor"""
        return self.valor * 2
```

## Passos para Criar uma Biblioteca

### 1. Criar a Estrutura de Diretórios

```bash
mkdir minha_biblioteca
cd minha_biblioteca
mkdir minha_biblioteca tests
touch minha_biblioteca/__init__.py
touch tests/__init__.py
touch README.md LICENSE pyproject.toml
```

### 2. Desenvolver o Código

Escreva seus módulos Python com funções e classes bem documentadas usando docstrings.

### 3. Escrever Testes

**tests/test_modulo1.py:**

```python
import pytest
from minha_biblioteca.modulo1 import funcao_principal, ClassePrincipal

def test_funcao_principal():
    resultado = funcao_principal("teste")
    assert resultado == "Processado: teste"

def test_funcao_principal_vazio():
    with pytest.raises(ValueError):
        funcao_principal("")

def test_classe_principal():
    obj = ClassePrincipal(5)
    assert obj.processar() == 10
```

### 4. Instalar Localmente para Desenvolvimento

```bash
# Instalar em modo editável
pip install -e .

# Ou com dependências de desenvolvimento
pip install -e ".[dev]"
```

### 5. Testar a Biblioteca

```bash
# Executar testes
pytest

# Verificar cobertura
pytest --cov=minha_biblioteca

# Verificar estilo de código
flake8 minha_biblioteca
black minha_biblioteca
```

## Publicação no PyPI

### 1. Preparar para Publicação

```bash
# Instalar ferramentas necessárias
pip install build twine

# Construir o pacote
python -m build
```

Isso criará os arquivos em `dist/`:
- `minha_biblioteca-0.1.0.tar.gz` (source distribution)
- `minha_biblioteca-0.1.0-py3-none-any.whl` (wheel)

### 2. Testar no TestPyPI (Recomendado)

```bash
# Fazer upload para TestPyPI
twine upload --repository testpypi dist/*

# Instalar do TestPyPI para testar
pip install --index-url https://test.pypi.org/simple/ minha_biblioteca
```

### 3. Publicar no PyPI Real

```bash
# Fazer upload para PyPI
twine upload dist/*
```

### 4. Instalar a Biblioteca Publicada

```bash
pip install minha_biblioteca
```

## Boas Práticas

### 1. Versionamento Semântico

Use versionamento semântico (SemVer): `MAJOR.MINOR.PATCH`

- **MAJOR**: Mudanças incompatíveis na API
- **MINOR**: Novas funcionalidades compatíveis
- **PATCH**: Correções de bugs

### 2. Documentação

- Escreva docstrings para todas as funções e classes
- Use ferramentas como Sphinx para gerar documentação
- Mantenha um README.md completo
- Inclua exemplos de uso

### 3. Testes

- Escreva testes para todas as funcionalidades
- Use pytest como framework de testes
- Mantenha cobertura de código alta (>80%)
- Configure CI/CD (GitHub Actions, Travis, etc.)

### 4. Controle de Qualidade

```bash
# Formatação de código
black minha_biblioteca

# Linting
flake8 minha_biblioteca
pylint minha_biblioteca

# Type checking
mypy minha_biblioteca

# Ordenação de imports
isort minha_biblioteca
```

### 5. Arquivo `.gitignore`

```gitignore
# Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
*.egg-info/
.installed.cfg
*.egg

# Virtual environments
venv/
ENV/
env/

# IDE
.vscode/
.idea/
*.swp
*.swo

# Testing
.pytest_cache/
.coverage
htmlcov/

# OS
.DS_Store
Thumbs.db
```

## Ferramentas Úteis

### Poetry (Gerenciador de Dependências Moderno)

```bash
# Instalar Poetry
curl -sSL https://install.python-poetry.org | python3 -

# Criar novo projeto
poetry new minha_biblioteca

# Adicionar dependências
poetry add numpy requests

# Instalar dependências
poetry install

# Publicar no PyPI
poetry publish --build
```

### Cookiecutter (Templates de Projeto)

```bash
# Instalar cookiecutter
pip install cookiecutter

# Criar projeto a partir de template
cookiecutter https://github.com/audreyr/cookiecutter-pypackage
```

## Exemplo Completo Mínimo

**pyproject.toml:**
```toml
[build-system]
requires = ["setuptools>=61.0"]
build-backend = "setuptools.build_meta"

[project]
name = "exemplo_lib"
version = "0.1.0"
dependencies = []
```

**exemplo_lib/__init__.py:**
```python
def ola(nome):
    return f"Olá, {nome}!"

__version__ = "0.1.0"
```

**Uso:**
```bash
pip install -e .
```

```python
from exemplo_lib import ola
print(ola("Mundo"))  # Saída: Olá, Mundo!
```

## Recursos Adicionais

- [Packaging Python Projects - Tutorial Oficial](https://packaging.python.org/tutorials/packaging-projects/)
- [PyPI - Python Package Index](https://pypi.org/)
- [Setuptools Documentation](https://setuptools.pypa.io/)
- [Poetry Documentation](https://python-poetry.org/docs/)
- [Real Python - Publishing Python Packages](https://realpython.com/pypi-publish-python-package/)
