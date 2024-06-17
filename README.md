# ESPhome E-ink 2.9" series sample

<a href="https://www.buymeacoffee.com/xangin" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/default-orange.png" alt="Buy Me A Coffee" height="30" width="130"></a>

本專案為利用2.9"電子紙搭配ESPhome與Home Assistant，可自由調整喜歡的版面

或是加上更多頁面並透過HA控制換頁，例如當衣服洗好時，會自動將畫面切為訊息提示

要有多少頁或是顯示什麼都能自由設計

- 成品尺寸: 寬 88mm x 高 47mm

<img src="https://i.imgur.com/ce0TzbS.jpg" width="50%" />

- 成品背面

<img src="https://i.imgur.com/h4HkUgz.jpg" width="50%" />


# 範例參考:

## 範例1. eink_clock.yaml

單純時鐘僅顯示日期與時間

<img src="https://i.imgur.com/N9xaPfd.jpg" width="66%" />


## 範例2. eink_clock_with_forecast.yaml

除了日期與時間外，再加上讀取來自HA的天氣預報圖示與氣溫

<img src="https://i.imgur.com/8LQM3R7.jpg" width="66%" />


## 範例3. eink_clock_with_temp.yaml

除了日期與時間外，再加上讀取來自HA的某個溫度與濕度實體

<img src="https://i.imgur.com/G7seg0c.jpg" width="66%" />

## Hardware 硬體架構

- [微雪 2.9吋黑白墨水屏裸屏](https://detail.tmall.com/item.htm?id=605757420567) - 不帶外殼
- [墨水屏驅動板 ESP8266](https://oshwhub.com/lingdy2012/mo-shui-ping-_esp8266-qu-dong-ban-_0603_wos_v0-1) - 閑魚有售
- [电子墨水屏外壳(EW029F2(2.9寸单电池))](https://item.taobao.com/item.htm?id=601700008521)- 外殼


## Installation 安裝方式

1. 將`/fonts`資料夾內的檔案放到HA/config/esphome的資料夾內
2. 將任一YAML放到HA/config/esphome內編譯後燒錄製ESP模組
3. 有讀取HA資訊的必須要將ESPhome裝置加入HA後才正確顯示資訊

