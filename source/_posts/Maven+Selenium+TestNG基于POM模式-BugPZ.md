---
title: "Maven+Selenium+TestNG基于POM模式 | BugPZ"
date: 2022-11-08T04:13:22.000Z
---
# Maven+Selenium+TestNG基于POM模式

[2022-11-08](/2022/11/08/Maven+Selenium+TestNG%E5%9F%BA%E4%BA%8EPOM%E6%A8%A1%E5%BC%8F/)

### [](#前言 "前言")前言

```
目前软件测试行业自动化测试分为两大分支，分别使用java和python
python：因为语法简单，拥有大量的三方库，被大量测试人员及培训机构选为第一语言教学。但是因为python有GIL全局解释锁，所以多线程和并发还是java更好
java：语法相对于python这种脚本语言难度偏大，但也拥有强大的社区及三方包，测开的必备语言，如二开jmeter，并发测试等
```

### [](#准备工作 "准备工作")准备工作

*   Jdk
*   Maven
*   Browser and BrpwserDriver

### [](#配置 "配置")配置

*   创建Maven工程 并打开项目下pom.xml文件 写入下面依赖
    
    1  
    2  
    3  
    4  
    5  
    6  
    7  
    8  
    9  
    10  
    11  
    12  
    13  
    14  
    15  
    16  
    17  
    18  
    19  
    20  
    21  
    22  
    23  
    24  
    25  
    26  
    27  
    28  
    29  
    30  
    31  
    32  
    33  
    34  
    35  
    36  
    37  
    38  
    39  
    40  
    41  
    42  
    43  
    44  
    45  
    46  
    47  
    48  
    49  
    50  
    51  
    52  
    53  
    54  
    55  
    56  
    57  
    58  
    
      <project xmlns\="http://maven.apache.org/POM/4.0.0" xmlns:xsi\="http://www.w3.org/2001/XMLSchema-instance"  
      xsi:schemaLocation\="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"\>  
      <modelVersion\>4.0.0</modelVersion\>  
      
      <groupId\>org.example</groupId\>  
      <artifactId\>hellp-world</artifactId\>  
      <version\>1.0-SNAPSHOT</version\>  
        <!--  <packaging>jar</packaging>-->  
      
      <properties\> <!--这里写自己的jdk版本-->  
        <maven.compiler.source\>18</maven.compiler.source\>  
        <maven.compiler.target\>18</maven.compiler.target\>  
        <project.build.sourceEncoding\>UTF-8</project.build.sourceEncoding\>  
      </properties\>  
      
      <dependencies\>  
        <dependency\> <!--testNG依赖-->  
          <groupId\>org.testng</groupId\>  
          <artifactId\>testng</artifactId\>  
          <version\>7.6.1</version\>  
          <scope\>test</scope\>  
        </dependency\>  
      
        <dependency\> <!--selenium依赖-->  
          <groupId\>org.seleniumhq.selenium</groupId\>  
          <artifactId\>selenium-java</artifactId\>  
          <version\>4.5.3</version\>  
        </dependency\>  
      
      </dependencies\>  
      
      
      <build\>  
        <plugins\>  
          <plugin\>  
            <groupId\>org.apache.maven.plugins</groupId\>  
            <artifactId\>maven-compiler-plugin</artifactId\>  
            <configuration\>  
              <encoding\>UTF-8</encoding\>  
            </configuration\>  
          </plugin\>  
      
          <plugin\> <!-- 用例执行配置 使用 mvn test 运行-->  
            <groupId\>org.apache.maven.plugins</groupId\>  
            <artifactId\>maven-surefire-plugin</artifactId\>  
            <version\>2.22.2</version\>  
            <configuration\>  
              <forkCount\>0</forkCount\>  
              <testFailureIgnore\>true</testFailureIgnore\>  
              <suiteXmlFiles\>  
                <suiteXmlFiles\>testng.xml</suiteXmlFiles\>  
              </suiteXmlFiles\>  
            </configuration\>  
          </plugin\>  
        </plugins\>  
      </build\>  
      
    </project\>  
    

