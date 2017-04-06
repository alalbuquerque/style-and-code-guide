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
  // Mapa de cores com suas variáções fixas 
  
  $colors: (
    primary:   #1EB7DB,
    secondary: #85C440,
    danger:    #F75757,
    success:   #A4CB1B,
    warning:   #FEBC11,
    light: (
      lightest: #FFFFFF,
      light:    #F9F9F9,
      base:     #EEEBEB,
      medium:   #E0E0E0
    ),
    dark: (
      light:   #C9C9C7,
      base:    #706f6f,
      medium:  #B1AFAF,
      darkest: #000000
    )
  ) !default;
  
  // Função de leitura de mapa
  
  @function color($color, $complementary: null) {
    @if map-has-key($colors, $color) {
      $type: map-get($colors, $color);

      @if $complementary == null {
        @if type-of($type) == map {
          @return map-get(map-get($colors, $color), 'base');
        } @else {
          @return map-get($colors, $color);
        }  
      } @else {
        @return map-get(map-get($colors, $color), $complementary); 
      }
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
    background-color: #1EB7DB;
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
