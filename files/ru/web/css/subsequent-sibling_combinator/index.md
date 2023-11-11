---
title: Селектор следующего элемента
slug: Web/CSS/Subsequent-sibling_combinator
original_slug: Web/CSS/General_sibling_combinator
---

{{CSSRef("Selectors")}}

## Описание

Общий комбинатор смежных селекторов `(~)` разделяет два селектора и находит второй элемент только если ему предшествует первый, и они оба имеют общего родителя. Свойства будут применены ко всем элементам, указанным в правой части, следующим за элементом, указанным в левой части.

## Синтаксис

```
element ~ element { style properties }
```

## Пример

```css
p ~ span {
  color: red;
}
```

```html
<span>Это не красный.</span>
<p>Здесь параграф.</p>
<code>Тут какой-то код.</code>
<span>А здесь span.</span>
```

{{ EmbedLiveSample('Example', 280, 120) }}

## Спецификации

{{Specifications}}

## Поддержка браузерами

{{Compat}}

## Смотрите также

- [Смежные селекторы](/ru/docs/Web/CSS/Adjacent_sibling_selectors)
