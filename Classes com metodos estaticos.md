# ES6 - Métodos Estáticos

* Para criar um classe com método estáticos, basta apenas assinar o método com a palavra static, veja um exemplo:

```javascript
class Pessoa {

    constructor(nome, sobrenome) {
        this.nome = nome;
        this.sobreNome = sobrenome;
    }

    obterNomeCompleto() {
        return `${this.nome} ${this.sobrenome}`;
    }

    static metodoStaticoQualquer() {
        console.log('Método estático chamado');
    }

}
```

* Agora para realizarmos a chamada basta executar a chamada do método diretamente da classe:
```javascript
Pessoa.metodoStaticoQualquer();
```
