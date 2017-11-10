## a animation javascript library

### use

```js

Animation()
  .easing(Animation.easing.Linear)
  .delay(500)
  .duration(1000)
  .run((i) => {
    document.querySelector('.box').style.left = 10 + 90 * i + 'px'
  })
  .then(() => {
    alert('finish')
  })

```