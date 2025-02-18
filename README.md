# ESPhome E-ink 2.9" series sample

<a href="https://www.buymeacoffee.com/xangin" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/default-orange.png" alt="Buy Me A Coffee" height="30" width="130"></a>

正體中文 | [English](https://github.com/xangin/esphome_eink2.9_series/blob/main/README_en.md)

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

## 範例4. eink_clock_with_pages.yaml

除了日期與時間外，還多了訊息頁面

<img src="https://user-images.githubusercontent.com/56766371/280905123-8fde26c9-1ff2-4ab5-92ce-a17f62dd761a.jpg" width="66%" />

## Hardware 硬體架構

- [微雪 2.9吋黑白墨水屏裸屏](https://detail.tmall.com/item.htm?id=605757420567) - 不帶外殼
- [墨水屏驅動板 ESP8266](https://oshwhub.com/lingdy2012/mo-shui-ping-_esp8266-qu-dong-ban-_0603_wos_v0-1) - 閑魚有售
- [电子墨水屏外壳(EW029F2(2.9寸单电池))](https://item.taobao.com/item.htm?id=601700008521)- 外殼


## Installation 安裝方式

1. 將`/fonts`資料夾內的檔案放到HA/config/esphome的資料夾內
2. 將任一YAML放到HA/config/esphome，並將內容修改成自己想要的，編譯後插上USB即可燒錄至ESP模組
3. 有讀取HA資訊的必須要將ESPhome裝置加入HA後才正確顯示資訊

## ESPHome yaml 說明

### 在HA控制換頁

有2個按鈕，按下去分別會去顯示p1(Time Page)與p2(Message Page)，如果有要再新增更多頁可以再仿照程式碼再新增

```YAML
button:
  - platform: template
    name: "Show Time Page"
    icon: 'mdi:clock'
    on_press:
      then:
        - display.page.show: p1
        - component.update: my_display
    
  - platform: template
    name: "Show Message Page"
    icon: 'mdi:update'
    on_press:
      then:
        - display.page.show: p2
        - component.update: my_display
```

### 根據Wi-Fi強度顯示圖示

說明: 
- 大於等於-60顯示三格
- -60~-70顯示兩格
- -70~-75顯示一格
- -75~-85顯示零格
- 小於-85顯示中斷

可自由變更強度範圍要顯示的格數

```YAML
          //wifi signal
          if (id(wifisignal).state >= -60) {
              //Excellent
              it.print(0, 0, id(wifi_font), "\U000F08BE");
          } else if (id(wifisignal).state  >= -70) {
              //Good
              it.print(0, 0, id(wifi_font), "\U000F08BD");
          } else if (id(wifisignal).state  >= -75) {
              //Fair
              it.print(0, 0, id(wifi_font),"\U000F08BC");
          } else if (id(wifisignal).state  >= -85) {
              //Weak
              it.print(0, 0, id(wifi_font),"\U000F08BF");
          } else {
              //Unlikely working signal
              it.print(0, 0, id(wifi_font),"\U000F0783");
          }
```

### 修改自訂訊息內容

作法:

1. 在字型宣告處的`msg_font`將要顯示的中文字**先全部寫出來**這樣才能正常顯示唷!!
```YAML
font:
  - file: "fonts/NotoSansTC-Medium.ttf"
    id: msg_font
    size: 40
    glyphs: 衣服已經洗拿去烘好囉!趕快收起來ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz,."%-~_:°
```
2. 在`display`的p2，置換想要顯示的文字，依目前設定的大小，一行就是顯示`7個中文字`，超過將無法顯示唷!
```YAML
display:
  ...
      - id: p2
        lambda: |- 
          it.printf(150,15, id(msg_font), TextAlign::TOP_CENTER, "衣服已經洗好囉!");
          it.printf(150,70, id(msg_font), TextAlign::TOP_CENTER, "趕快拿去烘~");
```

### 增加更多頁面

作法: 仿照按鈕及顯示的程式碼，可再新增多組，例如下面是第三頁，按下去顯示衣服烘好了

有不在`msg_font`內的中文字要記得新增，這樣才能正常顯示唷!

```YAML

button: #複製在button程式碼的最下面，不可重複寫button唷!
  ...
  - platform: template
    name: "Show Dryer Done Page"
    icon: 'mdi:update'
    on_press:
      then:
        - display.page.show: p3
        - component.update: my_display


display: #複製在display程式碼的最下面，不可重複寫display唷!
  ...
      - id: p3
        lambda: |- 
          it.printf(150,15, id(msg_font), TextAlign::TOP_CENTER, "衣服已經烘好囉!");
          it.printf(150,70, id(msg_font), TextAlign::TOP_CENTER, "趕快收起來!!");
```

