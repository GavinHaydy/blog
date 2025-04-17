---
title: "一文入门typescript+selenium进行web自动化测试"
date: 2022-07-14T10:07:03.000Z
tags: [typescript]
categories: [测试]
---
@[toc]

# 创建项目及配置文件
- 创建一个文件夹  暂且取名typescript_web_automated_test
- 在目录内创建包管理  package.json 内容如下


 ```json
{
  "scripts": {
    "build": "tsc",
    "run": "npm run test",
    "test": "jest",
    "test-c": "jest --coverage"
  },
  "jest": {
    "testEnvironment": "node"
  },
  "dependencies": {
    "ts-node": "^10.4.0",
    "typescript": "^4.5.4"
  },
  "devDependencies": {
    "@types/jest": "^27.0.3",
    "@types/selenium-webdriver": "^4.1.1",
    "jest": "^27.4.5",
    "selenium-webdriver": "^4.3.1",
    "ts-jest": "^27.1.2"
  }
}
 ```

- 创建 tsconfig.json  配置如下
```json
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "ES2020",
    "rootDir": "./src",
    "outDir": "./out",
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "skipLibCheck": true,
    "sourceMap": true,

  }
}
```

- 创建 jest.config.ts  代码如下
```json
module.exports = {
  roots: [
    "<rootDir>"
  ],
  testRegex: 'testcases/(.+)\\.test\\.(jsx?|tsx?)$',
  transform: {
    "^.+\\.tsx?$": "ts-jest"
  },
  moduleFileExtensions: ['ts', 'tsx', 'js', 'jsx', 'json', 'node'],
};
```

# 必要环境
==浏览器及对应的驱动==

# 代码
`首先在typescript_web_automated_test目录下创建一个src文件夹 `
`在src目录下分别创建utils、testcases和pages文件夹`

- `在utils目录下新建一个brw.ts文件,代码如下：`
```ts
import {Builder} from "selenium-webdriver";
const chrome = require('selenium-webdriver/chrome')

const service = new chrome.ServiceBuilder()

export class brw{
    public static driver = new Builder()
        .forBrowser('chrome')
        .setChromeService(service)
        .build()
}
```

- `在pages目录下新建一个home_baidu.ts,代码如下`
```ts
import {brw} from "../utils/brw";
import {By} from "selenium-webdriver";

export class Home_baidu extends brw{
    public static open_b(){
        return brw.driver.get('http://www.baidu.com')
}

    public static s_ipt(search_keys:string):any{
        return brw.driver.findElement(By.id('kw')).sendKeys(search_keys)
    }
	
	public static text(){
        return brw.driver.findElement(By.id('kw')).getAttribute('value')
    }
}
```

- `在testcases中创建一个t.test.ts，代码如下：`
```ts
import {Home_baidu} from "../pages/home_baidu";

test('webTest', async () =>{
    Home_baidu.open_b()
    Home_baidu.s_ipt('selenium')

    expect(await Home_baidu.text()).toBe('测试')
})
```

# 运行用例 打开 package.json  选择运行test-c 
`运行结果如图`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/2a2f51a8899af4a63f69f7a6381cd61f.png)
`断言失败，输入框内容是selenium 我们写的预期结果是"测试"`

==到此ts+selenium的简单脚本就完成了==