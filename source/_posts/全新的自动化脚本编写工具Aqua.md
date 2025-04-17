---
title: "全新的自动化脚本编写工具Aqua"
date: 2022-11-24 16:47:26
tags: [tools]
categories: [工具]
---

# 废话模式
```text
最近我们熟悉的JetBrains家族继Fleet后又迎来一位新成员Aqua
看了一下官方简介 定义为测试自动化工具
目前版本为预览版 在官方网站下载或者在Toolbox直接安装  预览版无需激活
下面开始本期体验
```

# 主界面及创建项目界面
`首先界面依旧是JetB家族风格  我是通过Toolbox安装的 它为啥会显示部分中文目前未知`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/815ebb246016da56722892fe8da827eb.png)
`因为我看见Aqua的第一眼亮点是快速进行元素定位，所以这里看看selenium`
`从上图可以看出目前直接创建selenium项目，语言只支持java、kotlin和Groovy这三个jvm系语言`
`后期可能会加上其他语言，毕竟目前只是预览版`
`如果想要创建其他语言版本，可以选择新建项目 如图`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/6789a1c672a92f922946290b496514b5.png)
# 创建selenium项目
`这里选择创建selenium项目，使用Java+Maven+TestNG 如图 选择添加示例代码`

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/1f0e3574accc70b945a225d1432395ae.png)
`下一步为选择selenium版本，报告及断言库  下图为默认选项  我不用selenide 所以干掉它  点创建`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/22a571142be84f5f4dbe6ce8aad07ea9.png)

==这里先说下界面吧==
`下图为默认界面，看着和Fleet一样 这个可以在设置里面修改`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/bca725575a05d3b4f40abf1d9d4c4ebd.png)
==目录结构==
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/50ecc4288ccf6d84f1386fe7c1566349.png)

### pom.xml 和之前自己写的是一样的
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>AquaExperience</artifactId>
    <version>1.0-SNAPSHOT</version>
    <name>AquaExperience</name>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.target>18</maven.compiler.target>
        <maven.compiler.source>18</maven.compiler.source>
        <aspectj.version>1.9.9.1</aspectj.version>
        <allure.version>2.19.0</allure.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-java</artifactId>
            <version>4.4.0</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>io.qameta.allure</groupId>
            <artifactId>allure-testng</artifactId>
            <version>${allure.version}</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.testng</groupId>
            <artifactId>testng</artifactId>
            <version>7.6.1</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-simple</artifactId>
            <version>1.7.30</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.22.2</version>
                <configuration>
                    <testFailureIgnore>true</testFailureIgnore>
                    <argLine>
                        -javaagent:"${settings.localRepository}/org/aspectj/aspectjweaver/${aspectj.version}/aspectjweaver-${aspectj.version}.jar"
                    </argLine>
                    <systemProperties>
                        <property>
                            <name>allure.results.directory</name>
                            <value>${project.build.directory}/allure-results</value>
                        </property>
                    </systemProperties>
                </configuration>
                <dependencies>
                    <dependency>
                        <groupId>org.aspectj</groupId>
                        <artifactId>aspectjweaver</artifactId>
                        <version>${aspectj.version}</version>
                    </dependency>
                </dependencies>
            </plugin>
            <plugin>
                <groupId>io.qameta.allure</groupId>
                <artifactId>allure-maven</artifactId>
                <version>2.11.2</version>
            </plugin>
        </plugins>
    </build>
</project>
```
### page代码
```java
package com.example.aquaexperience;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.FindBy;
import org.openqa.selenium.support.PageFactory;

// page_url = https://www.jetbrains.com/
public class MainPage {
    @FindBy(xpath = "//*[@data-test-marker='Developer Tools']")
    public WebElement seeDeveloperToolsButton;

    @FindBy(xpath = "//*[@data-test='suggestion-action']")
    public WebElement findYourToolsButton;

    @FindBy(xpath = "//div[@data-test='main-menu-item' and @data-test-marker = 'Developer Tools']")
    public WebElement toolsMenu;