### [](#TestNG配置 "TestNG配置")TestNG配置

1  
2  
3  
4  
5  
6  
7  
8  
9  
10  
11  
12  
13  
14  
15  
16  
17  
18  
19  
20  

<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd"\>  
<suite name\="All Test Suite" parallel\="classes" thread-count\="7"\>  
    <!-- 上面parallel为执行用例级别 后面为线程数-->  
    <!--下面为要执行的用例，我是执行所有，所以直接使用package 也可以参照下面注释的写  用例多了较麻烦-->  
    <test verbose\="2" preserve-order\="true" name\="order"\>  
        <packages\>  
            <package name\="org.example.testCase.\*"/>  
        </packages\>  
<!--        <classes>-->  
<!--            <class name="org.example.testCase.AppTest">-->  
<!--                <methods>-->  
<!--                    <include name="openBrowser"/>-->  
<!--                    <include name="eightComponents"/>-->  
<!--                    <include name="quitBrowser"/>-->  
<!--                </methods>-->  
<!--            </class>-->  
<!--        </classes>-->  
    </test\>  
</suite\>  

*   到这里配置就完成了，下面开始撸代码

### [](#首先封装WebDriver "首先封装WebDriver")首先封装WebDriver

*   创建base类 根据参数调实例并返回
    
    1  
    2  
    3  
    4  
    5  
    6  
    7  
    8  
    9  
    10  
    11  
    12  
    13  
    14  
    15  
    16  
    17  
    18  
    19  
    20  
    21  
    22  
    23  
    24  
    25  
    
    package org.theRuffian.common;  
      
    import org.openqa.selenium.WebDriver;  
    import org.openqa.selenium.chrome.ChromeDriver;  
    import org.openqa.selenium.edge.EdgeDriver;  
    import org.openqa.selenium.firefox.FirefoxDriver;  
    import org.openqa.selenium.safari.SafariDriver;  
      
    public class base {  
        static WebDriver driver;  
        public static WebDriver openBrowser(String driverName) {  
            if (driverName.equalsIgnoreCase("chrome")) {  
                driver = new ChromeDriver();  
            } else if (driverName.equalsIgnoreCase("firefox")) {  
                driver = new FirefoxDriver();  
            } else if (driverName.equalsIgnoreCase("edge")) {  
                driver = new EdgeDriver();  
            } else if (driverName.equalsIgnoreCase("safari")) {  
                driver = new SafariDriver();  
            } else {  
                driver = openBrowser("chrome");  
            }  
            return driver;  
        }  
    }  
    
*   创建browserOperation类 封装selenium操作  
    \==这里是因为我需要把等待给封装上,因为selenium4加了新特性，所以可能会麻烦点==
    
    1  
    2  
    3  
    4  
    5  
    6  
    7  
    8  
    9  
    10  
    11  
    12  
    13  
    14  
    15  
    16  
    17  
    18  
    19  
    20  
    21  
    22  
    23  
    24  
    25  
    26  
    27  
    28  
    29  
    30  
    31  
    32  
    33  
    34  
    35  
    36  
    37  
    38  
    39  
    40  
    41  
    42  
    43  
    44  
    45  
    
    package org.theRuffian.common;  
      
      import org.openqa.selenium.\*;  
      import org.openqa.selenium.support.locators.RelativeLocator;  
      import org.openqa.selenium.support.ui.ExpectedConditions;  
      import org.openqa.selenium.support.ui.WebDriverWait;  
      
      import java.time.Duration;  
      import java.util.List;  
      
      import org.openqa.selenium.support.locators.RelativeLocator.\*;  
      
      public class browserOperation {  
          WebDriver driver;  
          public browserOperation(WebDriver driver){  
              this.driver = driver;  
          }  
      
          public WebElement findElement( By locator) {  
              if (locator instanceof RelativeBy) {  
                  return driver.findElement(locator);  
              } else {  
                  return new WebDriverWait(driver, Duration.ofSeconds(5))  
                          .until(ExpectedConditions.presenceOfElementLocated(locator));  
              }  
          }  
      
          public static List<WebElement> findElements(WebDriver driver, By locator) {  
              if (locator instanceof RelativeBy) {  
                  return driver.findElements(locator);  
              } else {  
                  return new WebDriverWait(driver, Duration.ofSeconds(5))  
                          .until(ExpectedConditions.presenceOfAllElementsLocatedBy(locator));  
              }  
          }  
          // 这里举个例子  封装下sendkey和click,直接调用findelement即可  
      
          public void sendKeys(By by, String s){  
              findElement(by).sendKeys(s);  
          }  
      
          public void click(By by){  
              findElement(by).click();  
          }  
      }  
    

