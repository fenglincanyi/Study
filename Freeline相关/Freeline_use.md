# Freeline 使用

###1. 配置

最友好的文档：
https://www.freelinebuild.com/docs/zh_cn/###

* 主 app 模块的 gradle:
```
apply plugin: 'com.antfortune.freeline'
    
android {
    ...
    
    
    freeline {
        // hack true //已经废弃
        // productFlavor 'online' // 如果有多个，则选择一个设置，否则gradle 会报错提示
        // applicationProxy false //application代理，默认开启  若app启动就闪退，class not found等错误可通过此设置改变
    
        // 具体配置参考：https://github.com/alibaba/freeline/wiki/Freeline-DSL-References
    }
}
```
* Project 的 gradle:
```
dependencies {
    ...
    
    classpath 'com.antfortune.freeline:gradle:0.8.4'
}
```
* Application 中，初始化 Freeline:
```
@Override
public void onCreate() {
    super.onCreate();

    FreelineCore.init(this);
}
```
### 2. 环境及工具搭建
```
* Python 2.7 (当前只支持此版本)
* Freeline plugin （studio 中搜索安装即可）
```
* 下载Freeline相关工具
在终端 Terminal 中输入： 
 * windows: ```gradlew initFreeline -Pmirror``` 
 * mac/linux: ``` ./gradlew initFreeline``` 
稍等片刻，就会在项目中多出Freeline相关的目录

### 3. 遇到的问题

* 确定FreelineCore.init(this);加入到Application类中，且在onCreate()下的第一行，不要根据是否在主进程做特殊处理，否则可能导致FreelineService无法正常启动；[Freeline 0.7.0+开始，默认开启了Application替换，这条可以不用检查]
* 确定FreelineService以及freeline相关组件是否正常merge到最终的minifest中，最终的manifest路径在${module}/build/intermediates/manifests中；
* 确定python freeline.py -v与定义在build.gradle中的freeline的版本是否一致；
* 确定是否刚刚执行了清空app数据的操作，freeline缓存数据在/data/data路径，清空app数据也会导致连接不上的问题（执行freeline命令时，通常会有句明显的日志反复出现：server result is -1）；
* 确定是否开启了网络代理导致127.0.0.1被重定向？
* 一定要先使用freeline来打全量包，再来进行增量，否则也会出现这个问题。即，freeline的全量编译与android-studio自带的RUN会存在冲突。

> #### 字符编码问题

![](http://7xr1vo.com1.z0.glb.clouddn.com/tr0jan_1478500206635_45.png)

**原因：**
因为python系统使用的默认编码为ascii编码，但是代码运行中的操作的字符不属于ascii范围

**解决：**
在 android_tools.py 中 添加：
```
import sys

reload(sys)
sys.setdefaultencoding('utf-8')
```

> #### 有的时候会出现一直是全量编译

**原因：**
app里的缓存在重新安装的时候没有更新

**解决：**
通常卸载后再安装可以解决
