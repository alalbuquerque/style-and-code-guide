# Atomic Design Pattern - Componentes

introdução

## Bósons 
Bósons são as menores unidades que irão declarar os atributos de estilo dos átomos. Esses bósons são usado como placeholders ou mixins. 

Exemplo:
bosons/main.scss

```scss
@import “boson-colors”;
@import “boson-typography”;
@import “boson-responsive”;
```


### Cor 
Cores em bosons/colors.scss

Exemplo:

```scss
  $colors: (
    primary: #1eb7db,
    secundary: #85c440,
    tertiary: #a41034
  );

  @function color($color, $variant: null) {
    @if map-has-key($colors, $color) {
      $type: map-get($colors, $color);

      @if $variant == null {
        @if type-of($type) == map {
          @return map-get(map-get($colors, $color), 'base');
        } @else {
          @return map-get($colors, $color);
        }  
      } @else {
        @return map-get(map-get($colors, $color), $variant); 
      }
    } @else {
      @warn 'A chave "#{$color}" não existe. Verifique se ela foi definida no mpapa "$colors"!';
    }
  }  

  %buton_config {
    background-color: color(primary);
  }
  button {
    @extend %buton_config;
  }
  
```
Compilado:

```scss
  button {
    background-color: #FF0000;
  }
```

### Typography 
Cores em bosons/typografy.scss

Exemplo:

```scss
  %buton_config {
    font-size: 1.2em;
  }
  button {
    @extend %buton_config;
  }
```
Compilado:

```scss
  button {
    font-size: 1.2em;
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
