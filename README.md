##nodeJS是什么
  Node.js是一个JS runtime， 运行在谷歌引擎V8上
  Node.js用了事件驱动，非阻塞I/O模型
1. 非阻塞I/O
  阻塞:I/O时进程休眠等待I/O完成后进行下一步
  非阻塞:I/O时函数立即返回，进程不等待I/O完成
2. 事件驱动
  I/O等异步操作结束后的通知
  观察者模式
##nodeJS的优点
  前端职责范围变大，统一开发体验
  在处理高并发、I/O密集场景性能优势明显
1. CPU密集 VS I/O密集
  I/O密集：文件操作、网络操作，数据库
  CPU密集：压缩、解压、加密、解密
2. WEB常见场景
  静态资源读取
  数据库操作
  渲染页面
3. 高并发应对之道
  增加机器数
  增加每台机器的CPU数-多核
4. 进程
  进程：计算机中的程序关于某数据集合上的一次运行活动，是系统进行资源分配和调度的基本单位
5. NodeJS工作模型
  Client发生HTTP请求到Node.JS服务器
  单线程 
6. 线程
  线程：进程内一个相对独立的，可调度的执行单元，与同属一个进程的线程共享进程的资源
  多线程：
7. NodeJS的单线程
  单线程只是针对主进程，I/O操作系统低层多线程调度
  单线程并不是单进程
8. 常用场景
  Web Server
  本地代码构建
  实用工具开发
##环境
  1. CommonJS
    每个文件是一个模块，有自己的作用域
    在模块内部module变量代表模块本身
    module.exports属性代表模块对外接口
    1. require规则
      /表示绝对路径，./表示相对于当前文件的
      支持js、json、node扩展名，不写依次尝试
      不写路径则认为是build-in模块或者各级node_modules内的第三方模块
    2. require特性
      1. module被加载的时候执行，加载后缓存
      2. 一旦出现某个模块被循环加载，就只输出已经执行的部分，还未执行的部分不会输出
    3. exports和module.exports的关系
      const exports = module.exports
  2. global
    CommonJS
    Buffer,process,console
    timer
  3. process 
    ```js
    cosnt {argv, argv0, execArgv, execPath} = process```
##调试
  inspect
  步骤
  1. chrome://inspect/#devices
  2. 安装谷歌插件 NIM
##基础API
#PATH
  1. baseline
  2. join
  3. resolve
  4. basename
  5. dirname
  6. extname
  7. parse
  8. format
  9. sep
  10. delimiter
  11. win32
  12. posix
  ```js
  console.log('__dirname:', __dirname)
  console.log('process.cwd():', process.cwd())
  console.log('./:', path.resolve('./'))
  ```
  __dirname,__filename 总是返回文件的绝对路径
  process.cwd()总是返回执行node命令所在文件夹
  ./
    在require方法中总是相对于当前文件夹所在文件夹
    在其他地方和process.cwd()一样，相对node启动文件夹
#Buffer
  Buffer用于处理二进制数据流
  实例类似整数数组，大小固定
  C++代码在V8堆外分配物理内存
  ```js
  console.log(Buffer.alloc(10));
  console.log(Buffer.alloc(5,1));
  console.log(Buffer.allocUnsafe(5, 1));
  console.log(Buffer.from([1,2,3]));
  console.log(Buffer.from('test', 'base64'))
  ```
  ```js
  console.log(Buffer.byteLength('test'));
  console.log(Buffer.byteLength('测试'));

  console.log(Buffer.isBuffer({}));
  console.log(Buffer.isBuffer(Buffer.from([1,2,3])));

  const buf1 = Buffer.from('This ')
  const buf2 = Buffer.from('is ')
  const buf3 = Buffer.from('a ')
  const buf4 = Buffer.from('test ')
  const buf5 = Buffer.from('! ')
  const buf = Buffer.concat([buf1, buf2, buf3, buf4, buf5])
  console.log(buf.toString())
  ```
#event
  readFile/readFileSync/stat/readdir/mkdir/rmdir/watch
  /rename/unlink
  ```js
  const EventEmmiter = require('events')

  class MyEventEmmiter extends EventEmmiter{ }

  const myEmmiter = new MyEventEmmiter()

  myEmmiter.on('Susan', () => {
    console.log('触发了一个事件！')
  })

  myEmmiter.emit('Susan')
  ```
  ```js
  const EventEmmiter = require('events')

  class CustomEvent extends EventEmmiter {}

  const cus = new CustomEvent()

  cus.on('error', (err, time) => {
    console.log(err)
    console.log(time)
  })
  cus.emit('error', new Error('opps!'), Date.now())
  ```
  ```js
  const EventEmmiter = require('events')

  class CustomEvent extends EventEmmiter {}

  const ce = new CustomEvent()

  function fn1() {
    console.log('fn1');
  }

  function fn2() {
    console.log('fn2');
  }

  ce.on('test', fn1)
  ce.on('test', fn2)

  setInterval(()=>{
    ce.emit('test')
  },500)

  setInterval(()=>{
    ce.removeListener('test', fn2)
  }, 1500)
  ```

#fs
