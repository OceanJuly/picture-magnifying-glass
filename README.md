# Vue 3 + TypeScript + Vite
### 效果
()[]

### 实现
#### 根据小图上的鼠标位置去定位大图
> 通过`mouse`事件监听

如果是通过`mouse`事件监听，需要几个`API`去获取鼠标在`图片div`的位置：
- `getBoundingClientRect`获取屏幕上的位置
- `mouse`事件的`clientX`和`clientY`获取相对位置
- 相减得到`div`的左上角坐标
- 然后通过`mousemove`事件获取偏移量计算具体位置

> 通过`@vueuse/core`库获取偏移量

`VueUse`是一个为`Vue 3`提供常用功能和自定义钩子的开源库。
通过`useMouseInElement`来直接获取鼠标漂移量，还有鼠标是否在元素上面：
```ts
import { useMouseInElement } from '@vueuse/core'

const { isOutside, elementX, elementY } = useMouseInElement(target)
```

#### 如何让鼠标一直在小图 mask 的中间
#### 判断上下左右边界条件
这两个问题都是判断边界条件：左上、右下
> 左上

判断偏移量是否为负值，是置为`0`，并且当偏移量小于`mask`的一半时，也置为`0`，就能让鼠标一直在小图 `mask` 的中间，且`mask`不会从左上角溢出
```ts
// 计算初始的遮罩左上角的坐标
let maskX = x - getMaskWidth.value / 2
let maskY = y - getMaskHeight.value / 2

// 判断是否超出界限,并纠正
maskY = maskY < 0 ? 0 : maskY
maskX = maskX < 0 ? 0 : maskX
```

> 右下

判断偏移量加上`mask`的宽高是否比设定的`div`宽高大，是置为宽高的值：
```ts
if (maskY + getMaskHeight.value >= height) {
    maskY = height - getMaskHeight.value
}
if (maskX + getMaskWidth.value >= width) {
    maskX = width - getMaskWidth.value
}
```

#### 兼容处理，比如图片空白
在效果图的第二个那里能看当图片空白时，放大图也能把空白处显示出来，而第一个和第三个则没有这功能

主要是第一和第三的实现是通过`background-size`来设置放大效果，然后通过动态配置`background-position`来实现的

而要使放大镜更加逼真，需要给放大效果的`img`标签外层再加一层`div`，然后通过`overflow: hidden;`来隐藏溢出的`img`，通过绝对定位来移动图片

### 后面有空再补上通过HTML2Canvas实现放大效果
`coding...`