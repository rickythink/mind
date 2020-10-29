# 时间戳与时区

### 什么是时间戳

`时间戳` 指的就是Unix时间戳\(Unix timestamp\)。它也被称为Unix时间\(Unix time\)、POSIX时间\(POSIX time\)，是一种时间表示方式，定义为从格林威治时间1970年01月01日00时00分00秒起至现在的总秒数

`时间戳` 在地球的每一个角落都是相同的, 可以尝试修改不同时区查看下面代码结果

```javascript
console.log(Date.now());
```

结果都是一样的

### 什么是时区

`时区` 是指相同的时间在不同的地区有不同的含义, 我们所处的时区为 `东八区`, 可以在设备的时间设置中手动调整时区

同一时间戳在不同时区所表示的日期是不同的, 比如下面的代码

```text
const time = new Date(1556074515196);
console.log(time.getFullYear());
console.log(time.getMonth());
console.log(time.getDate());
```

在北京时间执行的结果为 `2019 3 24`, 而在美国时间执行的结果却是 `2019 3 23`

### 产生的问题

有这样一个场景, 接口返回了某演出的开演时间string和相应的时间戳, string是按服务端按东八区计算的结果, 然后端上会用时间戳展示一个日历组件, 结果导致用户选择了15号的日期, 实际演出却是16号的, 造成客诉

### 如何解决问题

定义了如下方法用于转换时区, 使其得到的结果与服务端一致

```javascript
/**
* 把指定时区的时间换算到当地时区
* @param {number} timestamp 指定的时间戳
* @param {number} timeZone 指定时间戳的时区，
* @returns {number} 系统时区的时间
*/
function transformToLocalTime(timestamp, timeZone = -8) {
    const date = new Date();
    const zone = date.getTimezoneOffset() / 60;
    const zoneGap = zone - timeZone;
    timestamp += (zoneGap * 60 * 60 * 1000);
    return timestamp;
}
```