    @FindBy(css = "[data-test='site-header-search-action']")
    public WebElement searchButton;

    public MainPage(WebDriver driver) {
        PageFactory.initElements(driver, this);
    }
}

```

### testcase
```java
package com.example.aquaexperience;

import org.testng.annotations.*;

import static org.testng.Assert.*;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

import java.time.Duration;

public class MainPageTest {
    private WebDriver driver;
    private MainPage mainPage;

    @BeforeMethod
    public void setUp() {
        driver = new ChromeDriver();
        driver.manage().window().maximize();
        driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));
        driver.get("https://www.jetbrains.com/");

        mainPage = new MainPage(driver);
    }

    @AfterMethod
    public void tearDown() {
        driver.quit();
    }

    @Test
    public void search() {
        mainPage.searchButton.click();

        WebElement searchField = driver.findElement(By.cssSelector("[data-test='search-input']"));
        searchField.sendKeys("Selenium");

        WebElement submitButton = driver.findElement(By.cssSelector("button[data-test='full-search-button']"));
        submitButton.click();

        WebElement searchPageField = driver.findElement(By.cssSelector("input[data-test='search-input']"));
        assertEquals(searchPageField.getAttribute("value"), "Selenium");
    }

    @Test
    public void toolsMenu() {
        mainPage.toolsMenu.click();

        WebElement menuPopup = driver.findElement(By.cssSelector("div[data-test='main-submenu']"));
        assertTrue(menuPopup.isDisplayed());
    }

    @Test
    public void navigationToAllTools() {
        mainPage.seeDeveloperToolsButton.click();
        mainPage.findYourToolsButton.click();

        WebElement productsList = driver.findElement(By.id("products-page"));
        assertTrue(productsList.isDisplayed());
        assertEquals(driver.getTitle(), "All Developer Tools and Products by JetBrains");
    }
}

```

## Web Inspector体验
`Web Inspector用于快速定位页面元素`
`这里重点说一下，第三步红框里面的加号可以快速生成代码，只需修改变量名即可`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/e71fcfaa592c8df3bd3ac6f6c4dbc6fa.png)
### 验证
`MainPage.java`
```java
package com.example.aquaexperience;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.FindBy;
import org.openqa.selenium.support.PageFactory;

// page_url = https://www.jetbrains.com/
public class MainPage {

    @FindBy(css = "a[data-report-query='spm=3001.4482']")
    public WebElement study;

    public MainPage(WebDriver driver) {
        PageFactory.initElements(driver, this);
    }
}

```

`MainPageTest.java`
```java
package com.example.aquaexperience;

import org.testng.annotations.*;

import static org.testng.Assert.*;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

import java.time.Duration;

public class MainPageTest {
    private WebDriver driver;
    private MainPage mainPage;

    @BeforeMethod
    public void setUp() {
        driver = new ChromeDriver();
        driver.manage().window().maximize();
        driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));
        driver.get("https://www.csdn.net");

        mainPage = new MainPage(driver);
    }

    @AfterMethod
    public void tearDown() {
        driver.quit();
    }

    @Test
    public void studyCode(){
        mainPage.study.click();
        System.out.println(driver.getTitle());
        assertEquals(driver.getTitle(), "让学习更有价值_CSDN学习");
    }
}

```
# 结果
`allure-results目录是测试报告 执行需要allure环境`
`命令 allure serve  allure-results`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/1dc3868bdea95dce4ffd5e891f0b4734.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/42d6a3790cc5c9554a92ef14d3becbcb.png)
## 接口测试
既然测试报告有了，那接口也就有了，顺便试试接口请求功能
```js
POST  http://192.168.56.1:63987/widgets/summary.json
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/7cc71fc5d1c78c8df46c253acf489c89.png)
`总体上感觉还不错，对于使用selenium的同学来说，元素过多的时候优势还是很明显的`
`目前因为还在开发，所以官方文档内容不多，感兴趣的可以去看看`
# 官方文档
[点击跳转](https://www.jetbrains.com/help/aqua/2022.3/getting-started-aqua.html)