# 实现 merge

```javascript
/**
 * Simple is object check.
 * @param item
 * @returns {boolean}
 */
function isObject(item) {
    return (item && typeof item === 'object' && !Array.isArray(item) && item !== null);
}


/**
 * Deep merge two objects.
 * @param target
 * @param source
 */
export function mergeDeep(target, source) {
    if (isObject(target) && isObject(source)) {
        Object.keys(source).forEach(key => {
            if (isObject(source[key])) {
                if (!target[key] || !isObject(target[key])) {
                    target[key] = source[key];
                }
                mergeDeep(target[key], source[key]);
            } else {
                Object.assign(target, { [key]: source[key] });
            }
        });
    }
    return target;
}

const A={
    var_a: {
        'loc': 'Earth',
        'title': 'Hello World',
        'type': 'Planet', 
        'deeper':{
            'map':new Map([['a', 'AAA'],['b', 'BBB']]),
            'mapId':15473, 
        }
   }
};

const B={
    var_a: {  
        'type': 'Star',
        'deeper':{ 
            'mapId':9999, 
            'alt_map':new Map([['x', 'XXXX'],['y', 'YYYY']]),
        }
   }
}

function assert(condition, message) {
    if (!condition) {
        message = message || "Assertion failed";
        if (typeof Error !== "undefined") {
            throw new Error(message);
        }
        throw message; // Fallback
    }
}

const C = mergeDeep(A,B);
const D = mergeDeep({a: 1}, { b : { c: { d: { e: 12345}}}}); 
const E = mergeDeep({b: {c: 'hallo'}}, { b : { c: { d: { e: 12345}}}}); 
const F = mergeDeep({ b : { c: { d: { e: 12345}}, d:'dag', 'f':'one'}}, {b: {c: 'hallo', e:'ok',f:'two'}}); 

assert (C.var_a.type === "Star");
assert (C.var_a.deeper.alt_map.get('x') === 'XXXX');
assert (C.var_a.deeper.map.get('b') === 'BBB');
assert (D.a === 1);
assert (D.b.c.d.e === 12345);
assert (E.b.c.d.e === 12345);
assert (F.b.c === 'hallo');
assert (F.b.d === 'dag');
assert (F.b.e === 'ok')
assert (F.b.f === 'two')
```

