# HTTP协议

在HTTP中，PUT被定义为idempotent的方法，POST则不是，这是一个很重要的区别。如果一个方法重复执行多次，产生的效果是一样的，那就是idempotent的。

 

举一个简单的例子，假如有一个博客系统提供一个Web API，模式是这样http://superblogging/blogs/post/{blog-name}，很简单，将{blog-name}替换为我们的blog名字，往这个URI发送一个HTTP PUT或者POST请求，HTTP的body部分就是博文，这是一个很简单的REST API例子。我们应该用PUT方法还是POST方法？取决于这个REST服务的行为是否是idempotent的，假如我们发送两个http://superblogging/blogs/post/Sample请求，服务器端是什么样的行为？如果产生了两个博客帖子，那就说明这个服务不是idempotent的，因为多次使用产生了副作用了嘛；如果后一个请求把第一个请求覆盖掉了，那这个服务就是idempotent的。前一种情况，应该使用POST方法，后一种情况，应该使用PUT方法。

