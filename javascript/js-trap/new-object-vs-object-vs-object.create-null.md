# new Object vs Object vs Object.create(null)

new Object() 和 Object 是一样的，都会创建一个对象实例

但是Object.create(null) 会创建一个空对象的实例

区别在于Object.create(null) 的实例是没有\_\_proto\_\_，而前两者都有
