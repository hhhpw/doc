```scss
@import './_vars';

$themes: (
  light: (
    'text-50': $light-text-50,
    'text-80': $light-text-80,
    'text-100': $light-text-100,
    'bg-08': $light-bg-08,
    'bg-100': $light-bg-100,
    'bg-200': $light-bg-200,
    'bg-300': $light-bg-300,
    'bg-400': $light-bg-400,
    'bg-500': $light-bg-500,
  ),
  dark: (
    'text-50': $dark-text-50,
    'text-80': $dark-text-80,
    'text-100': $dark-text-100,
    'bg-08': $dark-bg-08,
    'bg-100': $dark-bg-100,
    'bg-200': $dark-bg-200,
    'bg-300': $dark-bg-300,
    'bg-400': $dark-bg-400,
    'bg-500': $dark-bg-500,
  ),
);

// define
$theme-map: null;
@mixin theme() {
  @each $theme, $map in $themes {
    // $theme: darkTheme, lightTheme
    // $map: ('text-color': ..., 'bg-color': ...)
    // make the $map globally accessible, so that theme-get() can access it
    $theme-map: $map !global;

    // make a class for each theme using interpolation -> #{}
    // use & for making the theme class ancestor of the class
    // from which you use @include theme() {...}
    // 用的是next-theme，在html上会生成这个属性
    // 使用的时候看在html上生成的属性
    :global([html-theme='#{$theme}']) & {
      @content; // the content inside @include theme() {...}
    }
  }

  // no use of the variable $theme-map now
  $theme-map: null !global;
}
@function theme-get($key) {
  @return map-get($theme-map, $key);
}

```
### 使用

```scss

@include theme {
  background-color: theme-get('text-50');
}
```

@mixin、@function去配合。