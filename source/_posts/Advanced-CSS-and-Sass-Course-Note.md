---
title: Advanced CSS and Sass Course Note
date: 2022-03-28 16:58:16
tags:
---

紀錄於 Udemy 上由 _Jonas Schmedtmann_ 推出的 _Advanced CSS and Sass: Flexbox, Grid, Animations and More!_ 課程筆記。

## Directory
+ [CSS Specificity](#css-specificity)
+ [CSS Tips](#css-tips)
    - [Make background image have a gradient color](#1-make-background-image-have-a-gradient-color)
    - [Make image have a cool shape](#2-make-image-have-a-cool-shape)
    - [Make box be placed at the center of the parent element](#3-make-box-be-placed-at-the-center-of-the-parent-element)
    - [Make a cool animation](#4-make-a-cool-animation)
+ [SASS](#sass)


## Concept
### CSS Specificity

In css, `!important` have the most highest weight, and the other selector weight order is:  

`inline-style` > `id` > `class` > `type-selector`

We can calculate the count from the high-order to the low-order selectors. Compare which selector have a higher weight. 

> Pseudo class have the same weight as class selector

ex.
```css
/* (inline, id, class, type) */

/* (0, 1, 2, 2) */
#nav div.pull-right a.button {
  background-color: orangered;
}

/* (0, 1, 2, 1) */
#nav a.button:hover {
  background-color: yellow;
}
```
In this example, when we hover the button, the `background-color` will still being yellow. Because the upper selector total weight is higher than the selector which have `hover`. 

### Set the root font-size to `%`
We can set the root font-size in `html` tag. But we should set the `%` font-size rather than specific value. If the user's device has increased the default root value, we set the root font-size with specific value. It will not make the web views font-size change.

```css
html {
  /* font-size: 10px */
  font-size: 62.5% 
}
```

## CSS Tips
### 1. Make background image have a gradient color

Give `background-image` multiple values, the first value will be on the upper layer, the second value will be on the lower layer. We can make a color on the upper layer then set its color to a little bit transparent, so we can see the lower-layer image.

```css
.header{
  background-image: linear-gradient(to right bottom, #7ed56fc0, #28b485be), url("../img/hero.jpg");
}
```

### 2. Make image have a cool shape

MDN: [clip-path](https://developer.mozilla.org/zh-CN/docs/Web/CSS/clip-path)

Use `clip-path` and `polygon` to set top、top-right、bottom-right、bottom-left point's location, then it will create a cool shape.

```css
.header {
  clip-path: polygon(0 0, 100% 0, 100% 75%, 0 100%);
}
```

### 3. Make box be placed at the center of the parent element 

Set `position: relative` in the parent element, then set position `absolute` to the box. Use `transform` to make the box at the center.

```css
.box {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

### 4. Make a cool animation
We can use `keyframes` to create an animation with a specific name. After creating an animation, we can add `animation-name` and `animation-duration` to an element. Also, we can add the `animation` property to combine the above two properties. (By default animation will start when the CSS is loaded)

> For the browser performance, it's best to only animate two different properties `opacity` and `transform`.

```css
.heading-primary-main {
  /* animation-name: moveInLeft; */
  /* animation-duration: 1s; */
  animation: 1s moveInLeft;
}

@keyframes moveInLeft {
  0% {
    opacity: 0;
    transform: translateX(-100px);
  }
  100% {
    opacity: 1;
    transform: translateX(0);
  }
}
```

#### animation-delay  
We can use `animation-delay` and `animation-fill-mode` to make an order of animations. 

**animation-fill-mode**:   
**[reference](https://ithelp.ithome.com.tw/articles/10200393)**  

animation-fill-mode is used to set the view before / after the animation begin / finish.


```css
.btn-animated {
  /* animation-delay: 0.75s; */
  animation: moveInBottom 0.5s ease-out 0.75s;
  animation-fill-mode: backwards;
}
```

### SASS
Sass is a CSS pre-processor, an extension of CSS that adds power and elegance to the basic language. It's like we pass a Sass code to a Sass compiler, then it will be compiled to the CSS code.

> SASS have **Sass syntax** which don't have curly brackets & **SCSS syntax** which have curly brackets.


#### install
`npm install node-sass --save-dev`

#### compile
add script in `package.json`

```json
"scripts": {
  "compile:sass": "node-sass sass/main.scss css/style.css --watch"
}
```

#### variable
```scss
// define color variables
$color-primary: #f9ed69;
$color-secondary: #f08a5d;

nav {
  margin: 10px;
  background-color: $color-primary;
}
```

#### nesting
```scss
.navigation {
  list-style: none;
  // use nesting 
  li {
    display: inline-block;
    // define a psuedo class
    &:first-child {
      margin: 0;
    }
  }
}
```

#### mixin
It's like `function` in css, use `@mixin` to define a function and `@include` to call this function.
```scss
@mixin style-link-text($color) {
  text-decoration: none;
  text-transform: uppercase;
  color: $color;
}

.btn {
  @include style-link-text(#fff);
}
```
