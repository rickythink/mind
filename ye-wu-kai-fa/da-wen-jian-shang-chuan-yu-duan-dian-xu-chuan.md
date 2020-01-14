# 大文件上传与断点续传

## 文件上传

### 客户端

前端大文件上传核心是利用 `Blob.prototype.slice` 方法，和数组的 slice 方法相似，调用的 slice 方法可以返回原文件的某个切片。

这样我们就可以根据预先设置好的切片最大数量将文件切分为一个个切片，然后借助 http 的可并发性，同时上传多个切片，这样从原本传一个大文件，变成了同时传多个小的文件切片，可以大大减少上传时间。

### 服务端

服务端需要负责接受这些切片，并在接收到所有切片后`合并`切片

这里又引伸出两个问题

1. 何时合并切片，即切片什么时候传输完成
2. 如何合并切片

第一个问题需要前端进行配合，前端在每个切片中都携带切片最大数量的信息，当服务端接受到这个数量的切片时自动合并，也可以额外发一个请求主动通知服务端进行切片的合并

第二个问题，具体如何合并切片呢？这里可以使用 nodejs 的 api `fs.appendFileSync`，它可以同步地将数据追加到指定文件，也就是说，当服务端接受到所有切片后，先创建一个最终的文件，然后将所有切片逐步合并到这个文件中。

### 实现

```diff
+    // 生成文件切片
+    createFileChunk(file, length = LENGTH) {
+      const fileChunkList = [];
+      const chunkSize = Math.ceil(file.size / length);
+      let cur = 0;
+      while (cur < file.size) {
+        fileChunkList.push({ file: file.slice(cur, cur + chunkSize) });
+        cur += chunkSize;
+      }
+      return fileChunkList;
+    },
+   // 上传切片
+    async uploadChunks() {
+      const requestList = this.data
+        .map(({ chunk }) => {
+          const formData = new FormData();
+          formData.append("chunk", chunk);
+					 formData.append("hash", hash);
+          formData.append("filename", this.container.file.name);
+          return { formData };
+        })
+        .map(async ({ formData }) =>
+          this.request({
+            url: "http://localhost:3000",
+            data: formData
+          })
+        );
+      await Promise.all(requestList); // 并发切片
+    },
+    async handleUpload() {
+      if (!this.container.file) return;
+      const fileChunkList = this.createFileChunk(this.container.file);
+      this.data = fileChunkList.map(({ file }，index) => ({
+        chunk: file,
+        hash: this.container.file.name + "-" + index // 文件名 + 数组下标
+      }));
+      await this.uploadChunks();
+    }
```

## 断点续传

断点续传的原理在于前端/服务端需要`记住`已上传的切片，这样下次上传就可以跳过之前已上传的部分，有两种方案实现记忆的功能

* 前端使用 localStorage 记录已上传的切片 hash
* 服务端保存已上传的切片 hash，前端每次上传前向服务端获取已上传的切片

第一种是前端的解决方案，第二种是服务端，而前端方案有一个缺陷，如果换了个浏览器就失去了记忆的效果，所以这里选取后者  


### 文件秒传

### 文件秒传

在实现断点续传前先简单介绍一下文件秒传

所谓的文件秒传，即在服务端已经存在了上传的资源，所以当用户`再次上传`时会直接提示上传成功

文件秒传需要依赖上一步生成的 hash，即在`上传前`，先计算出文件 hash，并把 hash 发送给服务端进行验证，由于 hash 的唯一性，所以一旦服务端能找到 hash 相同的文件，则直接返回上传成功的信息即可  


### 暂停上传

讲完了生成 hash 和文件秒传，回到断点续传

断点续传顾名思义即断点 + 续传，所以我们第一步先实现“断点”，也就是暂停上传

原理是使用 XMLHttpRequest 的 `abort` 方法，可以取消一个 xhr 请求的发送，为此我们需要将上传每个切片的 xhr 对象保存起来，我们再改造一下 request 方法。

### 恢复上传

由于当文件切片上传后，服务端会建立一个文件夹存储所有上传的切片，所以每次前端上传前可以调用一个接口，服务端将已上传的切片的切片名返回，前端再跳过这些已经上传切片，这样就实现了“续传”的效果

而这个接口可以和之前秒传的验证接口合并，前端每次上传前发送一个验证的请求，返回两种结果

* 服务端已存在该文件，不需要再次上传
* 服务端不存在该文件或者已上传部分文件切片，通知前端进行上传，并把**已上传**的文件切片返回给前端



  


