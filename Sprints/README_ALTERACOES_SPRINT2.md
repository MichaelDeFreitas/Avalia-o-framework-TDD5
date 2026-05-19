# Alterações realizadas em relação ao projeto inicial

## Visão geral

Este documento descreve o que foi adicionado ou iniciado no projeto **LinkPedia** em comparação com a versão inicial da Sprint 1.

A versão inicial tinha como foco principal:

- Login;
- Logout;
- Página inicial protegida;
- Validação de e-mail institucional;
- Testes para autenticação;
- Cobertura de testes acima de 90%.

A nova etapa do projeto busca adicionar um **CRUD de links**.

---

## Comparação entre projeto inicial e projeto atual

| Área | Projeto inicial | Projeto atual |
|---|---|---|
| Login | Existente | Mantido |
| Logout | Existente | Mantido |
| Página inicial | Existente | Mantida |
| Validação de e-mail institucional | Existente | Mantida |
| Model de links | Existente como base | Mantido |
| Formulário de links | Não implementado | Adicionado parcialmente |
| Cadastro de links | Não implementado | Iniciado |
| Listagem de links | Não implementada | Estrutura criada |
| Edição de links | Não implementada | Estrutura criada |
| Exclusão de links | Não implementada | Estrutura criada |
| Templates do CRUD | Não existiam | Criados como base |
| Testes de CRUD | Não existiam | Ainda pendentes |
| Cobertura após CRUD | Não aplicável | Precisa ser validada |

---

## Arquivos adicionados ou modificados

### `core/forms.py`

Foi adicionado o formulário `LinkForm`, responsável por gerar um formulário baseado no model `LinkModel`.

```python
class LinkForm(ModelForm):
    class Meta:
        model = LinkModel 
        fields = ('titulo', 'link','observacao')
```

Esse formulário permite trabalhar com os campos:

- `titulo`
- `link`
- `observacao`

### Observação importante

O formulário foi iniciado corretamente, mas pode ser simplificado para evitar erros de validação desnecessários.

Versão mais simples recomendada:

```python
class LinkForm(ModelForm):
    class Meta:
        model = LinkModel
        fields = ('titulo', 'link', 'observacao')
```

---

### `core/views.py`

Foram adicionadas views para iniciar o CRUD:

- `cadastro`
- `listar`
- `editar`
- `excluir`

A view de cadastro já possui lógica para receber dados via POST e salvar o formulário:

```python
@login_required
def cadastro(request):
  if request.method == "POST":  
    form = LinkForm(request.POST)
    if form.is_valid():
      form.save()
      return redirect("home")
    context = {'form':form}
    return render(request, 'cadastro.html', context)
  else:
    form = LinkForm()
    context = {'form':form}
    return render(request, 'cadastro.html', context)
```

### Status atual das views

| View | Status |
|---|---|
| `cadastro` | Parcialmente funcional |
| `listar` | Precisa buscar os links no banco |
| `editar` | Precisa receber `id` e atualizar o link |
| `excluir` | Precisa receber `id` e excluir o link |

---

### `core/urls.py`

Foram adicionadas rotas para as novas páginas:

```python
path('cadastro/', cadastro, name='cadastro'),
path('listar/', listar, name='listar'),
path('editar/', editar, name='editar'),
path('excluir/', excluir, name='excluir'),
```

### Ajuste necessário

As rotas de editar e excluir precisam receber o `id` do link.

Versão recomendada:

```python
path('editar/<int:id>/', editar, name='editar'),
path('excluir/<int:id>/', excluir, name='excluir'),
```

---

### `core/templates/cadastro.html`

Foi criada uma tela para cadastro de links.

Essa tela deve conter o formulário:

```html
<form method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Salvar</button>
</form>
```

### Status atual

A tela existe, mas deve ser conferida para garantir que o formulário está aparecendo corretamente.

---

### `core/templates/listar.html`

Foi criada uma base para a tela de listagem.

### O que ainda precisa ser feito

A tela precisa receber a variável `links` da view e exibir os dados:

