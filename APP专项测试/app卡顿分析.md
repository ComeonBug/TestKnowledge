cpu、内存、网络都会影响卡顿

工具：systrace（sdk自带的工具）

/Users/liyang/Library/Android/sdk/platform-tools/systrace

1.细化到每一帧

2.线程状态

3.堆栈、函数调用都可以展示出来

cpu（计算耗时）、内存（内存抖动、full GC）、网络、render（布局复杂、overdraw）都会影响卡顿

内存的影响一般比较小

cpu和render影响比较大

render：