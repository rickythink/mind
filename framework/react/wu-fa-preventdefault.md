# 无法preventDefault

Chrome 51默认设定注入`scroll` 事件的`passive` 变量为`true` 。而React的设计原则是跟随chrome的设定。因此在React设计的**合成事件\(**SyntheticEvent**\)**,  无法实现 `e.preventDefault()` 

目前的解决方案是，自行绑定dom元素，添加`addEventListener` 

```javascript
import React, { useRef, useEffect } from 'react'

const BlockPageScroll = ({ children }) => {
  const scrollRef = useRef(null)
  useEffect(() => {
    const scrollEl = scrollRef.current
    scrollEl && scrollEl.addEventListener('wheel', stopScroll)
    return () => {
      scrollEl && scrollEl.removeEventListener('wheel', stopScroll)
    }
  }, [])
  const stopScroll = e => e.preventDefault()
  return (
    <div ref={scrollRef}>
      {children}
    </div>
  )
}

const Main = () => (
  <BlockPageScroll>
    <div>Scrolling here will only be targeted to inner elements</div>
  </BlockPageScroll>
)
```

