# Service

---

##0x00 科普

---

一个Service是没有界面且能长时间运行于后台的应用组件．其它应用的组件可以启动一个服务运行于后台，即使用户切换到另一个应用也会继续运行．另外，一个组件可以绑定到一个service来进行交互，即使这个交互是进程间通讯也没问题．例如，一个service可能处理网络事物，播放音乐，执行文件I/O，或与一个内容提供者交互，所有这些都在后台进行．

##0x01  知识要点

---






- startService()

	startService()方法会立即返回然后Android系统调用service的onStartCommand()方法．但是如果service尚没有运行，系统会先调用onCreate()，然后调用onStartCommand().

- protected abstract void onHandleIntent (Intent intent)

	调用工作线程处理请求

- public boolean onUnbind (Intent intent)

	当所有client均从service发布的接口断开的时候被调用。默认实现不执行任何操作，并返回false。



	这是所有service的基类．当你派生这个类时，在service中创建一个新的线程来做所有的工作是十分重要的．因为这个service会默认使用你的应用的主线程(UI线程)，这将拉低你的应用中所有运行的activity的性能

2. IntentService

	这是一个Service的子类，使用一个工作线程来处理所有的启动请求，一次处理一个．这是你不需你的service同时处理多个请求时的最好选择．你所有要做的就是实现onHandleIntent()，这个方法接收每次启动请求发来的intent，于是你可以做后台的工作．


**表现形式**

1. Started

	一个service在某个应用组件（比如一个activity)调用startService()时就处于"started"状态（注意，可能已经启动了）．一旦运行后，service可以在后台无限期地运行，即使启动它的组件销毁了．通常地，一个startedservice执行一个单一的操作并且不会返回给调用者结果．例如，它可能通过网络下载或上传一个文件．当操作完成后，service自己就停止了

2. Bound

	一个service在某个应用组件调用bindService()时就处于"bound"状态．一个boundservice提供一个client-server接口以使组件可以与service交互，发送请求，获取结果，甚至通过进程间通讯进行交叉进行这些交互．一个boundservice仅在有其它应用的组件绑定它时运行．多个应用组件可以同时绑定到一个service，但是当所有的自由竞争组件不再绑定时，service就销毁了．































**案例2:services劫持**

攻击原理:隐式启动services,当存在同名services,先安装应用的services优先级高

攻击模型

![](hijack.png)




	Intent i = getIntent();         					if(i.getAction().equals("serializable_action"))
		{  
			i.getSerializableExtra("serializable_key");//未做异常判断
		}
		
Parcelable:

	this.b =(RouterConfig)  this.getIntent().getParcelableExtra("filed_router_config");//引发转型异常崩溃

POC内传入畸形数据即可引发crash,修复很简单捕获异常即可.






