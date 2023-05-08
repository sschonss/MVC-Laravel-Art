# O que é MVC?
---

MVC é um padrão de arquitetura de software que separa a aplicação em três partes: Model, View e Controller.

Hoje em dia, o MVC é um dos padrões mais utilizados para o desenvolvimento de aplicações web em diversas linguagens de programação, incluindo o PHP.

Agora vamos entender melhor cada uma das partes do MVC.

---

## Model

O Model é a parte da aplicação que representa a camada de dados, ou seja, a representação dos dados que são manipulados pela aplicação.

É literalmente um modelo da aplicação, que pode ser representado por uma classe, por exemplo.

Seguindo o exemplo de um sistema de cadastro de clientes, o Model seria a representação dos dados do cliente, como nome, e-mail, telefone, etc.

Segue o exemplo de um Model de cliente em Laravel:

```php

<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Cliente extends Model
{
    use HasFactory;

    protected $fillable = [
        'nome',
        'email',
        'telefone',
    ];
}

```

Dentro do Model, podemos definir regras de negócio, como funções para validar os dados, por exemplo.

```php

public function adicionar($dados)
    {
        return $this->create($dados);
    }

    public function atualizar($dados)
    {
        return $this->update($dados);
    }

    public function excluir()
    {
        return $this->delete();
    }

    public function buscar($id)
    {
        return $this->find($id);
    }

    public function listar()
    {
        return $this->all();
    }

```

Lembre-se que esse código é apenas um exemplo didático, e que você pode implementar as regras de negócio da forma que achar melhor.

---

## Controller

Pronto, já temos nosso modelo!

Como usamos ele agora? Com o Controller.

O Controller é literalmente o controlador da aplicação, que recebe as requisições do usuário e as manipula, retornando uma resposta.

No nosso exemplo, o Controller seria responsável por receber as requisições do usuário, como por exemplo, salvar um novo cliente, e manipular os dados, como por exemplo, salvar o cliente no banco de dados.

Segue o exemplo de um Controller de cliente em Laravel:

```php

[...]

class ClienteController extends Controller
{
    public function adicionar(Request $request)
    {
        $cliente = new Cliente();
        $cliente->adicionar($request->all());
        return response()->json(['mensagem' => 'Cliente adicionado com sucesso!']);
    }

    public function atualizar(Request $request, $id)
    {
        $cliente = Cliente::find($id);
        $cliente->atualizar($request->all());
        return response()->json(['mensagem' => 'Cliente atualizado com sucesso!']);
    }

    public function excluir($id)
    {
        $cliente = Cliente::find($id);
        $cliente->excluir();
        return response()->json(['mensagem' => 'Cliente excluído com sucesso!']);
    }

    public function buscar($id)
    {
        $cliente = Cliente::find($id);
        return response()->json($cliente);
    }

    public function listar()
    {
        $clientes = Cliente::listar();
        return response()->json($clientes);
    }
}

```

O Controller conhece o Model, e pode manipular os dados através dele, mas também conhece a View, e pode retornar uma resposta para o usuário através dela.

Dessa forma, o Controller é o intermediário entre o Model e a View.

---

## View

A View é a parte da aplicação que representa a camada de apresentação, ou seja, a representação da interface gráfica da aplicação.

É literalmente a interface gráfica da aplicação, que pode ser representada por um arquivo HTML, por exemplo.

Seguindo o exemplo de um sistema de cadastro de clientes, a View seria a representação da interface gráfica do sistema, como formulários, botões, tabelas, etc.

Segue o exemplo de uma View de cliente em Laravel utilizando o Blade:

```html

<!DOCTYPE html>

<html lang="pt-br">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Clientes</title>

    <link rel="stylesheet" href="{{ asset('css/app.css') }}">

    <script src="{{ asset('js/app.js') }}"></script>

</head>

<body>

    <div class="container">

        <div class="row">

            <div class="col-md-12">

                <h1>Clientes</h1>

                <table class="table table-striped">

                    <thead>
                        <tr>
                            <th>Nome</th>
                            <th>E-mail</th>
                            <th>Telefone</th>
                            <th>Ações</th>
                        </tr>
                    </thead>

                    <tbody>
                        @foreach ($clientes as $cliente)
                        <tr>
                            <td>{{ $cliente->nome }}</td>
                            <td>{{ $cliente->email }}</td>
                            <td>{{ $cliente->telefone }}</td>
                            <td>
                                <a href="{{ route('clientes.edit', $cliente->id) }}" class="btn btn-primary">Editar</a>
                                <a href="{{ route('clientes.destroy', $cliente->id) }}" class="btn btn-danger">Excluir</a>
                            </td>
                        </tr>
                        @endforeach
                    </tbody>

                </table>

                <a href="{{ route('clientes.create') }}" class="btn btn-success">Novo cliente</a>

            </div>

        </div>

    </div>

</body>

</html>

```

Lembre-se que esse código é apenas um exemplo didático, e que você pode implementar a interface gráfica da forma que achar melhor e com a tecnologia que quiser.

Talvez você tenha percebido que a View está utilizando uma sintaxe diferente, com o uso de chaves e arrobas, isso é o Blade, um template engine do Laravel.

O Blade é uma ferramenta que permite a utilização de recursos como herança de templates, diretivas de controle, etc.

Outra coisa que você pode ter percebido é o uso de rotas, como por exemplo, `{{ route('clientes.create') }}`, isso é o uso de rotas nomeadas, que é uma forma de referenciar uma rota através de um nome, ao invés de referenciar através de uma URL.

---

## Rotas

As rotas são responsáveis por mapear as requisições do usuário e direcioná-las para o Controller.

No nosso exemplo, as rotas seriam responsáveis por mapear as requisições do usuário, como por exemplo, salvar um novo cliente, e direcioná-las para o Controller, como por exemplo, salvar o cliente no banco de dados.

Segue o exemplo de rotas de cliente em Laravel:

```php

[...]

Route::post('/clientes', [ClienteController::class, 'adicionar']);
Route::put('/clientes/{id}', [ClienteController::class, 'atualizar']);
Route::delete('/clientes/{id}', [ClienteController::class, 'excluir']);
Route::get('/clientes/{id}', [ClienteController::class, 'buscar']);
Route::get('/clientes', [ClienteController::class, 'listar']);

```

Lembre-se que esse código é apenas um exemplo didático, e que você pode implementar as rotas da forma que achar melhor.

---
## Informações adicionais

Dentro do Laravel, existe uma pasta chamada `app`, que contém as pastas `Http`, `Models` e `Views`, que são as pastas que representam o MVC.

A rotas no Laravel são definidas no arquivo `routes/web.php`.

As funções tem os nomes de 'index', 'create', 'store', 'show', 'edit', 'update' e 'destroy', que são os nomes padrões das funções de um Controller, porém, você pode criar funções com outros nomes, como por exemplo, 'adicionar', 'atualizar', 'excluir', 'buscar' e 'listar', como no nosso exemplo.

---

## Conclusão

Pronto, agora você já sabe o básico sobre MVC, e já pode começar a implementar em seus projetos.

Lembre-se que esse é apenas um exemplo didático, existem várias verificações que devem ser feitas, como por exemplo, verificar se o usuário está autenticado, verificar se o usuário tem permissão para acessar determinada rota, etc.

Eu usei o Laravel como exemplo, mas você pode implementar o MVC com a tecnologia que quiser, como por exemplo, PHP puro, Java, C#, etc.

Espero que esse artigo tenha sido útil para você, como foi para mim.

Obrigado por ler até aqui, e até a próxima!

---




