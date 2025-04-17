---
title: "接口自动化-typescript+axios1"
date: 2022-06-10T10:13:51.000Z
tags: [typescript]
categories: [测试]
---
@[TOC](导航)

# 必要的环境
1. nodejs
2. typescript
```shell
"本机node版本"
v16.15.0
```

# 第一步 项目创建及配置文件
- 创建一个文件夹  暂且取名demo_ts
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
    "axios": "^0.25.0",
    "ts-node": "^10.4.0",
    "typescript": "^4.5.4"
  },
  "devDependencies": {
    "@types/jest": "^27.0.3",
    "jest": "^27.4.5",
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

-至此 目录结构如下
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/b1b262f5811866e80b962823ddc60e8c.png)

# 代码部分
- 首先创建一个文件夹src  src里面分别创建文件夹 apis、 testcases、utils
 -  在utils文件夹创建一个request.ts文件  对axios进行简单的封装 代码如下 （封装可参考axios官方文档）
 ```ts
 import axios from "axios";

const service = axios.create({
    timeout: 5000
})
service.interceptors.request.use(
    function (config){
        if (config.headers.token) {
            config.headers.authorization = config.headers.token
        }
        if (config.method === 'post' || config.method === 'put') {
            config.headers.contentType = 'application/json'
        }
        return config
    },
    error => {
        return Promise.reject(error)
    }
)
service.interceptors.response.use(function (response: any) {
        if (response.data.success === true) {
            return response
        }
    },
    error => {
        return Promise.reject(error)
    }
)
export const Method:any = {
    GET: 'get',
    POST: "post",
    PUT: 'put',
    DELETE: 'delete'
}
export default service

 
    
  ```
 
 - 在apis创建接口文件  user.ts  代码如下  IP及api请自行修改 此处只是举例 无法使用
 ```ts
	import request, {Method} from "../utils/request";

const default_ip = 'http://www.baidu.com'

export const login = ( data = {}, headers = {}) => {
    return request({
        url:  default_ip + '/api/user/login',
        method: Method.POST,
        data,
        headers
    })
}
 ```
# 创建用例
 - 在tastcases文件夹创建文件  命名规则  xxx.test.ts   此处文件名为 user.test.ts  代码如下
 ```ts
import {login} from "../apis/user";

test('api_test', () => {
    return login({
        'password': "2d3383fa392936ad72d3383fa392936a",
        'phone': "13456789123"
    }).then( res => {
        expect(res.data.code).toBe(200)
    })
})
 ```
# 运行用例 打开 package.json  选择运行test-c 
 运行后会执行用例并生成测试报告  
 如图 coverage/lcov-report/index.html是测试web版 测试报告
 	 ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/267ff7c65f855700a67525ecd5110734.png)

