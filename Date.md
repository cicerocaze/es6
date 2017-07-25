# ES6 - Trabalhando com Datas

***Obtendo a data atual com o objeto Date***
* O JavaScript possui um objeto especial para representar datas, podemos obter uma data com base na data e hora atual da seguinte forma:
```javascript
let hoje = new Date();
```

***Obtendo uma data específica com o objeto Date***
* O objeto Date possui alguns contrutores de modo a facilitar a nossa vida na hora de setarmos uma data específica, veja algumas formas de utiliza-los:
```javascript
let hoje = new Date('2016/05/17');
let outraDataOutraManeira = new Date(2016, 4, 17);
let outraDataOutraManeiraComString = new Date("2016", "4", "17");
```

***Problemas com o objeto Date***
* O objeto Date guarda internamente os meses de 0 a 11, por isso devemos ter cuidado ao informar o mês desejado.
```javascript
let outraData = new Date('2016/05/17'); 
console.log(outraData.getDate()); // imprime 17
console.log(outraData.getMonth()); // imprime 4
console.log(outraData.getFullYear()); // imprime 2016
```

***Trabalhando com data no formato brasileiro***
* O objeto Date trabalha com datas no formato americano (ano/mes/dia), veja um exemplo de como criar uma data à partir de uma string de data no formato brasileiro:
```javascript
let dataString = '17-05-2016';
dataString = dataString.split('-').reverse().join('/');
let data = new Date(dataString);
console.log(data);
```

Ou de uma forma mais abreviada
```javascript
let dataString = '17-05-2016';
let data = new Date(dataString.split('-').reverse().join('/'));
console.log(data);
```
