# performance 分析

performance.getEntriesByType\("navigation"\);

![&#x8BB0;&#x4E0B;&#x8FD9;&#x79CD;&#x56FE;&#xFF0C;&#x5BF9;&#x7406;&#x89E3;&#x524D;&#x7AEF;&#x6027;&#x80FD;&#x5E2E;&#x52A9;&#x5F88;&#x5927;](../../.gitbook/assets/image%20%28140%29.png)

不同阶段之间是连续的吗? —— 不连续 

每个阶段都一定会发生吗？—— 不一定

## 测量资源加载时间

```javascript
const resourceListEntries = performance.getEntries('resource')
// 某类资源的加载时间，可测量图片、js、css、XHR
resourceListEntries.forEach(resource => {
    if (resource.initiatorType == 'img') {
    console.info(`Time taken to load ${resource.name}: `, resource.responseEnd - resource.startTime);
    }
});
```

## JS总加载时间

```javascript
const p = window.performance.getEntries();
let cssR = p.filter(ele => ele.initiatorType === "script");
Math.max(...cssR.map((ele) => ele.responseEnd)) - Math.min(...cssR.map((ele) => ele.startTime));
```

## CSS 加载耗时

```css
const p = window.performance.getEntries();
let cssR = p.filter(ele => ele.initiatorType === "css");
Math.max(...cssR.map((ele) => ele.responseEnd)) - Math.min(...cssR.map((ele) => ele.startTime));
```

## 总结

let p = window.performance.getEntries\(\);  
重定向次数：performance.navigation.redirectCount  
JS 资源数量：p.filter\(ele =&gt; ele.initiatorType === "script"\).length  
CSS 资源数量：p.filter\(ele =&gt; ele.initiatorType === "css"\).length  
AJAX 请求数量：p.filter\(ele =&gt; ele.initiatorType === "xmlhttprequest"\).length  
IMG 资源数量：p.filter\(ele =&gt; ele.initiatorType === "img"\).length  
总资源数量: window.performance.getEntriesByType\("resource"\).length

**不重复的耗时时段区分：**  
重定向耗时: redirectEnd - redirectStart  
DNS 解析耗时: domainLookupEnd - domainLookupStart  
TCP 连接耗时: connectEnd - connectStart  
SSL 安全连接耗时: connectEnd - secureConnectionStart  
网络请求耗时 \(TTFB\): responseStart - requestStart  
HTML 下载耗时：responseEnd - responseStart  
DOM 解析耗时: domInteractive - responseEnd  
资源加载耗时: loadEventStart - domContentLoadedEventEnd

**其他组合分析：**  
白屏时间: domLoading - fetchStart  
粗略首屏时间: loadEventEnd - fetchStart 或者 domInteractive - fetchStart  
DOM Ready 时间: domContentLoadEventEnd - fetchStart  
页面完全加载时间: loadEventStart - fetchStart



