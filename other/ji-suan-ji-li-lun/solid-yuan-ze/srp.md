# SRP | The Single Responsibility Principle

**单一功能原则**（Single responsibility principle）规定每个类都应该有一个单一的功能，并且该功能应该由这个类完全封装起来。所有它的（这个类的）服务都应该严密的和该功能平行（功能平行，意味着没有依赖）。

此原则意味着**一个类应该只做且只能做一件事情**。

举例来说，假如我们需要从网络上获取一些 JSON 数据，然后解析它，并把结果保存在本地数据库中。根据我们正在编码的平台，这种工作可以使用为数不多的代码来实现。由于代码量不多，我们可能会想把所有的逻辑全部扔到一个类中。但是，根据单一职责原则，这将会是一个糟糕的做法。我们可以清楚地区分 3 个不同的职责：从网络上获取 JSON 数据，解析数据，保存解析的结果到数据库中。基于此，我们应该有 3 个类。

第 1 个类应该只处理网络。我们给它提供一个 URL，然后接收 JSON 数据或者在出现问题时，收到一个错误信息。

第 2 个类应该只解析它接收到的 JSON 数据并以相应的格式返回结果。

第 3 个类应该以相应的格式接收 JSON 数据，并把它保存在本地数据库中。
