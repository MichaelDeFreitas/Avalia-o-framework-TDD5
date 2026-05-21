# LinkPedia — Sistema de Cadastro de Links

## Sobre o projeto

O **LinkPedia** é um sistema web desenvolvido em **Django** para a disciplina de **Desenvolvimento Web 3**, com foco na prática de **TDD (Test Driven Development)**.

A proposta do projeto é permitir que usuários autenticados possam cadastrar, listar, editar e excluir links úteis em uma plataforma simples, organizada e protegida por login.

O projeto foi desenvolvido em duas etapas principais:

- **Sprint 1:** criação da base de autenticação, login, logout e página inicial protegida.
- **Sprint 2:** implementação do CRUD completo de links, com testes automatizados e validação de cobertura.

---

## Objetivo geral

Criar um sistema web funcional e testável para gerenciamento de links, utilizando Django e testes automatizados.

O usuário deve conseguir:

- Fazer login com e-mail institucional;
- Acessar a página inicial apenas se estiver autenticado;
- Cadastrar novos links;
- Listar links cadastrados;
- Editar links existentes;
- Excluir links cadastrados;
- Utilizar o sistema com rotas protegidas por login;
- Validar o funcionamento do sistema por meio de testes automatizados.

---

## Tecnologias utilizadas

- Python
- Django
- SQLite
- HTML
- CSS
- Bootstrap
- Font Awesome
- Coverage.py
- Testes automatizados com `django.test`

---

## Estrutura principal do projeto

```text
Avaliação_P2/
├── README.md
├── requirements.txt
├── cobertura_testes.png
├── caso_uso.png
├── index.png
├── login.png
├── logout.png
├── model.png
└── linkpedia/
    ├── manage.py
    ├── links.sqlite3
    ├── core/
    │   ├── models.py
    │   ├── forms.py
    │   ├── views.py
    │   ├── urls.py
    │   ├── templates/
    │   │   ├── login.html
    │   │   ├── logout.html
    │   │   ├── index.html
    │   │   ├── cadastro.html
    │   │   ├── listar.html
    │   │   ├── editar.html
    │   │   └── excluir.html
    │   └── tests/
    │       ├── test_cadastro.py
    │       ├── test_excluir.py
    │       ├── test_form_login.py
    │       ├── test_linkform.py
    │       ├── test_listar_editar.py
    │       ├── test_login_logout.py
    │       └── test_model_agenda.py
    └── linkpedia/
        ├── settings.py
        ├── urls.py
        ├── asgi.py
        └── wsgi.py
```

---

## Model principal

O sistema utiliza o modelo `LinkModel`, responsável por representar os links cadastrados.

```python
class LinkModel(models.Model):
    titulo = models.CharField(max_length=150)
    link = models.URLField(max_length=500)
    observacao = models.TextField(blank=True)

    def __str__(self):
        return f"{self.titulo} - {self.link}"
```

Cada link possui:

| Campo | Função |
|---|---|
| `titulo` | Nome ou título do link |
| `link` | Endereço URL salvo |
| `observacao` | Comentário ou descrição opcional |

---

## Sprint 1 — Base inicial do sistema

Na primeira sprint, o foco foi criar a estrutura inicial do sistema, principalmente a autenticação.

### Metas da Sprint 1

| Meta | Status |
|---|---|
| Criar tela de login | Concluído |
| Validar e-mail institucional `@cps.sp.gov.br` | Concluído |
| Autenticar usuário cadastrado | Concluído |
| Criar página inicial | Concluído |
| Proteger página inicial com login obrigatório | Concluído |
| Criar logout | Concluído |
| Criar testes para login e logout | Concluído |
| Manter boa cobertura de testes | Concluído |

---

## Sprint 2 — CRUD de links

Na segunda sprint, o sistema foi expandido para permitir o gerenciamento completo dos links.

### Metas da Sprint 2

| Meta | Status |
|---|---|
| Criar formulário para `LinkModel` | Concluído |
| Criar tela de cadastro de links | Concluído |
| Salvar links no banco de dados | Concluído |
| Criar tela de listagem | Concluído |
| Exibir links cadastrados | Concluído |
| Criar edição de links | Concluído |
| Atualizar links existentes | Concluído |
| Criar exclusão de links | Concluído |
| Confirmar exclusão antes de remover | Concluído |
| Proteger CRUD com `login_required` | Concluído |
| Criar testes para formulário de links | Concluído |
| Criar testes para cadastro | Concluído |
| Criar testes para listagem | Concluído |
| Criar testes para edição | Concluído |
| Criar testes para exclusão | Concluído |
| Rodar Coverage e validar cobertura | Concluído |

---

## Funcionalidades implementadas

### Autenticação

O sistema possui login com validação de e-mail institucional.

O usuário precisa utilizar um e-mail com o domínio:

```text
@cps.sp.gov.br
```

Após o login, o usuário é direcionado para a página inicial do sistema.

---

### Página inicial

A página inicial apresenta as principais ações do sistema:

- Cadastrar link;
- Listar links;
- Acessar edição por meio da listagem;
- Acessar exclusão por meio da listagem;
- Fazer logout.

As opções de edição e exclusão direcionam para a listagem, pois é necessário escolher um link específico antes de alterar ou remover.

