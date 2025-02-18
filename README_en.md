# ESPhome E-ink 2.9" series sample

<a href="https://www.buymeacoffee.com/xangin" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/default-orange.png" alt="Buy Me A Coffee" height="30" width="130"></a>

正體中文 | [English](https://github.com/xangin/esphome_eink2.9_series/blob/main/README_en.md)

This project uses a 2.9" e-ink display with ESPhome and Home Assistant, allowing for customizable layouts.

You can add more pages and control page switching through HA, for example, when the clothes are done washing, the screen will automatically switch to a message prompt.

You can design how many pages or what to display freely.

- Product size: Width 88mm x Height 47mm

<img src="https://i.imgur.com/ce0TzbS.jpg" width="50%" />

- Back of the product

<img src="https://i.imgur.com/h4HkUgz.jpg" width="50%" />


# Sample references:

## Sample 1. eink_clock.yaml

A simple clock that only displays the date and time.

<img src="https://i.imgur.com/N9xaPfd.jpg" width="66%" />


## Sample 2. eink_clock_with_forecast.yaml

In addition to the date and time, it also reads the weather forecast icon and temperature from HA.

<img src="https://i.imgur.com/8LQM3R7.jpg" width="66%" />


## Sample 3. eink_clock_with_temp.yaml

In addition to the date and time, it also reads a temperature and humidity entity from HA.

<img src="https://i.imgur.com/G7seg0c.jpg" width="66%" />

## Sample 4. eink_clock_with_pages.yaml

In addition to the date and time, it also includes a message page.

<img src="https://user-images.githubusercontent.com/56766371/280905123-8fde26c9-1ff2-4ab5-92ce-a17f62dd761a.jpg" width="66%" />

## Hardware 硬體架構

- [Waveshare 2.9 inch Black and White E-ink Screen](https://detail.tmall.com/item.htm?id=605757420567) - Without casing
- [E-ink Screen Driver Board ESP8266](https://oshwhub.com/lingdy2012/mo-shui-ping-_esp8266-qu-dong-ban-_0603_wos_v0-1) - Available on Xianyu
- [E-ink Screen Casing (EW029F2(2.9 inch single battery))](https://item.taobao.com/item.htm?id=601700008521)- Case


## Installation

1. Place the files in the `/fonts` folder into the HA/config/esphome folder.
2. Place any YAML into HA/config/esphome, modify the content as desired, compile, and flash it to the ESP module via USB.
3. For reading information from HA, the ESPhome device must be added to HA for accurate display of information.

## ESPHome yaml

### Page switching control in HA

There are 2 buttons that display p1 (Time Page) and p2 (Message Page) when pressed. If you want to add more pages, you can follow the code to add more.

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

### Display icons based on Wi-Fi strength

說明: 

- Greater than or equal to -60 shows three bars
- -60~-70 shows two bars
- -70~-75 shows one bar
- -75~-85 shows zero bars
- Less than -85 shows disconnected

You can freely change the strength range to display the number of bars.

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

### Modify custom message content

Steps:

1. In the font declaration `msg_font`, write out **all** the Chinese characters to be displayed first for proper display.

```YAML
font:
  - file: "fonts/NotoSansTC-Medium.ttf"
    id: msg_font
    size: 40
    glyphs: 衣服已經洗拿去烘好囉!趕快收起來ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz,."%-~_:°
```

2. In the `display` p2, replace the text you want to show. Based on the current size setting, one line displays `7 Chinese characters`. Exceeding this limit will not display correctly.

```YAML
display:
  ...
      - id: p2
        lambda: |- 
          it.printf(150,15, id(msg_font), TextAlign::TOP_CENTER, "衣服已經洗好囉!");
          it.printf(150,70, id(msg_font), TextAlign::TOP_CENTER, "趕快拿去烘~");
```

### Add more pages

Steps: Follow the button and display code to add more groups. For example, below is a third page that shows the clothes are done drying when pressed.

Remember to add any Chinese characters not in `msg_font` to ensure proper display.

```YAML

button: #Copy to the very bottom of the button code, do not write button repeatedly!
  ...
  - platform: template
    name: "Show Dryer Done Page"
    icon: 'mdi:update'
    on_press:
      then:
        - display.page.show: p3
        - component.update: my_display


display: #Copy to the very bottom of the display code, do not write display repeatedly!
  ...
      - id: p3
        lambda: |- 
          it.printf(150,15, id(msg_font), TextAlign::TOP_CENTER, "衣服已經烘好囉!");
          it.printf(150,70, id(msg_font), TextAlign::TOP_CENTER, "趕快收起來!!");
```

