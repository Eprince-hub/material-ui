---
title: Componente React Grade
components: Grid
githubLabel: 'component: Grid'
materialDesign: https://material.io/design/layout/understanding-layout.html
---

# Grade

<p class="description">O leiaute responsivo da grade do Material Design se adapta ao tamanho e orientação da tela, garantindo a consistência entre leiautes.</p>

Uma [grade](https://material.io/design/layout/responsive-layout-grid.html) cria consistência visual entre leiautes, enquanto permite flexibilidade em uma ampla variedade de projetos. A UI responsiva do Material Design é baseada em uma grade de 12 colunas.

{{"component": "modules/components/ComponentLinkHeader.js", "design": false}}

> O componente `Grid` não deve ser confundido com um data grid; ele está mais próximo de um layout grid. Para um cabeçalho do data grid para [o componente `DataGrid`](/components/data-grid/).

## Como funciona

O sistema de grade é implementado com o componente `Grid`:

- Ela usa [o módulo CSS de box flexível](https://www.w3.org/TR/css-flexbox-1/) para alta flexibilidade.
- Existem dois tipos de leiautes: *contêineres* e *itens*.
- Larguras de itens são definidas em porcentagens, desse modo são sempre fluidas e dimensionadas de acordo os seus elementos pai.
- Itens têm preenchimento para criar o espaçamento entre itens individuais.
- Existem cinco pontos de quebra (breakpoints) na grade: xs, sm, md, lg e xl.
- Valores inteiros podem ser dados para cada ponto de quebra,  indicando quantas das 12 colunas disponíveis são ocupadas pelo componente quando a largura da área de exibição satisfaz as [restrições de ponto de quebra](/customization/breakpoints/#default-breakpoints).

Se você é **novo ou não está familiarizado com o flexbox**, nós recomendamos você a ler este [guia do Flexbox CSS-Tricks](https://css-tricks.com/snippets/css/a-guide-to-flexbox/).

## Grades fluídas

As grades fluídas usam colunas que dimensionam e redimensionam o conteúdo. Uma disposição fluida de malha pode usar pontos de ruptura para determinar se precisa mudar drasticamente.

### Grade básica

As larguras das colunas é representada por  um numero inteiro que varia de  1 a 12. Podemos aplica-las a qualquer ponto de ruptura para indicar quantas colunas são ocupadas pelo componente.

Um valor aplicado a um ponto de ruptura se aplica a todos os outros pontos maiores, a menos que seja alterado, como será visto posteriormente nesta página. Por exemplo nesse trecho ` xs={12} ` definimos que o componente ocupará toda a largura da tela, independente do tamanho da tela.

{{"demo": "pages/components/grid/BasicGrid.js", "bg": true}}

### Grade com pontos de quebra

Podemos definir várias larguras para os componentes, resultando em um leiaute que muda de acordo com o ponto de ruptura. Width values given to larger breakpoints override those given to smaller breakpoints.

Aqui por exemplo `xs={12} sm={6}` define que o componente ocupará metade da largura da tela (6 colunas) em um dispositivo com resolução de 600px pra cima. Já para dispositivos menores, o componente vai tomar todas as 12 colunas.</p> 

{{"demo": "pages/components/grid/FullWidthGrid.js", "bg": true}}



## Espaçamento

Para controlar o espaço entre os filhos, use a prop ` spacing ` O valor do espaçamento pode ser qualquer numero positivo, incluindo decimais e até strings (cadeia de caracteres) As props são convertidas em css usando o auxiliar  [`theme.spacing()`](/customization/spacing/) 

{{"demo": "pages/components/grid/SpacingGrid.js", "bg": true}}



### Row & column spacing

The `rowSpacing` and `columnSpacing` props allow for specifying the row and column gaps independently. It's similar to the `row-gap` and `column-gap` properties of [CSS Grid](/system/grid/#row-gap-amp-column-gap).

{{"demo": "pages/components/grid/RowAndColumnSpacing.js", "bg": true}}



## Valores responsivos

You can switch the props' value based on the active breakpoint. For instance, we can implement the ["recommended"](https://material.io/design/layout/responsive-layout-grid.html) responsive layout grid of Material Design.

{{"demo": "pages/components/grid/ResponsiveGrid.js", "bg": true}}

Responsive values is supported by:

- `colunas`
- `espaçoDeColuna`
- `direção`
- `espaçoDeLinha`
- `spacing`
- todas as outras propriedades do sistema [other props](#system-props)



> ⚠️ Quando Usamos a prop de código responsivo `colunas `, cada item da manha precisa de seu correspondente ponto de ruptura. Por exemplo, este não está funcionando Os itens da malha precisa de seu valor para ` md` (ponto médio)
> 
> ```jsx
> <Grid container columns={{ xs: 4, md: 12 }}>
>    <Grid item xs={2} />
> > </Grid>
> ```



## Interativo

Abaixo está uma demonstração interativa que permite explorar os resultados visuais das diferentes configurações:

{{"demo": "pages/components/grid/InteractiveGrid.js", "hideToolbar": true, "bg": true}}



## Leiaute Automático

O leiaute automático faz com que o espaço disponível seja compartilhado de forma proporcional  _items_ Isso também quer dizer que podemos definir que a largura de um _item_ se ajustará automaticamente ao redor dele

{{"demo": "pages/components/grid/AutoGrid.js", "bg": true}}



### Variable width content

Set one of the size breakpoint props to `"auto"` instead of `true` / a `number` to size a column based on the natural width of its content.

{{"demo": "pages/components/grid/VariableWidthGrid.js", "bg": true}}



## Grade Complexa

The following demo doesn't follow the Material Design guidelines, but illustrates how the grid can be used to build complex layouts.

{{"demo": "pages/components/grid/ComplexGrid.js", "bg": true}}



## Grade Aninhada

The `container` and `item` props are two independent booleans; they can be combined to allow a Grid component to be both a flex container and child.



> Um **container** flex é a caixa gerada por um elemento com uma exibição definida por `flex` ou `inline-flex`. Os filhos em um fluxo de um container flex são chamados de flex **items** e são dispostos usando o modelo de leiaute flex (flex layout).

https://www.w3.org/TR/css-flexbox-1/#box-model

{{"demo": "pages/components/grid/NestedGrid.js", "bg": true}}

⚠️ Defining an explicit width to a Grid element that is flex container, flex item, and has spacing at the same time lead to unexpected behavior, avoid doing it:



```jsx
<Grid spacing={1} container item xs={12}>
```


If you need to do such, remove one of the props.



## Colunas

You can change the default number of columns (12) with the `columns` prop.

{{"demo": "pages/components/grid/ColumnsGrid.js", "bg": true}}



## Limitações



### Margem negativa

The spacing between items is implemented with a negative margin. This might lead to unexpected behaviors. For instance, to apply a background color, you need to apply `display: flex;` to the parent.



### white-space: nowrap;

A configuração inicial em itens flexíveis é `min-width: auto`. Isto causa um conflito de posicionamento quando os elementos filhos estão usando `white-space: nowrap`. Você pode enfrentar o problema com:



```jsx
<Grid item xs>
  <Typography noWrap>
```


Para que o item permaneça dentro do contêiner, você precisa definir `min-width: 0`. In practice, you can set the `zeroMinWidth` prop:



```jsx
<Grid item xs zeroMinWidth>
  <Typography noWrap>
```


{{"demo": "pages/components/grid/AutoGridNoWrap.js", "bg": true}}



### direction: column | column-reverse

The `xs`, `sm`, `md`, `lg`, and `xl` props are **not supported** within `direction="column"` and `direction="column-reverse"` containers.

They define the number of grids the component will use for a given breakpoint. They are intended to control **width** using `flex-basis` in `row` containers but they will impact height in `column` containers. If used, these props may have undesirable effects on the height of the `Grid` item elements.



## Leiaute de Grade CSS

The `Grid` component is using CSS flexbox internally. But as seen below, you can easily use [the system](/system/grid/) and CSS Grid to layout your pages.

{{"demo": "pages/components/grid/CSSGrid.js", "bg": true}}



## Propriedades do sistema

As a CSS utility component, the `Grid` supports all [`system`](/system/properties/) properties. Você pode usá-los como "props" diretamente no componente. For instance, a padding:



```jsx
<Grid item p={2}>
```
