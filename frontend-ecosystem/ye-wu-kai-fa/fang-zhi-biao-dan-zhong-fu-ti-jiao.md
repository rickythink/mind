# 防止表单重复提交

## 前端JS提交后禁用，5秒后恢复

## 表单提交后重定向

这个也叫Post-Redirect-Get(PRG)模式。

当用户提交了表单后，你去执行一个客户端的重定向，转到提交成功信息页面。这能避免用户按F5导致的重复提交，而其也不会出现浏览器表单重复提交的警告，也能消除按浏览器前进和后退按导致的同样问题。

## session中存放一个特殊标志

**特殊标志**在服务器端，生成一个唯一的标识符，将它存入session，同时将它写入表单的隐藏字段中，然后将表单页面发给浏览器，用户录入信息后点击提交，在服务器端，获取表单中隐藏字段的值，与session中的唯一标识符比较，相等说明是首次提交，就处理本次请求，然后将session中的唯一标识符移除；不相等说明是重复提交，就不再处理。

##
