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

然后修改js文件内容，我们发现生产环境也立刻刷新了

