title: TravelMap
date: 2016-10-08 21:15:00
tags:
- android
 
---

<font style="font-family:微软雅黑">
## Activity通知更新Fragment：
通过Fragment的onAttach()函数以及使用handler：  
在Fragment类中定义handler成员并且重载handleMessage()函数，在handlerMessage函数中进行根据Message的信息来进行相应的操作来更新Fragment，那么怎么将Activity怎么使用这个handler呢？因为他们并不是在同一个类中，所以我在Activity中也有一个handler，在OnAttach的时候将其赋值为handler，一开始有考虑将这个handler设置为fragment类的静态成员变量，但是考虑到这样会带来其他的问题（静态成员变量就与具体的对象无关而与类相关了）。
## Fragment通知更新Activity数据
由Activity来实现一个接口，在Fragment的onAttach函数中可以将这个接口传递给Fragment，再通过这个接口来调用我们想要更新的Activity的数据。
## Bitmap oom
其实这个问题我们一开始也没有想到。因为我负责的那一块在显示一些图片的时候一直会有问题，经常是整个黑色的什么都没有，但是队友做的那部分却一切正常。所以也是被这个问题坑了很久。
知道问题后解决其实就不麻烦了，通过使用BitmapFactory的一些选项设置可以达到获得缩略图片的效果，自然问题也就迎刃而解了。
## 同一个界面，判断是否是不同的使用类型
因为在同一个界面上，我们可能是想要新建一个项，也可能是想要编辑一个项，那怎么判断比较好呢？最后我们的解决方法是使用一个布尔值得成员变量来帮助判断，而在其他界面跳转过来时根据携带的信息来确定这个成员变量的布尔值。
## 拍照很坑的地方
当时是使用一个startActivityForResult的方法来启动相机并且传入了相应的Uri，因为需要保存文件，同时也需要将拍到的照片在界面上面显示出来，所以，坑的地方就出现了，在返回的data里面获得的图片是空的，本来还想着说能够很方便地把结果给显示出来，没想到还是得根据uri自己去把图片加载出来。
## ListView的优化
## 架构
每个Activity均实现同一个接口，实现接口函数。新建一个继承自Handler的类，重载handlerMessage函数，在函数中调用上述提到的接口函数，。然后我们再在Thread中调用使用handler就可以了。
## 使用ViewPager时滑动冲突
新建一个类继承自ViewPager，直接将其滑动事件禁用掉。