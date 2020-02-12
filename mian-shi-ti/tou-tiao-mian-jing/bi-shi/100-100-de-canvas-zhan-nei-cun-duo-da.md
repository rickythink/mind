# 100 \* 100 的 Canvas 占内存多大

imageData = ctx.getImageData\(sx, sy, sw, sh\)这个 API回的是一个 ImageData 数组，这个数组是 Uint8 类型的，且四位表示一个像素

因此占用的内存是 100 _100_ 4 bytes = 40,000 bytes。

结果不一定准确，后面再补充。

