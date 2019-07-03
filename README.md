gifplayer
================

It's a vue component that will play and stop animated gifs.

### Basic Usage

1. Add a preview of the gif file to your website


```html
<template>
    <gif-player ref="example1" :src="media/banana.png"></gif-player>
</template>
<script src='https://unpkg.com/vue@2.2.6/dist/vue.js'></script>
<script src="../dist/vue-gif-player.min.js"></script>
  <script type="text/javascript">
    Vue.use(VueGifPlayer)
    var Main = {
      name: 'test',
      data() {
        return {
          url: 'media/banana.png'
        }
      },
      methods: {

      }
    }
    var Actor = Vue.extend(Main)
    new Actor().$mount('#app')
</script>
```