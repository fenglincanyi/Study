# Freeline 使用

###1. 配置
* 主 app 模块的 gradle:
```
apply plugin: 'com.antfortune.freeline'
    
android {
    ...
    
    // 此项可选
    freeline {
        hack true
    }
}
```
* Project 的 gradle:
```
dependencies {
    ...
    
    classpath 'com.antfortune.freeline:gradle:0.8.2'
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
