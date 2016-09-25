# nightwatch安裝/設定/測試

nightwatch是一套web service自動化測試框架，基於node.js撰寫並使用Selenium WebDriver API，開發者僅需編寫測試規範，就可以快速實作瀏覽器自動測試。本篇文章以OS X示範，分為以下三部分說明。
1. 下載安裝
2. 參數設定
3. 執行測試

參考資料：
- nightwatch官方網站：http://nightwatchjs.org/guide#guide


## 下載安裝

#### 1.下載安裝node.js和NPM
至[node.js官網](https://nodejs.org)下載最新node.js版本，下載後進行安裝，系統將一併完成node.js及NPM(node package manager)的安裝。安裝完成後，啟動mac電腦的**terminal**工具，輸入`node -v`或`npm -v`，看到版本號即表示安裝成功。
![](https://i.imgur.com/27D5cTt.png)

#### 2.通過NPM下載安裝nightwatch
在terminal中，輸入`npm install -g nightwatch`，NPM會自動下載及安裝nightwatch，安裝完成後輸入`nightwatch -v`，看到版本號即表示安裝成功。
![](https://i.imgur.com/pL3bUW5.png)

#### 3.下載selenium
至[selenium download page](http://selenium-release.storage.googleapis.com/index.html)下載最新正式版selenium，請下載**selenium-server-standalone-{版號}.jar**的檔案，範例下載的是**selenium-server-standalone-2.53.0.jar**的版本。
![](https://i.imgur.com/5W73teL.png)

#### 4.下載chrome driver
至[ChromeDriver binary](http://chromedriver.storage.googleapis.com/index.html)下載最新版本chrome driver，範例下載的是**2.9/chromedriver_mac32.zip**，下載後進行解壓縮並安裝。
![](https://i.imgur.com/CsGNABj.png)

## 參數設定
#### 1.整理專案資料夾
將相關檔案放在同一個專案資料夾，目前應該會包含有
1. selenium-server-standalone-2.53.1.jar
2. chromedriver
3. 請新增一個"tests"資料夾，之後用來存放我們撰寫的測試案例

#### 2.新增nightwatch.json參數檔
請新增叫做nightwatch.json的參數檔案，並貼上以下資料作為內容。在這份參數檔中，我們指定了:
- 測試來源為tests資料夾
- selenium自動啟動及server_path
- 偏好的測試瀏覽器為chrome
```
{
  "src_folders" : ["./tests"],
  "output_folder" : "reports",
  "custom_commands_path" : "",
  "custom_assertions_path" : "",
  "page_objects_path" : "",
  "globals_path" : "",

  "selenium" : {
    "start_process" : true,
    "server_path" : "./selenium-server-standalone-2.53.1.jar",
    "log_path" : "",
    "host" : "127.0.0.1",
    "port" : 4444,
    "cli_args" : {
      "webdriver.chrome.driver" : "./chromedriver",
      "webdriver.ie.driver" : ""
    }
  },

  "test_settings" : {
    "default" : {
      "launch_url" : "http://localhost",
      "selenium_port"  : 4444,
      "selenium_host"  : "localhost",
      "silent": true,
      "screenshots" : {
        "enabled" : false,
        "path" : ""
      },
      "desiredCapabilities": {
        "browserName": "chrome",
        "javascriptEnabled": true,
        "acceptSslCerts": true
      }
    },

    "chrome" : {
      "desiredCapabilities": {
        "browserName": "chrome",
        "javascriptEnabled": true,
        "acceptSslCerts": true
      }
    }
  }
}

```
#### 2.新增nightwatch.conf.js參數檔
```
module.exports = (function(settings) {
  settings.test_workers = false;
  return settings;
})(require('./nightwatch.json'));
```

## 執行測試

#### 1.新增測試案例
在**tests資料夾**，新增nightwatch官網developer guide所寫的測試**test1.js**檔案，這個檔案使用node.js撰寫，測試開啟google網站並搜尋"nightwatch"關鍵字，頁面上顯示night watch相關內容。
```
module.exports = {
  'Demo test Google' : function (browser) {
    browser
      .url('http://www.google.com')
      .waitForElementVisible('body', 1000)
      .setValue('input[type=text]', 'nightwatch')
      .waitForElementVisible('button[name=btnG]', 2000)
      .click('button[name=btnG]')
      .pause(2000)
      .assert.containsText('#main', 'Night Watch')
      .end();
  }
};
```
#### 2.執行測試
在**terminal**中，切換到專案資料夾路徑，輸入
`nightwatch tests/test1.js`
電腦將自動開啟chrome瀏覽器，啟動selenium伺服器，進行test1.js的案例測試，然後顯示測試結果。
![](https://i.imgur.com/AJ3Iysh.png)
