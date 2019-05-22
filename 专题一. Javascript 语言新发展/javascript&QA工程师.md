# 2019年5月20日直播课 JavaScript&QA工程师

## 一、单元测试 -- 小的函数测试 karma 安装和流程
```js
// 1. 安装karma
 npm install karma --save-dev 或者 yarn add karma --dev
//  2. 安装插件
 npm install karma-jasmine karma-chrome-launcher jasmine-core --save-dev
 npm install -g karma-cli
// 3. 初始化karma  不要在 git bush 下运行 
karma init 
// 配置文件 karma.conf.js 
// 4. 写测试文档 index.js  和 index.spec.js
// index.spec.js
describe("测试基本函数API", function(){
  it("+1函数的应用", function(){
    expect(window.add(1)).toBe(2);
  });
})
// index.js
window.add = (a) =>{
  return a+1
}
// 5. 在package.json 中设置
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "unit":"karma start"
  }
// 6. 运行
npm run unit
// 7. 分支测试检测 
npm i karma-coverage --save-dev
// 在karma.conf.js 中配置 
    preprocessors: {
      // source files, that you wanna generate coverage for
      // do not include tests or libraries
      // (these files will be instrumented by Istanbul)
      './tests/unit/**/*.js': ['coverage']
    },

    coverageReporter: {
      type : 'html',
      dir : './docs/coverage/'
    },
    // test results reporter to use
    // possible values: 'dots', 'progress'
    // available reporters: https://npmjs.org/browse/keyword/karma-reporter
    reporters: ['progress','coverage'],
```
## 二、e2e测试 --确保功能 selenium-webdriver 安装和配置
1. 安装测试包 selenium-webdriver
```
npm i selenium-webdriver --save-dev
或者
yarn add selenium-webdriver --dev
```
2. 下载对应的浏览器内核 并解压到浏览器中

[https://www.npmjs.com/package/selenium-webdriver]("https://www.npmjs.com/package/selenium-webdriver")

3.  e2e 测试 又叫端对端测试

## 三、接口测试 确保数据 Mocha 
1.  [Mocha官网]("https://mochajs.org/")
> Mocha是一个在Node.js和浏览器上运行的功能丰富的JavaScript测试框架，使异步测试变得简单而有趣。Mocha测试以串行方式运行，允许灵活准确的报告，同时将未捕获的异常映射到正确的测试用例

2. 安装

```javascript
    npm install --save-dev mocha
```
3. 小demo
```javascript
     // tests/service/router.spec.js
     const axios = require("axios");
    describe("node接口", function () {
        it("test接口测试", function (done) {
            axios.get("http://xcssss.com")
            .then(function(response){
                if(response.data.xx == ""){
                    done();
                }else{
                    done(new Error("数据请求出错"))
                }
            }).catch(function(error){
                done(error);
            })

        })
    });
```