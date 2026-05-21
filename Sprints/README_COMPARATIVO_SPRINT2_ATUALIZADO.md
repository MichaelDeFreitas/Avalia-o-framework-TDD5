# Comparativo — Projeto Inicial x Projeto Atualizado

Este documento apresenta as principais alterações realizadas no projeto **LinkPedia** em comparação com o projeto inicial recebido na Sprint 1.

---

## Situação do projeto inicial

O projeto inicial possuía a base da aplicação Django com foco em autenticação.

Funcionalidades já existentes na Sprint 1:

- Tela de login;
- Validação de e-mail institucional `@cps.sp.gov.br`;
- Logout;
- Página inicial após autenticação;
- Model `LinkModel` já definido;
- Testes iniciais de login, logout e index;
- Cobertura de testes acima de 90%.

Apesar de o model de links já existir, o sistema ainda não possuía o CRUD completo para manipular os links.

---

## Objetivo da atualização

A proposta da Sprint 2 era evoluir o sistema para conter um CRUD funcional de links.

O professor solicitou:

- Criar um formulário para o `LinkModel`;
- Cadastrar links;
- Listar links;
- Atualizar links;
- Remover links;
- Proteger as funcionalidades para usuários logados;
- Ajustar e criar testes;
- Manter a cobertura de testes acima de 90%.

---

## O que foi adicionado

### 1. Formulário de links

Foi criado o `LinkForm` em `core/forms.py`, utilizando `ModelForm`.

Antes, o projeto possuía apenas o formulário de login.

Depois, passou a possuir também:

```python
class LinkForm(ModelForm):
    class Meta:
        model = LinkModel
        fields = ('titulo', 'link', 'observacao')
```

Esse formulário permite criar e editar registros do `LinkModel`.

---

### 2. Cadastro de links

Foi adicionada a view `cadastro`, responsável por receber os dados do formulário e salvar um novo link no banco.

Fluxo implementado:

```text
GET  → mostra o formulário vazio
POST → valida os dados, salva o link e redireciona para a listagem
```

Após o cadastro, o usuário é direcionado para a página de listagem.

---

### 3. Listagem de links

Foi criada a view `listar`, responsável por buscar os links cadastrados no banco e enviar para o template.

```python
links = LinkModel.objects.all()
```

A tela de listagem passou a exibir:

- Título do link;
- URL cadastrada;
- Observação;
- Botão para editar;
- Botão para excluir;
- Mensagem quando não houver links cadastrados.

---

### 4. Edição de links

Foi adicionada a funcionalidade de edição usando o `id` do link.

URL criada:

```python
path('editar/<int:id>/', editar, name='editar')
```

A view busca o link selecionado, carrega o formulário com os dados já preenchidos e permite salvar as alterações.

O ponto principal da edição foi o uso de:

```python
instance=link
```

Isso garante que o sistema atualize o link existente em vez de criar um novo registro.

---

### 5. Exclusão de links

Foi criada a funcionalidade de exclusão com tela de confirmação.

URL criada:

```python
path('excluir/<int:id>/', excluir, name='excluir')
```

Fluxo implementado:

```text
GET  → mostra tela de confirmação
POST → remove o link e redireciona para a listagem
```

Esse comportamento evita que um link seja excluído apenas ao abrir a página.

---

### 6. Proteção das rotas

As páginas do CRUD foram protegidas com `@login_required`.

Rotas protegidas:

- Cadastro;
- Listagem;
- Edição;
- Exclusão.

Com isso, usuários não autenticados são redirecionados para login antes de acessar as funcionalidades internas.

---

### 7. Templates adicionados e atualizados

Foram criadas e/ou atualizadas as telas do sistema:

| Template | Função |
|---|---|
| `cadastro.html` | Formulário para cadastrar links |
| `listar.html` | Exibição dos links cadastrados |
| `editar.html` | Formulário para alterar links existentes |
| `excluir.html` | Confirmação antes da remoção |
| `index.html` | Atalhos para as funcionalidades do sistema |

As páginas foram padronizadas visualmente com:

- Bootstrap;
- Font Awesome;
- Cards centralizados;
- Gradiente de fundo;
- Botões organizados;
- Layout responsivo;
- Identidade visual do LinkPedia.

---

### 8. Testes adicionados

Foram adicionados testes para cobrir as novas funcionalidades da Sprint 2.

Arquivos de teste adicionados/atualizados:

```text
core/tests/test_cadastro.py
core/tests/test_excluir.py
core/tests/test_linkform.py
core/tests/test_listar_editar.py
```

Principais comportamentos testados:

- Cadastro exige login;
- Página de cadastro abre para usuário logado;
- Link válido é salvo;
- Link inválido não é salvo;
- Listagem exige login;
- Listagem mostra links cadastrados;
- Edição exige login;
- Edição carrega os dados existentes;
- Edição salva alterações válidas;
- Edição não salva URL inválida;
- Exclusão exige login;
- Tela de confirmação de exclusão abre corretamente;
- Exclusão por `POST` remove o link;
- Acesso por `GET` na exclusão não remove o link;
- Formulário de link possui os campos corretos;
- Observação pode ser vazia.

---

## Comparativo direto

| Item | Projeto inicial | Projeto atualizado |
|---|---|---|
| Login | Existia | Mantido |
| Logout | Existia | Mantido |
| Página inicial | Existia | Atualizada visualmente |
| Model de link | Existia | Mantido |
| Formulário de link | Não existia | Implementado |
| Cadastro de link | Não existia | Implementado |
| Listagem de links | Não existia | Implementada |
| Edição de links | Não existia | Implementada |
| Exclusão de links | Não existia | Implementada com confirmação |
| Proteção por login no CRUD | Não existia | Implementada |
| Testes do CRUD | Não existiam | Implementados |
| Cobertura de testes | Acima de 90% | 98% |
| Visual das páginas do CRUD | Não existia | Padronizado com Bootstrap |

---

## Resultado da cobertura

Após as implementações, os testes foram executados com Coverage.

Resultado final:

```text
TOTAL: 98%
```

Esse resultado confirma que a cobertura permaneceu acima da meta solicitada.

Comandos utilizados:

```bash
coverage run --source='.' manage.py test
coverage report
```

---

## Metas da Sprint 2

| Meta | Status |
|---|---|
| Criar formulário para `LinkModel` | Concluída |
| Cadastrar links | Concluída |
| Listar links | Concluída |
| Atualizar links | Concluída |
| Remover links | Concluída |
| Proteger com login | Concluída |
| Criar testes novos | Concluída |
| Manter cobertura acima de 90% | Concluída — 98% |

---

## Conclusão

Comparado ao projeto inicial, o LinkPedia evoluiu de uma aplicação com autenticação básica para um sistema com CRUD completo de links.

A Sprint 2 foi finalizada com:

- Funcionalidades principais implementadas;
- Rotas protegidas;
- Interface padronizada;
- Testes automatizados;
- Cobertura final de 98%.

Com isso, o projeto atende aos requisitos solicitados para a evolução da Sprint 2.
