# LinkPedia — Sistema de Cadastro de Links

## Sobre o projeto

O **LinkPedia** é um projeto desenvolvido em **Django** para a disciplina de **Desenvolvimento Web 3**, com foco na prática de **TDD (Test Driven Development)**.

A ideia principal do sistema é permitir que usuários autenticados possam acessar uma plataforma para cadastrar, listar, editar e remover links úteis.

O projeto começou com uma primeira sprint focada em autenticação e, na segunda sprint, evolui para a construção de um CRUD de links.

---

## Objetivo geral

Criar um sistema web simples, funcional e testável para gerenciamento de links.

O usuário deve conseguir:

- Fazer login usando e-mail institucional;
- Acessar a página inicial apenas se estiver logado;
- Cadastrar novos links;
- Listar links cadastrados;
- Editar links existentes;
- Excluir links cadastrados;
- Utilizar o sistema com proteção de acesso por login.

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
├── caso_uso.png
├── cobertura_testes.png
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
    │   └── tests/
    └── linkpedia/
        ├── settings.py
        ├── urls.py
        ├── asgi.py
        └── wsgi.py
```

---

## Model principal

O sistema trabalha com o modelo `LinkModel`.

```python
class LinkModel(models.Model):
    titulo = models.CharField(max_length=150)
    link = models.URLField(max_length=500)
    observacao = models.TextField(blank=True)

    def __str__(self):
        return f"{self.titulo} - {self.link}"
```

Esse model representa um link salvo no sistema.

Cada link possui:

| Campo | Função |
|---|---|
| `titulo` | Nome ou título do link |
| `link` | Endereço URL do site |
| `observacao` | Comentário ou descrição opcional |

---

## Sprint 1 — Metas propostas

Na primeira sprint, o foco principal foi criar a base de autenticação do sistema.

### Metas da Sprint 1

| Meta | Status |
|---|---|
| Criar tela de login | Concluído |
| Validar e-mail institucional `@cps.sp.gov.br` | Concluído |
| Autenticar usuário cadastrado | Concluído |
| Exibir página inicial após login | Concluído |
| Criar logout | Concluído |
| Proteger página inicial com login obrigatório | Concluído |
| Criar testes para login | Concluído |
| Manter cobertura de testes acima de 90% | Concluído na base inicial |

---

## Sprint 2 — Metas propostas

Na segunda sprint, o objetivo é implementar o CRUD de links.

### Metas da Sprint 2

| Meta | Status atual |
|---|---|
| Criar formulário para `LinkModel` | Parcialmente concluído |
| Criar tela de cadastro | Parcialmente concluído |
| Salvar links no banco de dados | Parcialmente concluído |
| Criar tela de listagem | Em desenvolvimento |
| Exibir links cadastrados | Pendente de finalização |
| Criar edição de links | Em desenvolvimento |
| Criar exclusão de links | Em desenvolvimento |
| Proteger CRUD com `login_required` | Parcialmente concluído |
| Criar testes para formulário de links | Pendente |
| Criar testes para cadastro/listagem/edição/exclusão | Pendente |
| Manter cobertura de testes acima de 90% | Pendente de nova validação |

---

## O que já foi feito até o momento

Até o momento, o projeto possui:

- Sistema de login funcional;
- Validação de e-mail institucional;
- Sistema de logout;
- Página inicial protegida;
- Model `LinkModel` criado;
- Testes para login;
- Testes para o model de links;
- Formulário `LinkForm` iniciado;
- View de cadastro iniciada;
- Templates de cadastro, listagem, edição e exclusão criados como base;
- Rotas iniciais para cadastro, listagem, edição e exclusão.

---

## O que ainda precisa ser finalizado

Para concluir completamente a Sprint 2, ainda é necessário:

- Ajustar a view de listagem para buscar os links no banco;
- Ajustar a view de edição para receber o `id` do link;
- Ajustar a view de exclusão para receber o `id` do link;
- Atualizar as URLs de edição e exclusão com `<int:id>`;
- Fazer o template `listar.html` exibir os links cadastrados;
- Fazer o template `editar.html` mostrar o formulário preenchido;
- Fazer o template `excluir.html` confirmar a exclusão;
- Criar testes para o `LinkForm`;
- Criar testes para as views do CRUD;
- Rodar `coverage` novamente para verificar a cobertura.

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

Sugestão de dados conforme a proposta inicial:

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

Dentro da pasta onde está o `manage.py`, execute:

```bash
python manage.py test
```

Para rodar com cobertura:

```bash
coverage run --source='.' manage.py test
coverage html
```

Depois abra o arquivo:

```text
htmlcov/index.html
```

---

## Resumo do projeto

O **LinkPedia** é um sistema Django criado para praticar desenvolvimento com testes.  
A primeira etapa focou no login e controle de acesso.  
A segunda etapa está focada na criação de um CRUD completo de links.

A meta final é ter um sistema onde apenas usuários logados possam cadastrar, visualizar, editar e excluir links, mantendo uma boa cobertura de testes automatizados.
