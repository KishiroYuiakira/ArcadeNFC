# ArcadeNFC Release Ver.2.0(2023/6/1)
本批次为正式版本Ver 2.0。

已支持: SEGA、SPICE、PN532直通模式。

读卡器采用ESP32-S2 SoC，使用esp-idf编写，使用经过修改的tinyusb-cdc作为串口实现方式，支持自适应波特率与免驱(在win10/win11上测试过)

其中的SEGA通讯协议与游戏串口号借鉴了[Arduino-Aime-Reader](https://github.com/Sucareto/Arduino-Aime-Reader)

## 网页控制面板使用
### 连接:
使用手机/笔记本等配有WiFi功能的设备扫描并根据以下信息连接读卡器
#### SSID:ArcadeNFC_[设备序列号]
#### 密码:MisakaTech
连接成功后会提示跳转/自动跳转到控制面板,如未自动跳转,请手动访问http://192.168.4.1。
### 使用:
#### Mode Switch菜单
在SEGA、SPICE、RAW模式之间进行切换，切换模式后读卡器会自动重启。
#### Config Edit菜单
##### SEGA Mode Config菜单:
参照下文,分别填入TEST Mode中的H/W(硬件版本号)与F/W(软件版本号)数据,以通过自检(不同游戏版本的硬件与软件版本号可能不同)。
##### SPICE Mode Config菜单:
1P 2P切换:主要在IIDX上使用,SDVX等单人游戏直接选择"1P"
Direct UID:M1 UID直通,开启后可以使用绝大多数NFC卡片登录,但可能与其他型号读卡器存在不兼容现象(本型号读卡器完全兼容)。
##### RAW Mode Config菜单:
设置是否开启串口通信指示灯(有人反馈指示灯太闪,故开发此功能)。
## SEGA协议: 绿色指示灯，与游戏自检通讯完成后跟随游戏灯光。
### 使用方法:
1.打开游戏进程,进入TEST Mode ,打开读卡器控制面板,将TEST Mode中的读卡器H/W(硬件版本号)与F/W(软件版本号)数据填入，保存。
2.根据下列方法(来自[Arduino-Aime-Reader](https://github.com/Sucareto/Arduino-Aime-Reader))设置串口号
| 游戏代号 | COM端口号 | 支持的卡 | 默认波特率 |
| - | - | - | - |
| SDDT/SDEZ | COM1 | FeliCa,MIFARE | 115200 |
| SDEY | COM2 | MIFARE | 38400 |
| SDHD | COM4 | FeliCa,MIFARE | cvt=38400,sp=115200 |
| SBZV/SDDF | COM10 | FeliCa,MIFARE | 38400 |
| SDBT | COM12 | FeliCa,MIFARE | 38400 |
- 有使用 amdaemon 的，可以参考 config_common.json 内 aime > unit > port 确认端口号

3.使用segatools的,在segatools.ini设置以下选项来关闭Aime读卡器模拟
```ini
[aime]
enable=0
```
## SPICE协议:紫色指示灯，根据刷卡结果反馈为绿色与红色。
### 使用方法:
打开spicecfg.exe,在`Options`中找到`API Serial Port`，设置为读卡器的串口号(串口号不能为COM1或COM2,否则会报错)，并将下面的`API Serial Baud`设置为`115200`。
## RAW模式:
PN532模式直通,可参照第三方PN532教程进行读卡、写卡、复制、解密等操作。
## 测试支持:
目前在SDEZ、SDHD、SDDT与KFC上测试，均可正常使用。
## 错误处理:
### 红色指示灯常亮:
PN532链接失败，拆开外壳，尝试重新插拔PN532模组
### SEGA游戏无法使用:
在TEST模式中查看读卡器是否连接成功，依次尝试重新插拔读卡器并重启游戏、更换波特率。
### Spice无法使用:
打开spicecfg.exe,在`Options`中找到`API Verbose Logging`和`API Debug Mode`，设置为Enabled，打开游戏 查看错误日志,按照提示进行修复或咨询卖家。

### 如依旧无法解决，请联系卖家或发issue。
### 电子版说明书: https://github.com/SnowSakuraCN/ArcadeNFC
