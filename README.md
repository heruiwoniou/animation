# 一个精简的js动画库

## 项目地址
[animation.js](https://github.com/heruiwoniou/animation)

由于项目须要做一些动画，并不想让整个项目引入太多的内容导致文件过大，所以为了满足要求写了一个精简的可扩展的动画库功能是简单模仿d3.js里面的tween函数

### 用法：
```js
Animation().easing(Animation.easing.Linear).duration(1000).run((i)=>{
  // 这里的i会返回0到1之间的数字
  // 再根据情况自己处理须要处理的动画
  // eg: 改变div left 属性 10 到 100
  document.querySelector('div').style.left = 10 + 90 * i + 'left'
})
.then(()=>{

})
```

### 代码过程：
首先，须要一个缓动函数，这个网上一搜一大把
这里帖上一个
```js

let easing = {
  Linea: function (t, b, c, d) {
    return c * t / d + b
  },
  Quad: {
    easeIn: function (t, b, c, d) {
      return c * (t /= d) * t + b
    },
    easeOut: function (t, b, c, d) {
      return -c * (t /= d) * (t - 2) + b
    },
    easeInOut: function (t, b, c, d) {
      if ((t /= d / 2) < 1) return c / 2 * t * t + b
      return -c / 2 * ((--t) * (t - 2) - 1) + b
    }
  },
  Cubic: {
    easeIn: function (t, b, c, d) {
      return c * (t /= d) * t * t + b
    },
    easeOut: function (t, b, c, d) {
      return c * ((t = t / d - 1) * t * t + 1) + b
    },
    easeInOut: function (t, b, c, d) {
      if ((t /= d / 2) < 1) return c / 2 * t * t * t + b
      return c / 2 * ((t -= 2) * t * t + 2) + b
    }
  },
  Quart: {
    easeIn: function (t, b, c, d) {
      return c * (t /= d) * t * t * t + b
    },
    easeOut: function (t, b, c, d) {
      return -c * ((t = t / d - 1) * t * t * t - 1) + b
    },
    easeInOut: function (t, b, c, d) {
      if ((t /= d / 2) < 1) return c / 2 * t * t * t * t + b
      return -c / 2 * ((t -= 2) * t * t * t - 2) + b
    }
  },
  Quint: {
    easeIn: function (t, b, c, d) {
      return c * (t /= d) * t * t * t * t + b
    },
    easeOut: function (t, b, c, d) {
      return c * ((t = t / d - 1) * t * t * t * t + 1) + b
    },
    easeInOut: function (t, b, c, d) {
      if ((t /= d / 2) < 1) return c / 2 * t * t * t * t * t + b
      return c / 2 * ((t -= 2) * t * t * t * t + 2) + b
    }
  },
  Sine: {
    easeIn: function (t, b, c, d) {
      return -c * Math.cos(t / d * (Math.PI / 2)) + c + b
    },
    easeOut: function (t, b, c, d) {
      return c * Math.sin(t / d * (Math.PI / 2)) + b
    },
    easeInOut: function (t, b, c, d) {
      return -c / 2 * (Math.cos(Math.PI * t / d) - 1) + b
    }
  },
  Expo: {
    easeIn: function (t, b, c, d) {
      return (t == 0) ? b : c * Math.pow(2, 10 * (t / d - 1)) + b
    },
    easeOut: function (t, b, c, d) {
      return (t == d) ? b + c : c * (-Math.pow(2, -10 * t / d) + 1) + b
    },
    easeInOut: function (t, b, c, d) {
      if (t == 0) return b
      if (t == d) return b + c
      if ((t /= d / 2) < 1) return c / 2 * Math.pow(2, 10 * (t - 1)) + b
      return c / 2 * (-Math.pow(2, -10 * --t) + 2) + b
    }
  },
  Circ: {
    easeIn: function (t, b, c, d) {
      return -c * (Math.sqrt(1 - (t /= d) * t) - 1) + b
    },
    easeOut: function (t, b, c, d) {
      return c * Math.sqrt(1 - (t = t / d - 1) * t) + b
    },
    easeInOut: function (t, b, c, d) {
      if ((t /= d / 2) < 1) return -c / 2 * (Math.sqrt(1 - t * t) - 1) + b
      return c / 2 * (Math.sqrt(1 - (t -= 2) * t) + 1) + b
    }
  },
  Elastic: {
    easeIn: function (t, b, c, d, a, p) {
      if (t == 0) return b
      if ((t /= d) == 1) return b + c
      if (!p) p = d * 0.3
      if (!a || a < Math.abs(c)) {
        a = c
        var s = p / 4
      } else var s = p / (2 * Math.PI) * Math.asin(c / a)
      return -(a * Math.pow(2, 10 * (t -= 1)) * Math.sin((t * d - s) * (2 * Math.PI) / p)) + b
    },
    easeOut: function (t, b, c, d, a, p) {
      if (t == 0) return b
      if ((t /= d) == 1) return b + c
      if (!p) p = d * 0.3
      if (!a || a < Math.abs(c)) {
        a = c
        var s = p / 4
      } else var s = p / (2 * Math.PI) * Math.asin(c / a)
      return (a * Math.pow(2, -10 * t) * Math.sin((t * d - s) * (2 * Math.PI) / p) + c + b)
    },
    easeInOut: function (t, b, c, d, a, p) {
      if (t == 0) return b
      if ((t /= d / 2) == 2) return b + c
      if (!p) p = d * (0.3 * 1.5)
      if (!a || a < Math.abs(c)) {
        a = c
        var s = p / 4
      } else var s = p / (2 * Math.PI) * Math.asin(c / a)
      if (t < 1) return -0.5 * (a * Math.pow(2, 10 * (t -= 1)) * Math.sin((t * d - s) * (2 * Math.PI) / p)) + b
      return a * Math.pow(2, -10 * (t -= 1)) * Math.sin((t * d - s) * (2 * Math.PI) / p) * 0.5 + c + b
    }
  },
  Back: {
    easeIn: function (t, b, c, d, s) {
      if (s == undefined) s = 1.70158
      return c * (t /= d) * t * ((s + 1) * t - s) + b
    },
    easeOut: function (t, b, c, d, s) {
      if (s == undefined) s = 1.70158
      return c * ((t = t / d - 1) * t * ((s + 1) * t + s) + 1) + b
    },
    easeInOut: function (t, b, c, d, s) {
      if (s == undefined) s = 1.70158
      if ((t /= d / 2) < 1) return c / 2 * (t * t * (((s *= (1.525)) + 1) * t - s)) + b
      return c / 2 * ((t -= 2) * t * (((s *= (1.525)) + 1) * t + s) + 2) + b
    }
  },
  Bounce: {
    easeIn: function (t, b, c, d) {
      return c - Tween.Bounce.easeOut(d - t, 0, c, d) + b
    },
    easeOut: function (t, b, c, d) {
      if ((t /= d) < (1 / 2.75)) {
        return c * (7.5625 * t * t) + b
      } else if (t < (2 / 2.75)) {
        return c * (7.5625 * (t -= (1.5 / 2.75)) * t + 0.75) + b
      } else if (t < (2.5 / 2.75)) {
        return c * (7.5625 * (t -= (2.25 / 2.75)) * t + 0.9375) + b
      } else {
        return c * (7.5625 * (t -= (2.625 / 2.75)) * t + 0.984375) + b
      }
    },
    easeInOut: function (t, b, c, d) {
      if (t < d / 2) return Tween.Bounce.easeIn(t * 2, 0, c, d) * 0.5 + b
      else return Tween.Bounce.easeOut(t * 2 - d, 0, c, d) * 0.5 + c * 0.5 + b
    }
  }
}

```

其次，我们须要用到requestAnimationFrame,要先解决一下兼容性问题
这个也是网上一找一大把,这里贴上我用的代码

``` js

var vendors = ['webkit', 'moz']
for (var x = 0; x < vendors.length && !window.requestAnimationFrame; ++x) {
  window.requestAnimationFrame = window[vendors[x] + 'RequestAnimationFrame']
  window.cancelAnimationFrame = window[vendors[x] + 'CancelAnimationFrame'] ||
    window[vendors[x] + 'CancelRequestAnimationFrame']
}

if (!window.requestAnimationFrame) {
  window.requestAnimationFrame = function (callback, element) {
    var currTime = new Date().getTime()
    var timeToCall = Math.max(0, 16.7 - (currTime - lastTime))
    var id = window.setTimeout(function () {
      callback(currTime + timeToCall)
    }, timeToCall)
    lastTime = currTime + timeToCall
    return id
  }
}
if (!window.cancelAnimationFrame) {
  window.cancelAnimationFrame = function (id) {
    clearTimeout(id)
  }
}

```

然后，我们须要定义一下时间戳的兼容函数，后面会用到

``` js

function time () {
  if (typeof performance !== 'undefined' && performance.now) {
    return performance.now()
  }
  return Date.now ? Date.now() : (new Date()).getTime()
}

```

最后，我们要开始制作整个动画的主要内容了

核心是利用缓动函数进行目标值生成

缓动参数的说明:
t: 已经流逝的时间
b: 初始值
c: 目标值
d: 总时间

可以看出来缓动函数的大概功能是
根据 【t所占d的比例】
算出 【b-c之间的值】

我们只须要开始一个轮询，传入执行函数的时间和值直到结束就行了
下面看代码

``` js

class AnimationCore {
  constructor () {
    this.__duration__ // 执行动画的时间
    this.__delay__ = 0 // 开始动画的间隔
    this.__action__ // 传入的处理函数
    this.__easing__ = easing.Linear // 缓动函数
    this.__startTime__ // 开始动画的时间戳
    this.resolve = null
    this.timer = null
  }

  /**
   * 设置 执行动画的时间
   * 
   */
  duration (d) {
    this.__duration__ = d
    return this
  }

  /**
   * 设置 开始动画的间隔
   * 
   */
  delay (d) {
    this.__delay__ = d
    return this
  }

  /**
   * 设置 缓动函数
   * 
   */
  easing (e) {
    this.__easing__ = e
    return this
  }

  /**
   * 停止
   * 
   */
  stop () {
    if (this.timer) {
      cancelAnimationFrame(this.timer)
      this.timer = null
    }
  }

  /**
   * 结束并到最后状态
   * 
   */
  finish () {
    if (this.timer) {
      cancelAnimationFrame(this.timer)
      this.timer = null
      this.action(1)
      this.resolve()
    }
  }
  /**
   * 开始 动画
   * 
   */
  run (action, easingfunction) {
    return new Promise((resolve, reject) => {
      // 根据设置的delay来进行延时
      setTimeout(() => {
        // 获取开始时间
        this.__startTime__ = time()
        easingfunction = easingfunction || this.__easing__ || easing.Linear
        this.action = action
        this.resolve = resolve
        // 这里先执行一次 因为下次执行时可能跳过了第一帖
        this.action(0)
        // 下面是每贴要执行的内容
        let step = () => {
          // 获取流逝了的时间
          var passedTime = Math.min(time() - this.__startTime__, this.__duration__)
          // 获取0到1之间的值是通过缓动函数
          // 参数说明:
          //  t: 已经流逝的时间
          //  b: 初始值
          //  c: 目标值
          //  d: 总时间
          this.action(
            easingfunction(
              passedTime,
              0,
              1,
              this.__duration__
            )
          )
          if (passedTime >= this.__duration__) {
            this.start = null
            this.timer = null
            this.resolve()
          } else this.timer = requestAnimationFrame(step)
        }
        this.timer = requestAnimationFrame(step)
      }, this.__delay__)
    })
  }
}

```
导出内容

```js

function Animation () {
  return new AnimationCore()
}
Animation.easing = easing

// 导出
export default Animation


```