# ArcadeNFC Ver.Alpha(2022/11/6)
本批次为技术验证与兼容性测试版本。由于外壳厂家提供数据不全，PCB高度存在差异，所以对开孔位置进行了修正，使用热熔胶固定，后续版本中会修复。

目前已支持: SBGA、SPICE、PN532直通模式，但暂时没有开发模式切换功能，仅能在源代码中进行切换。由于北京疫情一日三变，恐不能及时发出，故先发出两个读卡器，进行技术验证与兼容性测试。

两读卡器分别为SBGA和SPICE协议，均采用ESP32-S2 SoC，使用esp-idf编写，使用经过修改的tinyusb-cdc作为串口实现方式，理论上支持自适应波特率与免驱(在win10/win11上测试过)

其中的SBGA通讯协议与游戏串口号借鉴了[Arduino-Aime-Reader](https://github.com/Sucareto/Arduino-Aime-Reader)

## SBGA协议: 绿色指示灯，与游戏自检通讯完成后跟随游戏灯光。
### 使用方法:
1.根据下列方法(来自[Arduino-Aime-Reader](https://github.com/Sucareto/Arduino-Aime-Reader))设置串口号
| 游戏代号 | COM端口号 | 支持的卡 | 默认波特率 |
| - | - | - | - |
| SDDT/SDEZ | COM1 | FeliCa,MIFARE | 115200 |
| SDEY | COM2 | MIFARE | 38400 |
| SDHD | COM4 | FeliCa,MIFARE | cvt=38400,sp=115200 |
| SBZV/SDDF | COM10 | FeliCa,MIFARE | 38400 |
| SDBT | COM12 | FeliCa,MIFARE | 38400 |
- 有使用 amdaemon 的，可以参考 config_common.json 内 aime > unit > port 确认端口号

2.使用segatools的,在segatools.ini设置以下选项来关闭Aime读卡器模拟
```ini
[aime]
enable=0
```
## SPICE协议:紫色指示灯，根据刷卡结果反馈为绿色与红色。
### 使用方法:
打开spicecfg.exe,在`Options`中找到`API Serial Port`，设置为读卡器的串口号，并将下面的`API Serial Baud`设置为`115200`。
## 测试支持:
目前仅在SDEZ与KFC上测试，均可正常使用。
## 错误处理:
### 红色指示灯常亮:
PN532链接失败，拆开外壳，尝试重新插拔PN532模组
### SBGA游戏无法使用:
在TEST模式中查看读卡器是否连接成功，尝试更换波特率。
### Spice无法使用:
打开spicecfg.exe,在`Options`中找到`API Verbose Logging`和`API Debug Mode`，设置为Enabled，打开游戏 查看错误日志。

### 如依旧无法解决，请联系或发issues。
## 已知问题:
Spice读卡器设置为了1P，后续开发模式切换后会解决此问题。