```html
{% for link in links %}
    <p>{{ link.titulo }}</p>
    <p>{{ link.link }}</p>
    <p>{{ link.observacao }}</p>

    <a href="{% url 'editar' link.id %}">Editar</a>
    <a href="{% url 'excluir' link.id %}">Excluir</a>
{% empty %}
    <p>Nenhum link cadastrado.</p>
{% endfor %}
```

---

### `core/templates/editar.html`

Foi criada uma base para a tela de edição.

### O que ainda precisa ser feito

A tela precisa mostrar o formulário preenchido com os dados do link selecionado:

```html
<form method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Salvar alterações</button>
</form>
```

---

### `core/templates/excluir.html`

Foi criada uma base para a tela de exclusão.

### O que ainda precisa ser feito

A tela precisa confirmar se o usuário deseja excluir o link:

```html
<p>Tem certeza que deseja excluir {{ link.titulo }}?</p>

<form method="POST">
    {% csrf_token %}
    <button type="submit">Confirmar exclusão</button>
</form>
```

---

## Testes existentes

Atualmente o projeto possui testes para:

### Login

Arquivo:

```text
core/tests/test_form_login.py
```

Testa:

- Campos do formulário de login;
- E-mail institucional;
- E-mail obrigatório;
- Usuário inexistente;
- Senha incorreta;
- Login válido.

### Model de links

Arquivo:

```text
core/tests/test_model_agenda.py
```

Testa:

- Criação de um `LinkModel`;
- Retorno do método `__str__`.

---

## Testes que ainda precisam ser adicionados

Para finalizar a Sprint 2, ainda é importante criar testes para:

### Formulário de links

Sugestão de arquivo:

```text
core/tests/test_form_link.py
```

Testes sugeridos:

- Verificar se o formulário possui os campos `titulo`, `link` e `observacao`;
- Verificar se dados válidos passam;
- Verificar se uma URL inválida gera erro.

---

### Views do CRUD

Sugestão de arquivo:

```text
core/tests/test_views_links.py
```

Testes sugeridos:

- Acessar tela de cadastro logado;
- Cadastrar um link via POST;
- Acessar tela de listagem;
- Verificar se um link aparece na listagem;
- Atualizar um link;
- Excluir um link;
- Verificar se páginas protegidas exigem login.

---

## Metas já batidas

Até o momento, foram batidas as seguintes metas:

- Login mantido;
- Logout mantido;
- Página inicial protegida mantida;
- Validação de e-mail institucional mantida;
- Model `LinkModel` existente;
- Formulário `LinkForm` iniciado;
- View de cadastro iniciada;
- Rotas básicas do CRUD criadas;
- Templates básicos do CRUD criados;
- Testes de login mantidos;
- Testes do model de links mantidos.

---

## Metas ainda pendentes

Ainda precisam ser finalizadas:

- Listagem real dos links cadastrados;
- Edição real de links;
- Exclusão real de links;
- Ajuste das URLs com `id`;
- Ajuste dos templates para mostrar dados reais;
- Criação dos testes do CRUD;
- Nova execução do coverage;
- Confirmação de cobertura acima de 90%.

---

## Próximos passos recomendados

A ordem recomendada para finalizar o projeto é:

```text
1. Simplificar e conferir o LinkForm
2. Finalizar a view de cadastro
3. Corrigir a view listar para buscar LinkModel.objects.all()
4. Corrigir editar usando id e instance
5. Corrigir excluir usando id e delete()
6. Ajustar urls.py com <int:id>
7. Ajustar listar.html para mostrar os links
8. Ajustar editar.html e excluir.html
9. Criar testes do formulário de link
10. Criar testes das views do CRUD
11. Rodar python manage.py test
12. Rodar coverage
```

---

## Resumo das mudanças

O projeto inicial entregava a base de autenticação.  
A versão atual começou a adicionar o CRUD de links, criando formulário, rotas, views e templates iniciais.

A Sprint 2 ainda precisa ser finalizada para que o CRUD esteja totalmente funcional e coberto por testes.
