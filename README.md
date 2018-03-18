# NDKJpegCompress
使用jpeg压缩图片（仿微信），达到图片大小缩小，像素一样，不失真的效果。

大家都知图片压缩一般都是用bitmap的compress来压缩，但是这样的压缩不管做的再好都会失真或者更改像素。
因为google开发者在封装skia（jpeg的二次封装）的时候把jpeg的优越的性能给屏蔽掉了。
所以想要是图片压缩达到最好的效果，我们直接使用jpeg来压缩。

jpeg在android源码当中有，可以直接下载，我们使用到的就是jpeg的头文件。
1、在开发之前，NDK的环境肯定是需要搭建好的。
2、拷贝jpeg的头文件到jni目录下。
3、调用lib库和创建native方法。
4、创建c/cpp文件，实现native方法，native方法就是我们的处理压缩方法，jpeg是使用jpeg_compress_struct进行压缩到的。具体实现看源码。
5、jni目录下编写Android.mk和Application.mk文件，和build.gradle的ndk编译shell脚本。

当然配置参数这些就是基本功了。

Android NDK从r16 beta1开始，不再支持 ARM5 (armeabi)。默认情况下，Android NDK不会构建ARM5版本。如果在Application.mk配置了构建ARM5版本，会收到类似以下的警告：

      Error:(81) Android NDK: Application targets deprecated ABI(s): armeabi
      Error:(82) Android NDK: Support for these ABIs will be removed in a future NDK release. 
如果不需要支持ARM5，可以在APP_ABI列表里吧armeabi去掉。

Application.mk配置APP_ABI

Application.mk的参数APP_ABI用来配置Android系统支持的CPU架构。

支持所有架构

APP_ABI := all
支持所有32位架构

APP_ABI := all32
支持指定的架构

APP_ABI := armeabi，armeabi-v7a，x86，mips，arm64-v8a，mips64，x86_64
多个架构使用逗号隔开。
在根目录gradle.properties文件加入: android.useDeprecatedNdk=true
ok可以编译运行了。
