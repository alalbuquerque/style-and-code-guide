# Atomic Design Pattern - Componentes

O Atomic Design foi criado por Brad Frost no qual sua proposta é a escabilidade do desenvolvimento de software. Desenvolvido usando analogias científicas e ferramentas de guia de estilo, seu uso é indicado para sistemas nos quais são necessários reusos de componentes.

Sua ideia inicial é segmentar os arquivos de estilos do projeto organizando de forma padronizada. 

![Fluxo atomico](http://www.marijazaric.com/wp-content/uploads/2016/08/atomic-web-design-1.gif)

Caso queira entrar mais afundo no assunto:

[Blog do Brad Frost - Atomic Design](http://bradfrost.com/blog/post/atomic-web-design/)

[Pattern Lab](http://patternlab.io/)

[Nomadev - Atomic Design Extended](http://nomadev.com.br/atomic-design-b%C3%B3sons-e-quarks-extended/)

[Nomadev - Atomic Design - Por que usar?](http://nomadev.com.br/atomic-design-por-que-usar/)

[SASS + Atomic design](https://medium.com/umdevux/sass-atomic-design-59d039f946ed)

[Smashing Magazine - The “Other” Interface: Atomic Design With Sass](https://www.smashingmagazine.com/2013/08/other-interface-atomic-design-sass/)

## Bósons 
Bósons são as menores unidades que irão declarar os atributos de estilo dos átomos. Esses bósons são usado como placeholders ou mixins. 

Exemplo:
main.scss

```scss
@import “boson/colors”;
@import “boson/typography”;
@import “boson/responsive”;
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
  
  
 // Estilizando um átomo
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

  // Mapa de font-sizes
 $font-sizes: (
   xxbig: 36px,
   xbig: 24px,
   big: 18px,
   medium: 16px,
   small: 14px,
   xsmall: 12px
 ) !default;

  // Função de leitura de mapa
 @function font-size($value, $rem: true) {
   @if map-has-key($font-sizes, $value) {
     @if $rem {
       @return map-get($font-sizes, $value);
     } @else {
       @return map-get($font-sizes, $value);
     }
   }
 }

 // Estilizando um átomo
 %buton_config {
   font-size: font-size(big);
 }
 button {
   @extend %buton_config;
 }
  

```
Compilado:

```scss
  button {
    font-size: 18px;
  }
```
### Responsive
Cores em bosons/responsive.scss

Exemplo:

```scss

  // Mapa de breakpoints 
$breakpoints: (
  small   : 641px,
  medium  : 769px,
  large   : 1025px,
  xlarge  : 1321px,
  xxlarge : 1921px
) !default;


 //mixin breakpoints mobile-first
 @mixin responsive($breakpoint, $width: min) {
   @if variable-exists(breakpoints) {
     @if map-has-key($breakpoints, $width) {
       @media (min-width: em(map-get($breakpoints, $breakpoint))) {
         @media (max-width: em(map-get($breakpoints, $width) - 1)) {
           @content;
         }
       }
     } @else if $width == max {
       @media (max-width: em(map-get($breakpoints, $breakpoint) - 1)) {
         @content;
       }
     } @else {
       @media (min-width: em(map-get($breakpoints, $breakpoint))) {
         @content;
       }
     }
   } @else {
     @warn 'O mapa $breakpoints não existe';
   }
 }

 button {
   @include responsive(medium, xlarge) {
     background: blue;
     width: 50px;
   }
 }

```
Compilado:

```css
  @media (min-width: em(769px)) and (max-width: em(1320px)) {
    button {
      background: blue;
      width: 50px;
    }
  }

```

## Quarks
Quarks são os elementos que irão ser modificados pelos bósons. E quando juntos criam um átomo, no exemplo a baixo ele separa o a tag `<button>` e o texto interno.
Exemplo:

```html
<!-- quark -->
<button></button>

<!-- quark -->
Texto
```

## Átomos
Átomo é a entidade gerada a partir da união de quarks e bósons.

```scss
 %buton_config {
   font-size: font-size(big);
   background-color: color(primary);
 }
 
 button {
   @extend %buton_config;
 }

```

```html
<!-- átomo -->
<button>Texto</button>

```

## Moléculas
Molécula seria o a união de dois ou mais átomos:
Exemplo: 

```html
<!-- átomo -->
<div class="card">
  <p class="message">lorem ipsum dolor sit amet consectetur</p>
  <button>Texto</button>
</div>

```

