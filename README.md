# game-shake
JS 模拟微信摇一摇
#摇一摇案例教程
2015年的春节，微信借着春晚通过摇一摇抢红包，再一次的走近大家的眼中，创出一个全名抢红包的事情，不知道大家是否都抢到红包了没，别急，让我们自己写一个页面来再一次的过一把瘾吧。
所谓抢红包，其实就是微信的摇一摇，不得不说，这个摇一摇的动作在其他的应用里面真的是很少被使用，那么我们如何通过js来实现呢？
在html5中，有个属性可以来获取设备的运动传感器的时间、状态、加速度等数据，常见的数据就是来自于设备的陀螺仪、加速计、罗盘。
其中，deviceorientation事件，提供了设备的物理方向信息；devicemotion事件，提供了设备的加速度。
很明显，通过这两个事件，我们能做摇一摇、重力感应、指南针等应用。
###下面让我们开始制作摇一摇的旅程吧。

####第一步，首先来判断一下，当前这个设备是否支持运动传感事件。
```
if(window.DeviceMotionEvent){//判断设备是否支持运动传感事件。
        window.addEventListener('devicemotion', shake, false);//如果支持，那么就绑定shake方法到事件上
}else{
        alert('本设备不支持摇一摇功能’);//如果不支持，提示即可
}
```
####第二步，编写shake方法。
首先让我们分析一下，用户什么样子的操作才能算作使用摇一摇了呢？
1.用户摇动手机，是以一个方向为主的摇动。
2.用户摇动手机的时候，加速度肯定是有变化的。
3.设置一个阈值，判断用户单位时间内的运动状态，以避免将用户的正常行为当作触发摇一摇。

```
//首先定义一下，全局变量
var lastTime = 0;//此变量用来记录上次摇动的时间
var x = y = z = lastX = lastY = lastZ = 0;//此组变量分别记录对应x、y、z三轴的数值和上次的数值
var shakeSpeed = 800;//设置阈值
//编写摇一摇方法
function shake(){
        var acceleration = eventDate.accelerationIncludingGravity;//获取设备加速度信息
        var nowTime = new Date().getTime();//记录当前时间
        //如果这次摇的时间距离上次摇的时间有一定间隔 才执行
        if(nowTime - lastTime > 100){
                var diffTime = nowTime - lastTime;//记录时间段
                lastTime = nowTime;//记录本次摇动时间，为下次计算摇动时间做准备
                x = acceleration.x;//获取x轴数值，x轴为垂直于北轴，向东为正
                y = acceleration.y;//获取y轴数值，y轴向正北为正
                z = acceleration.z;//获取z轴数值，z轴垂直于地面，向上为正
                //计算 公式的意思是 单位时间内运动的路程，即为我们想要的速度
                var speed = Math.abs(x + y + z - lastX - lastY - lastZ) / diffTime * 10000;
                if(speed > shakeSpeed){//如果计算出来的速度超过了阈值，那么就算作用户成功摇一摇
                        //这里就是放置如果用户成功的摇一摇，将要触发的事件，例如提示摇到了谁，摇到了多少金币等等
                }
                lastX = x;//赋值，为下一次计算做准备
                lastY = y;//赋值，为下一次计算做准备
                lastZ = z;//赋值，为下一次计算做准备
        }
}
```

好了，核心的东西我们已经写完了，剩下的其实就是一些页面上的动画效果，当摇一摇被触发才执行的事件，这就要和你写的项目相关联了，这里就不一一写了。

附上本次实例地址：http://shake.coolfishstudio.com
