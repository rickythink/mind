# html classlist

## 原生JS增删查html classlist

```javascript
const htmlNode = document.getElementsByTagName('html')[0]
const classList = htmlNode.classList

classList.contains('is-clipped')
classList.add('is-clipped')
classList.remove('is-clipped')
```
