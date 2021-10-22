# 实现缩放

实现缩放的核心代码如下

```typescript
const [node, setNode] = useState<HTMLElement | null>(null)
const [matrix, setMatrix] = useState([1, 0, 0, 1, 0, 0])

function handleWheelScale(event: WheelEvent) {
  if ((event.ctrlKey || event.altKey) && node) {
    event.preventDefault()
    // ref: https://github.com/aleofreddi/svgpan/blob/d59255197236e5936650e4cd9b1ec0b88f199188/svgpan.js#L146
    const { offsetLeft, offsetTop } = node
    const { clientX, clientY } = event
    // 1.2 and 360 control the sensitivity
    const zoom = Math.pow(1.2, -event.deltaY / 360)
    const ctm = new DOMMatrix(matrix)
    let mouse = new DOMPoint(clientX - offsetLeft, clientY - offsetTop)
    mouse = mouse.matrixTransform(ctm.inverse())
    const trans = new DOMMatrix()
      .translate(mouse.x, mouse.y)
      .scale(zoom)
      .translate(-mouse.x, -mouse.y)
    setMatrix(dmatrix2Array(ctm.multiply(trans)))
  }
}

function dmatrix2Array(dmatrix: DOMMatrix) {
  return [dmatrix.a, dmatrix.b, dmatrix.c, dmatrix.d, dmatrix.e, dmatrix.f]
}
```

