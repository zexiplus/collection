#### nightWatch

**配置文件 nightwatch.conf.js（nightwatch.json）示例**

[配置详情](http://nightwatchjs.org/gettingstarted#settings-file)

```js
require('babel-register')
var config = require('../../config')

// http://nightwatchjs.org/gettingstarted#settings-file
module.exports = {
  src_folders: ['test/e2e/specs'], // 测试的文件夹（相对于根目录）
  output_folder: 'test/e2e/reports', // 输出log的文件夹（相对于根目录）
  custom_assertions_path: ['test/e2e/custom-assertions'], //自定义断言文件夹（相对于根目录）

  selenium: {
    //自定义selenium-server 配置
    start_process: true, 
    // selenium-server(测试服务)程序路径 e.g: bin/selenium-server-standalone-2.43.0.jar
    server_path: require('selenium-server').path, 
    host: '127.0.0.1',
    port: 4444,
    cli_args: {
      'webdriver.chrome.driver': require('chromedriver').path
      // 'webdriver.ie.driver', 'webdriver.firefox.profile'
    }
  },

  test_settings: {
    default: {
      launch_url : "http://localhost", // 可在测试文件browser.lanuchUrl 拿到
      selenium_port: 4444,
      selenium_host: 'localhost',
      silent: true,
      globals: {
        devServerURL: 'http://localhost:' + (process.env.PORT || config.dev.port)
      }
    },

    chrome: {
      desiredCapabilities: {
        browserName: 'chrome',
        javascriptEnabled: true,
        acceptSslCerts: true
      }
    },

    firefox: {
      desiredCapabilities: {
        browserName: 'firefox', 
        javascriptEnabled: true,
        acceptSslCerts: true
      }
    }
  }
}

```



**测试文件 test.js 示例**

[api参考](http://nightwatchjs.org/api)

```js
module.exports = {
  'default e2e tests': function (browser) {
    // automatically uses dev Server port from /config.index.js
    // default: http://localhost:8080
    // see nightwatch.conf.js
    const devServer = browser.globals.devServerURL

    browser
      .url(devServer)
      .waitForElementVisible('#app', 5000)
      .assert.elementPresent('.hello')
      .assert.containsText('h1', 'Welcome to Your Vue.js App')
      .assert.elementCount('img', 1)
      .end()
  }
}

```

