# 在Android Studio工程里面使用Protobuf的添加步骤

## 1.	添加proto文件
首先在工程下的src/main文件夹下建立一个proto文件夹，将编辑好的proto文件放入其中，这是我的proto文件示例：
```proto
syntax = "proto3";
package com.example.netrecv;

message Msg_Data
{
   int32 RowNum      = 1;       //字符串起始行  1-14
   int32 Column      = 2;       //字符串起始列  1-24
   bool bSmall       = 3;       //是否小字体
   bool bReverse     = 4;       //反向显示
   bool bUnderscore  = 5;       //下划线
   bool bFlashing    = 6;       //闪烁
   int Color         = 7;       //字体颜色
   string data       = 8;       //显示的字符串
}
```
## 2.	修改gradle文件
### 2.1 修改项目gradle文件
 在gradle下有两个build.gradle文件一个是项目的（显示project），一个是应用的（显示App），首先修改项目下的build.gradle，在dependencies中添加：
```java
classpath 'com.google.protobuf:protobuf-gradle-plugin:0.8.12'
 ```
### 2.2 修改程序gradle文件
修改app的build.gradle，添加如下代码
 文件头增加
```java
apply plugin: 'com.google.protobuf'
```
denpendencies中添加
```java
implementation 'com.google.protobuf:protobuf-java:3.11.4'
```
然后添加如下代码：
```java
sourceSets {
    main.java.srcDirs += "${protobuf.generatedFilesBaseDir}/main/java"
}
protobuf {
    protoc {
        // You still need protoc like in the non-Android case
        artifact = 'com.google.protobuf:protoc:3.11.4'
    }

    generateProtoTasks {
        all().each { task ->
            task.builtins {
                // In most cases you don't need the full Java output
                // if you use the lite output.
                remove java
            }
            task.builtins {
                java {}
            }
        }
    }

}
```
这样在系统编译的时候就会自动生成java包文件。
