# animation

> a turtor to learn javascript animation



#### javascript animation api





#### javascript animiation library



**1.three.js**

> Github [link](https://github.com/mrdoob/three.js)

> Usage:

```html
<script src="js/three.min.js"></script>
```

```js
var camera = new THREE.PerspectiveCamera( 70, window.innerWidth / window.innerHeight, 0.01, 10 );
```



**2.Anime.js**

> Github [link](https://github.com/juliangarnier/anime)

>  Usage:

```shell
npm i animejs
```

```html
<script src="anime.min.js"></script>
```

```js
anime({
  targets: 'div',
  translateX: [
    { value: 100, duration: 1200 },
    { value: 0, duration: 800 }
  ],
  rotate: '1turn',
  backgroundColor: '#FFF',
  duration: 2000,
  loop: true
});
```



**3.particles.js**

> Github [link](https://github.com/VincentGarreau/particles.js)

> Usage:

```html
<div id="particles-js"></div>

<script src="particles.js"></script>
```

```js
particlesJS.load('particles-js', 'assets/particles.json', function() {
  console.log('callback - particles.js config loaded');
});
```

particles.json

```json
{
    "particles": {
    "number": {
      "value": 80,
      "density": {
        "enable": true,
        "value_area": 800
      }
    },
    "color": {
      "value": "#ffffff"
    },
    "shape": {
      "type": "circle",
      "stroke": {
        "width": 0,
        "color": "#000000"
      },
      "polygon": {
        "nb_sides": 5
      },
      "image": {
        "src": "img/github.svg",
        "width": 100,
        "height": 100
      }
    }
}
```

