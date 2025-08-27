# Ripple-Light-Hover-Vue-Lib

## Example

Import styles

```js
import 'kn-circle-light/kn-circle-light.css'
```

Usage

```html
<circle-light-wrapper #default="{ cls, wrapper }" v-bind="wrapperProps" wrapper-extra-class="rounded">
      <div class="row">
        <div class="col" v-for="i in 40" :key="i">
          <component :is="wrapper">
            <div :class="cls" class="card rounded" v-text="`Card ${i}`">
              
            </div>
          </component>
        </div>
      </div>
    </circle-light-wrapper>
```

## Props

| Prop               | Type       | Default        |
| ------------------ | ---------- | -------------- |
| color              | `string?`  | rgb(0,111,255) |
| disabled           | `boolean?` |                |
| zIndex             | `number?`  | 0              |
| wrapperExtraClass  | `string?`  |                |
| size               | `number?`  | 2              |
| borderCircleSize   | `number?`  | 300            |
| innerCircleSize    | `number?`  | 300            |
| innerCircleOpacity | `number?`  | 0.05           |

