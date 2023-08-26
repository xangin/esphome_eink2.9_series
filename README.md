# ESPhome E-ink 2.9" series sample

# 範例參考圖片:

## 1. eink_clock.yaml

單純時鐘僅顯示日期與時間

<img src="https://i.imgur.com/N9xaPfd.jpg" width="66%" />


## 2. eink_clock_with_forecast.yaml

除了日期與時間外，再加上讀取來自HA的天氣預報圖示與氣溫

<img src="https://i.imgur.com/8LQM3R7.jpg" width="66%" />

## 3. eink_clock_with_temp.yaml

除了日期與時間外，再加上讀取來自HA的某個溫度與濕度實體

<img src="https://i.imgur.com/G7seg0c.jpg" width="66%" />

## Installation 安裝方式

1. 將`/fonts`資料夾內的檔案放到HA/config/esphome的資料夾內
2. 將任一YAML放到HA/config/esphome內編譯後燒錄製ESP模組
3. 有讀取HA資訊的必須要將ESPhome裝置加入HA後才正確顯示資訊

