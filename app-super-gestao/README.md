<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400" alt="Laravel Logo"></a></p>


## Expressões Regulares
Tratando parâmetros de rotas com expressões regulares:

```php
Route::get(
    '/contato/{nome?}/{categoria_id}',
    function (
        string $nome = 'Desconhecido', 
        int $categoria_id = 1 // 1 - 'Informação'
    ) {
        echo "Estamos aqui: $nome - $categoria_id";
    }
)->where('categoria_id', '[0-9]+')->where('nome', '[A-Za-z]+');
```
Dessa forma, a rota só vai dar match no back-end se o nome for caracteres de letras minusculas/maiúsculas entre A-Z e ter pelo menso 1 caractere.

Observe que o número também deve ser entre 0 e 9 e ter pelo menso 1 caractere.

## Verbos HTTP
Os verbos HTTP são utilizados para definir a ação que será realizada sobre o recurso. Os principais verbos HTTP são:

- GET
- POST
- PUT
- PATCH
- DELETE
- OPTIONS

## Agrupamento de rotas
```php
Route::prefix('app')->group(function () {
    Route::get('/', function () {
        return 'Página principal do app';
    });
    Route::get('profile', function () {
        return 'Página de perfil';
    });
    Route::get('about', function () {
        return 'Sobre';
    });
});
```
O agrupamento de rotas permite definir um prefixo para um grupo de rotas. Sendo assim facilita "isolar" certas rotas para um usuário logado, por exemplo.

## Nomeando Rotas
Possibilita facilitar a chamada de rotas em outras partes do site, pois não depende do caminho da rota, mas sim do nome da rota.

Criando e nomeando a rota no aquivo web.php:
```php
Route::get('/contato', [App\Http\Controllers\ContatoController::class,'contato'])->name('site.contato');
```
Chamando a rota nomeada em alguma parte do site:
```php
<a href="{{ route('site.contato') }}">Contato</a>
```

## Redirecionamento de Rotas
Permite redirecionar o usuário para outra rota. Pode ser usada caso a rota atual não exista ou não seja encontrada, ou quando o usuário fizer uma ação no site, levá-lo a uma página de erro ou sucesso, por exemplo.

Redirecionando rota por callback no arquivo web.php:
```php
Route::get('/rota2', function(){
    return redirect()->route('site.rota1');
})->name('site.rota2');
```

Redirecionando rota usando o Route Redirect no arquivo web.php:
```php
Route::redirect('/rota2', '/rota1');
```

Também pode ser utilizado no Controller (Forma mais utilizada!)

## Rota de contigência(fallback [ERRO 404 - Not Found])
Caso o usuário tente acessar uma rota que não exista, o Laravel irá redirecionar o usuário para a rota de contigência.

```php
Route::fallback(function(){
    echo 'A rota acessada não existe. <a href="'.route('site.index').'">Clique aqui</a> para voltar a página inicial.';
});
```

## Encaminhando parâmetros do Controller para a view
Existem 3 formas de encaminhar parâmetros do Controller para a view:
```php
// Array associativo
return view('site.teste', ['p1' => $p1, 'p2' => $p2]); // Array associativo

// Compact
return view('site.teste', compact('p1', 'p2')); // Compact

// With
return view('site.teste')->with('p1', $p1)->with('p2', $p2); // With
```

## Blade - Incluindo comentários e blocos PHP puros
```php
//Comentário
{{-- <p>Comentário</p> --}}

//Bloco PHP puro
<?php echo 'Olá'; ?>
//ou
<?= 'Olá'; ?>

//Outra forma de bloco PHP puro no blade
@php
    echo 'Olá';
@endphp

```
## Exemplo de if no Blade
```php
@if(count($fornecedores) > 0 && count($fornecedores) < 10)
    <h3>Existem alguns fornecedores cadastrados</h3>
@elseif(count($fornecedores) > 10)
    <h3>Existem vários fornecedores cadastrados</h3>
@else
    <h3>Ainda não existem fornecedores cadastrados</h3>
@endif
```

## Blade - @unless
Permite verificar se uma condição é falsa. Se for falsa, executa o bloco de código:

```php
@unless($fornecedores)//veriica se nao existe nada no array
    //Caso não exista fornecedores, exibe a mensagem:
    <h3>Não existem fornecedores cadastrados</h3>
@endunless //fim do unless  
```
## Blade - @isset
Serve para verificar se uma variável está definida. Se estiver, executa o bloco de código:

```php
@isset($fornecedores)//verifica se existe algo no array
    //Caso exista fornecedores, exibe a mensagem
    <h3>Existem fornecedores cadastrados</h3>
@endisset//fim do isset
```

## Laravel Sponsors
Acesse o link [Laravel Partners](https://partners.laravel.com).

### Autores
- **[Luys Fernnando](https://vehikl.com/)**
- **[Udemy](https://udemy.com)**
