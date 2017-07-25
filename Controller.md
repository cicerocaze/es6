# ES6 - Criação de Controllers

***Criando um controller***
```javascript
class ContadorController {
    constructor() {
        this._contador = 0;
        alert(this._contador);
    }

    get contador() {
        return this._contador;
    }

    incrementar() {
        this._contador++;
        document.querySelector('#p1').textContent = this._contador;
        alert(this._contador);
    }
}
```

***Associando um controller com uma tela***
```html
<html>
  <head></head>
  <body>
    <button onclick="contadorController.incrementar()">Incrementar</button>
    <script src="ContadorController.js"></script>
    <script>
        let contadorController = new ContadorController();
    </script>
  </body>
</html>
```

***Melhorando a performance do seu controller***
* No código a seguir temos um controller que é chamado pela view, executa um método que atualiza nossa view. Tudo funciona bem aqui, o problema é que toda vez que execurtamos o método incrementa, faremos uma busca no DOM pelo parágrafo desejado.

* É como se você tivesse que assinar um documento e a caneta estivesse na sala ao lado, você vai na sala ao lado para pegar a caneta, volta na sua sala para assinar o papel e depois guarada a caneta na sala ao lado novamente. Se precisar assinar outro documento fará o mesmo processo novamente.

```javascript
class ContadorController {
    constructor() {
        this._contador = 0;
        alert(this._contador);
    }

    get contador() {
        return this._contador;
    }

    incrementa() {
        this._contador++;
        document.querySelector('#p1').textContent = this._contador;
    }
}
```

```html
<html>
    <head></head>
    <body>
        <p id="p1">0</p>
        <button onclick="contadorController.incrementa()">Incrementar</button>
        <script src="ContadorController.js"></script>
        <script>
            let contadorController = new ContadorController();
        </script>
    </body>
</html>
```
* Para melhorar a performance do seu controller, você pode guardar a referência do elemento a ser atualizado em uma propriedade da classe. Assim toda vez que você precisar atualizar aquele elemento, você realizará a busca pelo elemento apenas na primeira vez. O código ficaria assim:

```javascript
class ContadorController {
    constructor() {
        this._contador = 0;
        this._elemento = document.querySelector('#p1'); // busca uma única vez
        alert(this._contador);
    }

    get contador() {
        return this._contador;
    }

    incrementa() {
        this._contador++;
        this._elemento.textContent = this._contador; // não precisa buscar o elemento, já temos uma referência para ele
    }
}
```
