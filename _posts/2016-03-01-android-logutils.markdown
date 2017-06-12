---
layout:     post
title:      "LogUtils"
subtitle:   "史上最强Log框架，比Logger更强大"
date:       2016-03-01 12:00:00
author:     "SnailStudio"
header-img: "img/post-bg-2015.jpg"
tags:
    - Android
---

> “Yeah It's on. ”


## 前言

史上最强Log框架，比Logger更强大。

---

## 项目源码

[https://github.com/xuqiqiang/LogUtils](https://github.com/xuqiqiang/LogUtils)

---

## LogUtils能提供的功能

LogUtils库有以下优势：

* 更加轻巧，资源的占用更少
* 支持保存日志到文件，几乎不影响性能
* 支持漂亮的日志样式，也可以自定义
* 支持记录崩溃日志
* 支持打印任意对象、集合、多维数组
* 支持格式打印json、xml等
* 显示源码位置等

---

## LogUtils的使用

LogUtils的使用非常简单。

#### 配置

LogUtils不需要配置就能直接使用。如果你需要启用更多的功能，可以参考如下配置：

```java
        LogUtils.initialize(LogConfig.newBuilder()
                .level(Log.VERBOSE) // Specify the level of the logs
                .tag("TAG") // Specify the tag of the logs
                .debug(BuildConfig.DEBUG) // Output to LogCat
                .stress(true) // Highlight all the logs
                .codeInfo(true) // Show the code information
                .enableWrite(this, dirPath) // Output to files
                .reportCrash(true) // Report crash logs
                .build());
```

#### 打印字符串

```java
LogUtils.d("Hello");

LogUtils.w("The best Log framework\non the earth");
```

![打印效果](/img/logutils/1.png)

#### 打印格式化字符串

```java
LogUtils.d("hello %s", LogUtils.class.getSimpleName());

LogUtils.i("version:%.1f", 1.0f);
```

![打印效果](/img/logutils/3.png)


#### 打印格式化json、xml

```java
LogUtils.json(TAG, "{\"firstName\": \"Brett\", \"lastName\": \"McLaughlin\"}");

LogUtils.xml(TAG,
        "<?xml version=\"1.0\" encoding=\"UTF-8\"?><note><to>Tove</to><from>Jani</from><heading>Reminder</heading><body>Don't forget me this weekend!</body></note>");
```

![打印效果](/img/logutils/5.png)

#### 打印多维数组

```java
{%raw%}
LogUtils.object(new String[]{"abc", "123"});

LogUtils.object(new int[]{123, 456});

LogUtils.object(new String[][]{
        {"abc", "123"},
        {"def", "456"}
});

LogUtils.object(new int[][]{
        {123, 456},
        {789, 123}
});

LogUtils.object(new String[][][]{
        {{"abc", "123"}, {"def", "456"}},
        {{"qwe", "123"}, {"zxc", "456"}}
});
{%endraw%}
```

![打印效果](/img/logutils/7.png)

#### 打印对象数组

```java
{%raw%}
Shoes[] shoes = new Shoes[]{
        new Shoes("A", "red"),
        new Shoes("B", "blue"),
};
LogUtils.object(shoes);
{%endraw%}
```

![打印效果](/img/logutils/9.png)

#### 打印集合

```java
{%raw%}
List<String> list = new ArrayList<>();
list.add("list1");
list.add("list2");
LogUtils.object(list);

Map map = new HashMap();
map.put("key1", "value1");
map.put("key2", "value2");
LogUtils.object(map);
{%endraw%}
```

![打印效果](/img/logutils/b.png)

#### 打印对象集合

```java
Shoes[] shoes = new Shoes[]{
        new Shoes("A", "red"),
        new Shoes("B", "blue"),
};

List<Shoes> shoesList = new ArrayList<>();
Collections.addAll(shoesList, shoes);
LogUtils.object(shoesList);

Map shoesMap = new HashMap();
shoesMap.put("shoes1", shoes[0]);
shoesMap.put("shoes2", shoes[1]);
LogUtils.object(shoesMap);

Map shoesMap1 = new HashMap();
shoesMap1.put(shoes[0], shoes[1]);
LogUtils.object(shoesMap1);
```

![打印效果](/img/logutils/d.png)

#### 打印对象

```java
{%raw%}
    class Person {
        String name;
        int age;
        String sex;
        Clothes clothes;

        Shoes[] shoes;
        String[] array1;
        String[][] array2;
        String[][][] array3;
        String[][][][] array4;
        List<String> list;
        Map map;

        public Person(String name, String sex) {
            this.name = name;
            this.age = 25;
            this.sex = sex;
            this.clothes = new Clothes("T-shirt", "white");
            this.shoes = new Shoes[]{
                    new Shoes("A", "red"),
                    new Shoes("B", "blue"),
            };

            array1 = new String[]{"abc", "123"};
            array2 = new String[][]{
                    {"abc", "123"},
                    {"def", "456"}
            };
            array3 = new String[][][]{
                    {{"abc", "123"}, {"def", "456"}},
                    {{"qwe", "123"}, {"zxc", "456"}}
            };
            list = new ArrayList<String>();
            list.add("list1");
            list.add("list2");

            map = new HashMap<String, String>();
            map.put("key1", "value1");
            map.put("key2", "value2");
        }


        class Clothes {
            String name;
            String color;
            String[] array1;

            public Clothes(String name, String color) {
                this.name = name;
                this.color = color;

                array1 = new String[]{"abc", "123"};
            }
        }
    }

    class Shoes {
        String name;
        String color;

        public Shoes(String name, String color) {
            this.name = name;
            this.color = color;
        }
    }
{%endraw%}
```

```java
LogUtils.object(new Person("Harry", "male"));
```

![打印效果](/img/logutils/f.png)

#### 打印Throwable

```java
try {
    "test".charAt(100);
} catch (Exception e) {
    LogUtils.e(e, "Test Exception");
}
```

![打印效果](/img/logutils/g.png)

#### 打印当前应用CPU占用率

```java
LogUtils.cpuRate();
```

![打印效果](/img/logutils/h.png)

#### 自定义log样式

```java
        LogUtils.initialize(LogConfig.newBuilder()
                .debug(BuildConfig.DEBUG) // Output to LogCat
                .logAdapter(new LogAdapter() {
                    @Override
                    public boolean isLoggable(int priority, String tag) {
                        return TextUtils.equals(tag, "TAG1");
                    }

                    @Override
                    public void log(int priority, String tag, String message) {
                        Log.println(priority, tag, "--------- " + message + " ---------");
                    }
                })
                .build());
```

```java
LogUtils.dLog("TAG1", "message of TAG1");

LogUtils.dLog("TAG2", "message of TAG2");
```

![打印效果](/img/logutils/j.png)

#### 输出日志到文件

需要添加在AndroidManifest中添加如下配置：

```xml
        <service
            android:name="com.dftc.logutils.writer.LogUtilsService"
            android:exported="true"
            android:process="com.dftc.service.logrecorder">
            <intent-filter>
                <action android:name="com.dftc.service.logrecorder.LogUtilsService" />
            </intent-filter>
        </service>
```

```java
        String dirPath = Environment.getExternalStorageDirectory().getAbsolutePath() + File.separator
                + "log1";

        LogUtils.initialize(LogConfig.newBuilder()
                .enableWrite(context, dirPath) // Output to files
                .build());
```

由于写入文件的操作比较消耗系统资源，所以采用了一个第三方进程的服务，对打印的数据进行缓存，每隔5秒写入到文件，并且在系统资源占用过高的时候跳过写入操作。

另外对文件数量作了限制，防止存储空间写满。

写入文件中的信息：

```xml
2017-06-12 18:02:34.685 D/TAG(MainActivity.java:92) : hello LogUtils
2017-06-12 18:02:34.810 I/TAG(MainActivity.java:94) : version:1.0
2017-06-12 18:02:34.877 W/TAG(MainActivity.java:96) : The best Log framework
on the earth
2017-06-12 18:02:34.899 D/MainActivity(MainActivity.java:98) : {
    "firstName": "Brett",
    "lastName": "McLaughlin"
}
2017-06-12 18:02:35.653 D/MainActivity(MainActivity.java:100) : <?xml version="1.0" encoding="UTF-8"?>
<note>
    <to>Tove</to>
    <from>Jani</from>
    <heading>Reminder</heading>
    <body>Don't forget me this weekend!</body>
</note>

2017-06-12 18:02:35.661 E/TAG(MainActivity.java:106) : Test Exception : java.lang.StringIndexOutOfBoundsException: length=4; index=100
	at com.dftc.logdemo1.MainActivity$3.run(MainActivity.java:104)
	at java.lang.Thread.run(Thread.java:841)

2017-06-12 18:02:35.673 D/TAG(MainActivity.java:109) : String[2] : [abc, 123]
2017-06-12 18:02:35.680 D/TAG(MainActivity.java:111) : int[2] : [123, 456]
2017-06-12 18:02:35.687 D/TAG(MainActivity.java:113) : String[2][2] : [
    [abc, 123]
    [def, 456]
]
2017-06-12 18:02:35.700 D/TAG(MainActivity.java:118) : int[2][2] : [
    [123, 456]
    [789, 123]
]
2017-06-12 18:02:35.710 D/TAG(MainActivity.java:123) : [[[abc, 123], [def, 456]], [[qwe, 123], [zxc, 456]]]
2017-06-12 18:02:35.907 D/TAG(MainActivity.java:131) : ArrayList {size = 2} [
    [0] : list1,
    [1] : list2
]
2017-06-12 18:02:35.924 D/TAG(MainActivity.java:136) : HashMap {size = 2} [
    [key2] -> value2,
    [key1] -> value1
]
2017-06-12 18:02:35.955 D/TAG(MainActivity.java:142) : Shoes[2] : [
    Shoes {
        color = red,
        name = A
    },
    Shoes {
        color = blue,
        name = B
    }
]
2017-06-12 18:02:35.965 D/TAG(MainActivity.java:146) : ArrayList {size = 2} [
    [0] : Shoes {
        color = red,
        name = A
    },
    [1] : Shoes {
        color = blue,
        name = B
    }
]
2017-06-12 18:02:35.975 D/TAG(MainActivity.java:151) : HashMap {size = 2} [
    [shoes2] -> Shoes {
        color = blue,
        name = B
    },
    [shoes1] -> Shoes {
        color = red,
        name = A
    }
]
2017-06-12 18:02:35.985 D/TAG(MainActivity.java:155) : HashMap {size = 1} [
    [Shoes {
        color = red,
        name = A
    }] -> Shoes {
        color = blue,
        name = B
    }
]
2017-06-12 18:02:36.007 D/TAG(MainActivity.java:157) : Person {
    array1 = String[2] : [abc, 123],
    array2 = String[2][2] : [
        [abc, 123]
        [def, 456]
    ],
    array3 = [[[abc, 123], [def, 456]], [[qwe, 123], [zxc, 456]]],
    array4 = null,
    clothes = Clothes {
        array1 = String[2] : [abc, 123],
        color = white,
        name = T-shirt
    },
    list = ArrayList {size = 2} [
        [0] : list1,
        [1] : list2
    ],
    map = HashMap {size = 2} [
        [key2] -> value2,
        [key1] -> value1
    ],
    name = Harry,
    sex = male,
    shoes = Shoes[2] : [
        Shoes {
            color = red,
            name = A
        },
        Shoes {
            color = blue,
            name = B
        }
    ],
    age = 25
}
2017-06-12 18:02:36.397 I/TAG(MainActivity.java:159) : Current process(4993) use cpu : 2.78%
Current cpu rate : 4.97%
```

#### 记录崩溃日志

```java
        LogUtils.initialize(LogConfig.newBuilder()
                .reportCrash(true) // Report crash logs
                .build());
```

```java
        // Cause crash
        new Handler().postDelayed(new Runnable() {
            @Override
            public void run() {
                String str = null;
                str.length();
            }
        }, 30000l);
```

在文件中可看到如下信息：

```xml
2017-06-12 18:02:47.984 E/TAG(CrashHandler.java:67) : AndroidRuntime: Shutting down VM
java.lang.NullPointerException
	at com.dftc.logdemo1.MainActivity$2.run(MainActivity.java:80)
	at android.os.Handler.handleCallback(Handler.java:733)
	at android.os.Handler.dispatchMessage(Handler.java:95)
	at android.os.Looper.loop(Looper.java:136)
	at android.app.ActivityThread.main(ActivityThread.java:5017)
	at java.lang.reflect.Method.invokeNative(Native Method)
	at java.lang.reflect.Method.invoke(Method.java:515)
	at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:779)
	at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:595)
	at dalvik.system.NativeStart.main(Native Method)
```

—— SnailStudio 后记于 2017.5
