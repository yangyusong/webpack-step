# webpack-step

# 环境配置
```
npm install webpack webpack-dev-server
```

下载案例并使用
```
git clone https://github.com/yangyusong/webpack-step.git
```

## 案例1(单文件编译和自动刷新)

```
cd webpack-step/basic
webpack index.js bundle.js
```

用浏览器打开文件，随意开一个服务进行访问即可。比如
```
python -m http.server
```
webpack本身就有服务，不过服务本身会用上我们的配置文件，这里我们只是测试命令行的使用。看到bundle.js的生成，其实就可以进入下一步了。


### 使用配置文件
webpack就该用自己的服务，要不然就太落后了。在工程目录下运行命令：
```
webpack-dev-server
```
命令行提示访问地址：
http://localhost:8080/webpack-dev-server/

如果我们要访问
http://localhost:8080/会怎样，我们发现区别不大，只是比http://localhost:8080/webpack-dev-server/少了app状态的显示。区别不仅于此。我们改一下输出的内容，在http://localhost:8080/webpack-dev-server/下是会自动刷新的，但是在http://localhost:8080/下的访问没有变化,这个其实是开发环境和生产环境的区别

其中配置文件中用entry来指定输入，用output来指定输出
```
    entry: './index.js',
	output: {
		filename: 'bundle.js'
	}
```
既然我们输出的js名称叫，bundle.js，那么html里面引用的也应该是此文件
```
<script src='bundle.js'></script>
```

如果我们想要生产环境也立刻刷新可以这样
```
webpack-dev-server --inline
```

然后修改js文件内容，我们发现生产环境也立刻刷新了.

## 多文件编译

我们在basic工程的基础上修改后就可以，修改后的工程叫multiple

我们打开index.js，加入一行
```
require('./req.js');
```

新加req.js文件
内容为：
```
console.log("common req");
document.write("common req");
```

如果服务还开着，服务器上我们就能看到req.js输出的内容了，太棒了，我们只是用了webpack，就支持了commonjs规范和之前上个例子所聊到的特性，那我们就和写后端一样的写前端了。后面我们再总结一下看webpack究竟给我们带来了什么。

不要高兴太早，我们打开bundle.js，发现里面根本没合并进req.js的内容，那是怎么回事呢，其实weppack把内容存在内存中了，我们只是访问到内存中的内容。而要确实打包进入我们的目标文件，我们应该通过第二种方式，通过配置文件来完成。配置文件中entry字段作为输入文件的配置，单个文件是一个字符串，那么多个文件就应该是个数组了吧。对的就是这样
```
module.exports = {
	entry: ['./index.js', './misc.js'],
	output: {
		filename: 'bundle.js'
	}
};
```

新加的文件misc.js内容
```
console.log("I'm a misc");
document.write(" I'm a misc");

```

完成后，我们重启服务即可

