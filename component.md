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
Os bósons são partículas que são usados para refletir modificações em outros elementos. 
A aplicação mais eficiente desses modificadores é a utilização de placeholders e mixins, que são extendidos nos arquivos de estilo dos atomos e quarks.
Os bósons pode ser separados de acordo com seus objetivo:

Exemplo:
```scss
@import “boson/colors.scss”; /* bósons de cores */
@import “boson/typography.scss”; /* bósons de tipografia */
@import “boson/responsive.scss”; /* bósons de media-queries */
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
    color: color(secundary);
    border-color: color(secundary);
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
    }
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
Quarks são os elementos que irão ser modificados pelos bósons.

Exemplo:

```scss
 //bóson de tipografia
 %buton_text {
   font-size: font-size(big);
 }
 //bóson de cor
 %buton_color {
   background-color: color(primary);
 }
 
 //quark de button
 button {
   @extend %buton_text;
   @extend %buton_color;
 }
```
## Átomos
Átomo é a entidade gerada a partir da união de quarks e bósons.

```scss
 //quark de button
 %buton_config {
   @extend %buton_text;
   @extend %buton_color;
 }
 
 //átomo de button
 button {
   @extend %buton_config;
 }

```

```html
<!-- átomo -->
<button>Enviar</button>

```

## Moléculas
Molécula seria o a união de dois ou mais átomos:
Exemplo: 

```scss
 //quark de button
 %button_config {
   @extend %button_typo;
   @extend %button_color;
 }
 
 //quark de input
 %input_config {
   @extend %input_typo;
   @extend %input_color;
 }
 
 //quark de card
 %card_config {
   @extend %card_color;
 }
 
 
//átomo de card
.form-search {
   @extend %card_config;
   
   //átomo de input
   > input {
     @extend %buton_config;
   }
 
   //átomo de button
   > button {
     @extend %button_config;
   }
 }
 
 //compilado
.form-search {
  background-color: #dadada;
}
.form-search > input {
  background-color: #EEEEEE;
  color: #000000;
  font-family: #334422;
  font-size: 16px; 
}
.form-search > button {
  background-color: #884202;
  color: #FFFFFF;
  font-family: #333333;
  font-size: 18px; 
}
```

```html
<!-- molécula -->

<div class="form-search"> <!-- quark -->
  <input type="text"><!-- quark -->
  <button>Texto</button> <!-- quark -->
</div>

```

## Organismos
Organismos seria o a união de dois ou mais moléculas:
Exemplo: 

```html
<!-- organismo -->
<header>
  <!-- molécula -->
  <a href="#"><img src="logo.svg" title="logo brand" role="logo"></a>

  <!-- molécula -->
  <ul>
    <li><a href="#">Link</a></li>
    <li><a href="#">Link</a></li>
    <li><a href="#">Link</a></li>
    <li><a href="#">Link</a></li>
  </ul>

  <!-- molécula -->
  <form class="header-search">
    <input type="text" placeholder="Search">
    <button type="submit">Submit</button>
  </form>

  <!-- molécula -->
  <ul>
    <li><a href="#">Link</a></li>
    <li class="dropdown">
      <a href="#" class="dropdown-toggle">Dropdown <span class="caret"></span></a>
      <ul class="dropdown-menu">
        <li><a href="#">Link</a></li>
        <li><a href="#">Link</a></li>
        <li><a href="#">Link</a></li>
        <li><a href="#">Link</a></li>
      </ul>
    </li>
  </ul>
</header>
```

