# socket.io-flex

socket.io 使用 `socket.io-flex` 组件让服务端能够提供柔性服务.

## How to use

```js
var app = require('http').createServer(handler);

var io = require('socket.io')(app);
var flex = require('socket.io-flex');

function handler (req, res) {
    res.setHeader("Access-Control-Allow-Origin", "*");
    res.writeHead(200);
    var data = 'no http service available!';
    res.end(data);
}

io.adapter(flex({
    threshold:[5,2000],
    thresholdTime:1,
    sysReTime:10,
    loadavg: [ 3,10 ],
    cpuusage: [ 10,30 ],
    memory: [ 0.99,0.3 ]
}));

io.on('connection', function (socket) {

	io.of('/').adapter.flexService(socket.id,function (err, obj,flag) {
		//TODO
	});

});

app.listen(3000);
```




## API


### adapter(opts)

The following options are allowed:

- `pingInterval`: 隔多少毫秒发送一个新的ping包 (`25000`)
- `threshold`: 柔性服务变化区间，第一个元素是websocket达到转短连数，第二个是websocekt达到转长连数 (`[5,2]`)
- `thresholdTime`: 每次变换等待小时数，不设置则达到就变 (`1`)
- `allowUpgrades`: 是否运行协议升级，不设置该项时才能设置柔性服务功能 (`true`)
- `sysReTime`: 读取系统信息间隔时间 (`1`)
- `loadavg`: 根据CPU 1分钟的负载，设置柔性服务功能 (`[0.9,0.5]`)
- `cpuusage`: 根据CPU 使用率，设置柔性服务功能 (`[70,1]`)
- `memory`: 根据内存使用率，设置柔性服务功能 (`[0.7,0.3]`)


### FlexAdapter

The flex adapter instances expose the following properties
that a regular `Adapter` does not


### FlexAdapter#flexService(id:String, fn:Function)


```js
io.of('/').adapter.flexService(id,function (err, obj, flag) {
  console.log(obj,flag);
});
```
The following obj are allowed:

- `pingInterval`: 隔多少毫秒发送一个新的ping包 (`25000`)
- `threshold`: 柔性服务变化区间，转websocekt的区间 (`[5,2]`)
- `thresholdTime`: 每次变换等待小时数，不设置则达到就变 (`1`)
- `clientsCount`: 目前服务的总连接数 (`11`)
- `clientsWebSocketCount`: 目前服务WebSocket的总连接数 (`10`)
- `clientsPollingCount`: 目前服务轮询的总连接数 (`1`)
- `changeTime`: 上一次协议改变的时间 (`1489649263605`)
- `isWs`: 是否是WebSocket连接 (`true`)
- `sysReTime`: 读取系统信息间隔时间 (`1`)
- `loadavg`: 根据CPU 1分钟的负载，设置柔性服务功能 转websocekt的区间 (`[0.9,0.5]`)
- `cpuusage`: 根据CPU 使用率，设置柔性服务功能 转websocekt的区间 (`[70,1]`)
- `memory`: 根据内存使用率，设置柔性服务功能 转websocekt的区间 (`[0.7,0.3]`)
- `currcpuusage`: 当前CPU使用率 (`0.3`)
- `currmemusage`: 当前内存使用率 (`0.2`)

The following flag are allowed:

- `flag`: 0表示没有转变，1表示转为短链，2表示转为长链 (`0`)

## License

MIT
