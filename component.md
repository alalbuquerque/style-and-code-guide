# Atomic Design Pattern - Componentes

introdução

## Bósons 
Bósons são as menores unidades que irão declarar os atributos de estilo dos átomos. Esses bósons são usado como placeholders ou mixins.

### Cor 
Cores em bosons/colors.scss

```scss
  %color-primary {
    color: #FF0000
  }
  .title {
    @extend color-primary
   }
   
   //compilado
   
   
```

### Typography 
Cores em bosons/typografy.scss

```scss
  %h1 {
    font-size: 16px;
  }
  .title {
    @extend color-primary
   }
```

### Responsive


## Semântica
* Evite o uso de div e span nos components dos elementos, tente usar elementos com definições adequadas.
* Crie nomes intuitivos para suas classes e IDs, isso facilita na hora de realizar manutenções.  
* Nomeie seus components e elementos com classes que possuem pelo menos duas palavras. Separe as palavras por hífen. 
  Exemplo:
  
```html
  <div class="card-info"></div>
```
 
## Acessibilidade
use atributos ARIA para tecnologias assistivas

## Javascript

### Reuso
lorem ipsum

### Convenções

### Modificadores de Estado

visível ou invisível;
ativo ou inativo;
habilitado ou desabilitado;
carregando ou carregado;
temProdutos ou semProdutos;
vazio ou cheio.

## SCSS
### Reuso
Sempre que puder crie um placeholder, map, mixin ou function para otimizar alguma ação
