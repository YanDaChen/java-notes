| jar包  | 功能  |  说明 |
|:-:|:-:|:-:|
|jcl-over-slf4j.jar  |  jcl    -> slf4j       |将Jakarta(Apache) Commons Logging日志框架到 slf4j 的桥接|
|jul-to-slf4j.jar    |  juc    -> slf4j       |将java.util.logging的日志桥接到 slf4j|
|log4j-over-slf4j.jar|  log4j  -> slf4j       |将log4j 的日志，桥接到slf4j|
|osgi-over-slf4j.jar |  osgi   -> slf4j       |将osgi环境下的日志，桥接到slf4j|
|slf4j-android.jar   |  android-> slf4j       |将android环境下的日志，桥接到slf4j|
|slf4j-api.jar       |                        |slf4j 的api接口jar包|
|slf4j-ext.jar       |                        |扩展功能|
|slf4j-jcl.jar       |  lf4j -> jcl           |slf4j 转接到 Jakarta Commons Logging日志输出框架|
|slf4j-jdk14.jar     |  slf4j -> jul          |slf4j 转接到 java.util.logging，所以这个包不能和jul-to-slf4j.jar同时用，否则会死循环|
|slf4j-log4j12.jar   |  slf4j -> log4j        |slf4j 转接到 log4j,所以这个包不能和log4j-over-slf4j.jar同时用，否则会死循环|
|slf4j-migrator.jar  |                        |一个GUI工具，支持将项目代码中 JCL,log4j,java.util.logging的日志API转换为slf4j的写法|
|slf4j-nop.jar       |  slf4j -> null         |slf4j的空接口输出绑定，丢弃所有日志输出|
|slf4j-simple.jar    |  slf4j -> slf4j-simple |slf4j的自带的简单日志输出接口|
|log4j-1.2-api.jar   |  log4j -> log4j2       |将log4j 的日志转接到log4j2日志框架|
|log4j-api.jar       |                        |log4j2的api接口jar包|
|log4j-core.jar      |                        |log4j2的日志输出核心jar包|
|log4j-slf4j-impl.jar|  slf4j  -> log4j2      |slf4j 转接到 log4j2 的日志输出框架  不能和 log4j-to-slf4j同时用)|
|log4j-to-slf4j.jar  |  log4j2 -> slf4j       |将 log4j2的日志桥接到 slf4j  (不能和 log4j-slf4j-impl 同时用)|
|logback-core.jar    |                        |logback核心包|
|logback-classic.jar |  slf4j  -> logback     |桥接包|



