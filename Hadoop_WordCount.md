# Hadoop 实例

## 事前准备
 新建input文件夹，新建一个test.txt文件，输入下面内容：
 
      the weather is good
      today is good
      good weather is good
      today has good weather

## IntelliJ 运行WordCount   
### 新建项目
在Intellij中点击File->New->Project，在弹出的对话框中选择Maven，JDK选择1.8，点击Next。

### 把WordCount代码拷入

![project](https://github.com/RogerGold/media/blob/master/wordCount_project.png)

### 添加依赖项

![depandence](https://github.com/RogerGold/media/blob/master/wordCount_depandence.png)

### 接下来打开project structure。 点击左侧的artifacts，添加一个空jar.

![artifacts](https://github.com/RogerGold/media/blob/master/wordCount_artifacts.png)

### 接下来，点击右上角的edit configurations，如果当前没有application则新建一个application

![application](https://github.com/RogerGold/media/blob/master/wordCount-application.png)

### 运行
运行成功后到Output目录，代开生成的文件查看结果，注意，下次运行时需要把Output目录删除，否则会报目录已存在错误。

![output](https://github.com/RogerGold/media/blob/master/wordCount_output.png)
