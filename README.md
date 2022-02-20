# svelte-tabcontent

A fully-customizable tab UI component to go with your beautiful designs

![screenshot](https://github.com/zogs/zogs/blob/master/screenshot.png?raw=true)

## Demo

- [REPL](https://svelte.dev/repl/178e44cfd4104cb28d51070a703d5c29?version=3.46.4)

## Installation

```shell
npm install svelte-tabcontent --save
```

## Basic usage

```jsx
<script>
  import TabContent from 'svelte-tabcontent'
</script>

<TabContent
  backgroundColor="orange"
  borderColor="brown"
  borderWidth={2}
  >
    <h1>What an awesone component !</h1>
</TabContent>
```

## Props

| Prop               | Type                                | Default | Description                                                                                                                                                                                                                                                                                                 |
| :----------------- | :---------------------------------- | :------: | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `backgroundColor` | CSS color |    #BBB     | The background color. |
| `borderColor` | CSS color |   #BBB       | The border color. |
| `borderWidth` | number |    0     | Border width in pixel. |
| `roundTop` | number |     50     | Roundness aspect of the top curve (in %).  |
| `roundBottom` | number |    50      | Roundness aspect of the bottom curve (in %). |
| `curveWidth` | number |   50       | Total width of the roundness part (in px). |
| `depth` | number |    80     | Depth of the tab (in %). |
| `padding` | number |      0    | x padding arround the content. |
| `inversed` | boolean |    false      | Set to true to make it hand from above. |
| `align` | string |    'center'      | Align on the x axis ('left','center','right') |
| `shiftX` | number |   0       | Shift on the x axis in px). |
| `pattern` | string |      null    | SVG pattern element
| `patternId` | string |    'pattern'      | Name of the pattern element |

## License

[MIT](https://github.com/zogs/svelte-tabcontent/blob/master/LICENSE)