### [](#页面 "页面")页面

*   这里页面就相对简单了 只需要把url及元素写出来就行
    *   这里以baidu为例
        
        1  
        2  
        3  
        4  
        5  
        6  
        7  
        8  
        9  
        10  
        11  
        
        package org.theRuffian.pages;  
          
        import org.openqa.selenium.By;  
          
          
        public class homePage{  
            public static String url \= "https://www.baidu.com";  
          
            public static By searchText \= By.id("kw");  
            public static By searchClick \= By.id("su");  
        }      
        

### [](#用例 "用例")用例

1  
2  
3  
4  
5  
6  
7  
8  
9  
10  
11  
12  
13  
14  
15  
16  
17  
18  
19  
20  
21  
22  
23  
24  
25  
26  
27  
28  
29  
30  
31  
32  
33  
34  
35  
36  
37  
38  
39  
40  
41  
42  
43  
44  
45  
46  
47  
48  
49  

package org.example.testCase;  
  
  
import org.openqa.selenium.WebDriver;  
import org.testng.annotations.\*;  
import org.testng.asserts.SoftAssert;  
import org.theRuffian.common.base;  
import org.theRuffian.common.browserOperation;  
import org.theRuffian.pages.homepage;  
  
import java.time.Duration;  
  
  
public class searchTest {  
    private SoftAssert softAssert \= new SoftAssert();  
    WebDriver driver;  
    browserOperation dri;  
  
    @BeforeMethod(alwaysRun = true)  
    public void openBrowser(){  
        driver = base.openBrowser("chrome");  
        dri = new browserOperation(driver);  
        dri.get(homepage.url);  
  
    }  
  
    @Test  
    public void search(){  
        dri.sendKeys(homepage.searchText,"selenium");  
        dri.click(homepage.searchClick);  
        softAssert.assertEquals(driver.getTitle(),"selenium\_百度搜索");  
        softAssert.assertAll();   
    }  
  
    @Test  
    public void search0(){  
        dri.sendKeys(homepage.searchText,"TestNG");  
        dri.click(homepage.searchClick);  
        softAssert.assertEquals(driver.getTitle(),"TestNG\_百度搜索");  
        softAssert.assertAll();  
    }  
  
    @AfterMethod(alwaysRun = true)  
    public void quitBrowser(){  
        System.out.println("关闭");  
        driver.quit();  
    }  
  
}  

### [](#执行用例及结果 "执行用例及结果")执行用例及结果

*   命令行 mvn clean test （根据testNG.xml执行用例）
*   执行完成后在target->surefire-reports->打开index.html查看测试报告

[

赏

谢谢你请我吃糖果



](javascript:;)

*   [Selenium](javascript:void\(0\))
*   [Java](javascript:void\(0\))
*   [Maven](javascript:void\(0\))
*   [TestNG](javascript:void\(0\))

[](javascript:;)

[](javascript:;)[](javascript:;)[](javascript:;)[](javascript:;)[](javascript:;)[](javascript:;)[](javascript:;)[](javascript:;)

[](javascript:;)

扫一扫，分享到微信

![微信分享二维码](images/Maven+Selenium+TestNG%E5%9F%BA%E4%BA%8EPOM%E6%A8%A1%E5%BC%8F)