---

### Cadastro de links

A tela de cadastro permite criar um novo link informando:

- Título;
- URL;
- Observação.

Após salvar, o sistema redireciona o usuário para a página de listagem.

---

### Listagem de links

A página de listagem exibe todos os links cadastrados no banco de dados.

Cada item apresenta:

- Título;
- Link clicável;
- Observação;
- Botão para editar;
- Botão para excluir.

Caso não exista nenhum link cadastrado, a página exibe uma mensagem informativa.

---

### Edição de links

A tela de edição permite alterar os dados de um link já cadastrado.

A edição utiliza o `id` do link na URL:

```python
path('editar/<int:id>/', editar, name='editar')
```

Na view, o formulário é carregado com os dados atuais do link usando:

```python
form = LinkForm(instance=link)
```

E, ao salvar, os dados são atualizados usando:

```python
form = LinkForm(request.POST, instance=link)
```

---

### Exclusão de links

A exclusão possui uma página de confirmação.

O link só é removido do banco quando o usuário confirma a ação por meio de um `POST`.

A URL utilizada é:

```python
path('excluir/<int:id>/', excluir, name='excluir')
```

---

## Rotas principais

| Rota | Função |
|---|---|
| `/login/` | Tela de login |
| `/logout/` | Logout do usuário |
| `/` | Página inicial |
| `/cadastro/` | Cadastro de links |
| `/listar/` | Listagem de links |
| `/editar/<id>/` | Edição de um link |
| `/excluir/<id>/` | Exclusão de um link |

---

## Testes automatizados

O projeto possui testes automatizados para validar as principais funcionalidades.

Foram criados testes para:

- Login;
- Logout;
- Formulário de login;
- Formulário de links;
- Cadastro de links;
- Listagem de links;
- Edição de links;
- Exclusão de links;
- Model principal;
- Acesso protegido por login.

---

## Cobertura de testes

A cobertura foi validada com o **Coverage.py**.

Resultado obtido:

```text
TOTAL: 98%
```

Esse resultado indica que a maior parte do código principal foi coberta por testes automatizados.

Arquivos principais como `forms.py`, `models.py`, `views.py` e `urls.py` ficaram com cobertura completa.

### Comandos utilizados

```bash
coverage run --source='.' manage.py test
coverage report
coverage html
```

O relatório HTML pode ser acessado em:

```text
htmlcov/index.html
```

---

## Comparativo entre o projeto inicial e a versão atual

### Projeto inicial

A primeira versão do projeto continha principalmente:

- Login;
- Logout;
- Página inicial;
- Validação de e-mail institucional;
- Base de testes para autenticação;
- Model `LinkModel` já estruturado.

### Versão atual

A versão atual adiciona:

- CRUD completo de links;
- Formulário `LinkForm`;
- Cadastro de links no banco de dados;
- Listagem dinâmica dos links;
- Edição de registros existentes;
- Exclusão com confirmação;
- Templates próprios para cadastro, listagem, edição e exclusão;
- Rotas com `id` para editar e excluir;
- Proteção das páginas com `login_required`;
- Testes para todas as partes principais do CRUD;
- Coverage com resultado de 98%.

---

## Como executar o projeto

### 1. Criar ambiente virtual

No Windows:

```bash
python -m venv venv
```

### 2. Ativar ambiente virtual

```bash
.\venv\Scripts\activate
```

### 3. Instalar dependências

```bash
pip install -r requirements.txt
```

Caso o Coverage não esteja instalado:

```bash
pip install coverage
```

### 4. Entrar na pasta do projeto Django

```bash
cd linkpedia
```

### 5. Rodar migrações

```bash
python manage.py migrate
```

### 6. Criar superusuário

```bash
python manage.py createsuperuser
```

Sugestão de dados:

```text
Username: aluno
E-mail: seu e-mail institucional
Password: fatec
```

### 7. Rodar o servidor

```bash
python manage.py runserver
```

Depois acesse:

```text
http://127.0.0.1:8000/
```

---

## Como rodar os testes

Dentro da pasta onde está o arquivo `manage.py`, execute:

```bash
python manage.py test
```

Para rodar com Coverage:

```bash
coverage run --source='.' manage.py test
coverage report
```

Ou, caso o terminal não reconheça o comando `coverage`:

```bash
python -m coverage run --source='.' manage.py test
python -m coverage report
```

---

## Status final do projeto

| Item | Status |
|---|---|
| Login | Concluído |
| Logout | Concluído |
| Página inicial | Concluído |
| Cadastro de links | Concluído |
| Listagem de links | Concluído |
| Edição de links | Concluído |
| Exclusão de links | Concluído |
| Proteção por login | Concluído |
| Testes automatizados | Concluído |
| Coverage | 98% |

---

## Observações finais

O **LinkPedia** foi finalizado como um sistema simples e funcional para gerenciamento de links.

A Sprint 1 entregou a base de autenticação e acesso protegido.  
A Sprint 2 completou o CRUD de links, adicionando cadastro, listagem, edição e exclusão, além de testes automatizados para validar o comportamento do sistema.

O projeto atingiu uma cobertura de testes de **98%**, superando a meta de manter uma cobertura acima de 90%.
