# Raspberry Pi + ESP32 小比例地面机器人采购清单

更新时间：2026-06-07  
目标：第一代小比例地面机器人，先验证地面收卷、张力检测、计米、安全停机和基础视觉识别。无人机暂缓，用人工辅助模拟解缠。

## 采购原则

- 总预算优先控制在 5000 RMB 内；如果采购 3D 打印机或深度相机，建议拆成第二批。
- 第一批只买能直接帮助闭环验证的东西：底盘、ESP32、电机驱动、收卷、张力、计米、电源、安全。
- 淘宝商品链接易失效，本文使用稳定的淘宝搜索链接，并给出必须满足的型号/规格。采购按规格筛选销量、评价和发货地。
- 从国内带到美国使用的设备，所有插电设备必须确认支持 `100-240 V, 50/60 Hz`，或明确支持美国 `120 V, 60 Hz`。
- 锂电池运输限制多，若不确定，优先到美国购买电池和充电器。

## 架构更新：树莓派 + ESP32

第一代原型建议改为双层控制：

- Raspberry Pi 5：主计算机，负责 USB 摄像头/深度相机、OpenCV、日志、网页控制台、ROS/脚本和任务状态。
- ESP32：底层控制器，负责电机 PWM、编码器、HX711/张力采样、急停和通信中断后的本地安全停机。
- 两者通过 USB 串口连接。树莓派下发高层命令，ESP32 独立执行安全限幅和停机。

不要用树莓派直接做全部电机实时控制；Linux 不是硬实时系统。不要用 ESP32 直接跑视觉；性能和内存都不适合。

## 一页版采购决策

下面这张表是给采购直接执行的版本。`具体候选链接`中，淘宝链接使用商品 ID 形式，可能需要登录或手机端打开；如果链接失效，就用同一行的淘宝搜索链接按“购买规格”筛选。

| 优先级 | 模块 | 买这个版本 | 数量 | 具体候选链接 | 保留搜索链接 | 购买规格 | 备注 |
| --- | --- | --- | ---: | --- | --- | --- | --- |
| 必买 | 主计算机 | Raspberry Pi 5 8GB | 1 | [淘宝候选：makebit 树莓派5 套件/主板](https://item.taobao.com/item.htm?id=604580136526) / [Seeed 树莓派目录](https://www.seeedstudio.com.cn/raspberry-pi) | [淘宝搜索](https://s.taobao.com/search?q=Raspberry%20Pi%205%208GB) | 优先买 8GB 主板 + 官方主动散热器；不要买带大量无关传感器的大套装 | 美国使用建议电源在美国买官方 27W USB-C，避免插头/电压麻烦 |
| 必买 | 底层控制 | ESP32 DevKitC / ESP32-WROOM-32E | 3 | [淘宝候选：ESP32 CP2102 Type-C 模块](https://item.taobao.com/item.htm?id=805161973303) / [乐鑫官方 DevKitC 页面](https://www.espressif.com.cn/zh-hans/products/devkits/esp32-devkitc) | [淘宝搜索](https://s.taobao.com/search?q=ESP32%20DevKitC%20ESP32-WROOM-32E%20%E5%BC%80%E5%8F%91%E6%9D%BF) | 选 ESP32-WROOM-32E、Type-C 或 Micro-USB 均可；CP2102 优先；排针已焊优先 | 低价 ESP32 质量参差，买 3 块备用 |
| 必买 | 底盘 | 小型金属履带底盘，12V，带编码器 | 1 | 暂无稳定单品链接，用搜索链接筛选 | [淘宝搜索](https://s.taobao.com/search?q=%E5%B0%8F%E5%9E%8B%E5%B1%A5%E5%B8%A6%E6%9C%BA%E5%99%A8%E4%BA%BA%E5%BA%95%E7%9B%98%2012V%20%E7%BC%96%E7%A0%81%E5%99%A8%20%E9%87%91%E5%B1%9E) | 12V 减速电机、左右双电机、带 AB 相编码器、承载 1-3kg、卖家给线序 | 不买全尺寸底盘，不买只有玩具 TT 电机的塑料底盘 |
| 必买 | 底盘电机驱动 | TB6612FNG 两块，或 BTS7960 两块 | 2 | [DFRobot TB6612FNG 参考](https://www.robotshop.com/products/dfrobot-tb6612fng-2x12a-dc-motor-driver-module) | [TB6612FNG 搜索](https://s.taobao.com/search?q=TB6612FNG%20%E7%94%B5%E6%9C%BA%E9%A9%B1%E5%8A%A8%20%E6%A8%A1%E5%9D%97) / [BTS7960 搜索](https://s.taobao.com/search?q=BTS7960%2043A%20%E7%94%B5%E6%9C%BA%E9%A9%B1%E5%8A%A8%20%E6%A8%A1%E5%9D%97) | 小底盘电机额定电流 <1A 用 TB6612；更大电流直接 BTS7960 | 不要用 L298N，发热大、效率低 |
| 必买 | 收卷电机 | JGY370 12V 蜗轮蜗杆减速电机，60rpm，自锁 | 1 | [淘宝候选：JGY370 12V 60转](https://item.taobao.com/item.htm?id=550600227459) / [候选 2](https://item.taobao.com/item.htm?id=580096302461) | [淘宝搜索](https://s.taobao.com/search?q=JGY370%2012V%20%E8%9C%97%E8%BD%AE%E8%9C%97%E6%9D%86%E5%87%8F%E9%80%9F%E7%94%B5%E6%9C%BA%2060rpm%20%E8%87%AA%E9%94%81) | 选择 `12V-60转`，带支架/联轴器更好 | 60rpm 是第一版比较稳的速度，后续可换 30rpm |
| 必买 | 收卷电机驱动 | BTS7960 43A / IBT-2 H 桥 | 1-2 | [BTS7960 参考](https://www.icstation.com/mobile/high-power-motor-drive-module-bts7960-driver-chip-current-limit-control-semiconductor-refrigeration-drive-p-15887.html) | [淘宝搜索](https://s.taobao.com/search?q=BTS7960%2043A%20%E7%94%B5%E6%9C%BA%E9%A9%B1%E5%8A%A8%20%E6%A8%A1%E5%9D%97) | 6-27V，PWM/方向控制，散热片完整，板上端子牢固 | 淘宝 43A 是峰值宣传，实际要按散热降额使用 |
| 必买 | 张力/称重 | 首选 S 型拉力传感器 5kg + HX711；易用替代 M5Stack Mini Scales Unit | 2 | [M5Stack Mini Scales U177](https://shop.m5stack.com/products/mini-scales-unit-hx711) / [淘宝候选：HX711+5kg 套件](https://item.taobao.com/item.htm?id=611254073645) | [S 型搜索](https://s.taobao.com/search?q=S%E5%9E%8B%E6%8B%89%E5%8A%9B%E4%BC%A0%E6%84%9F%E5%99%A8%205kg%20HX711%20%E6%A8%A1%E5%9D%97) | 如果做线缆张力，优先 S 型 5kg；如果快速台架验证，M5Stack I2C 模块更省调试 | M5Stack Mini Scales 是称重结构，不是标准 inline 拉力计，机械安装要自己设计 |
| 必买 | 计米 | 橡胶计米轮 + AB 相编码器 | 1-2 | 暂无稳定单品链接，用搜索链接筛选 | [淘宝搜索](https://s.taobao.com/search?q=%E7%BC%96%E7%A0%81%E5%99%A8%E8%AE%A1%E7%B1%B3%E8%BD%AE%20%E5%85%89%E7%94%B5%20%E7%BC%96%E7%A0%81%E5%99%A8%20%E6%A9%A1%E8%83%B6%E8%BD%AE) | 轮径已知、橡胶轮、弹簧压紧、AB 相输出 | 不买纯机械计数器 |
| 必买 | 电源/安全 | 12V 系统、急停、保险丝、XT60、降压 | 1 批 | 暂无稳定单品链接，用搜索链接筛选 | [急停/保险丝/XT60](https://s.taobao.com/search?q=%E6%80%A5%E5%81%9C%E6%8C%89%E9%92%AE%20%E4%BF%9D%E9%99%A9%E4%B8%9D%E5%BA%A7%20XT60%20%E8%88%B9%E5%9E%8B%E5%BC%80%E5%85%B3) / [LM2596](https://s.taobao.com/search?q=LM2596%20%E9%99%8D%E5%8E%8B%E6%A8%A1%E5%9D%97%2012V%E8%BD%AC5V) | 急停必须能物理切断收卷电机；保险丝靠近电池；XT60 统一接口 | 锂电池建议美国买，别让弟弟托运 |
| 建议 | RGB 视觉 | 5MP/1080P UVC USB 摄像头，手动对焦/低畸变 | 1-2 | [淘宝候选：OV5640 USB 摄像模块](https://item.taobao.com/item.htm?id=731931288180) | [淘宝搜索](https://s.taobao.com/search?q=USB%E6%91%84%E5%83%8F%E5%A4%B4%201080P%20%E6%89%8B%E5%8A%A8%E5%AF%B9%E7%84%A6%20%E4%BD%8E%E7%95%B8%E5%8F%98) | UVC 免驱、手动对焦优先、可固定镜头、带 1/4 螺孔或模块孔位 | 细光纤识别先靠 RGB + 补光，不要指望深度图直接看清细线 |
| 建议 | 深度相机 | Intel RealSense D405 | 0-1 | [RealSense 官方 D405](https://store.realsenseai.com/buy-intel-realsense-depth-camera-d405.html) | [淘宝搜索](https://s.taobao.com/search?q=Intel%20RealSense%20D405%20%E6%B7%B1%E5%BA%A6%E7%9B%B8%E6%9C%BA) | 近距离 7-50cm 场景优先；树莓派/电脑 USB 连接 | 预算紧先不买 |
| 替代 | 深度相机 | Orbbec Gemini 2 | 0-1 | [Orbbec 官方 Gemini 2 系列](https://store.orbbec.com/collections/gemini-2-series) / [DigiKey 介绍](https://www.digikey.cn/zh/product-highlight/o/orbbec/gemini-2-stereo-depth-camera) | [淘宝搜索](https://s.taobao.com/search?q=%E5%A5%A5%E6%AF%94%E4%B8%AD%E5%85%89%20Gemini%202%20%E6%B7%B1%E5%BA%A6%E7%9B%B8%E6%9C%BA) | 国产方案，主动双目，USB 3.0 | 若主要在美国开发，D405/RobotShop/B&H 生态更省心 |
| 建议 | 3D 打印机 | Bambu Lab A1 mini 单机版 | 0-1 | [Bambu US Store A1 mini](https://us.store.bambulab.com/collections/best-selling-products/products/a1-mini) | [淘宝搜索](https://s.taobao.com/search?q=Bambu%20Lab%20A1%20mini%203D%E6%89%93%E5%8D%B0%E6%9C%BA) | 如果带去美国，确认 100-240V；也可直接美国买，保修/插头更省心 | 不建议第一批和机器人硬件抢预算 |

### 采购筛选结论

采购时每类只按下面的结论挑，不要在相似型号里反复比较：

- 主计算机：买 Raspberry Pi 5 8GB + 官方主动散热器。不要买 RISC-V/国产派作为第一版主计算机，后续可以做替代验证；第一版要优先保证相机、OpenCV、ROS 和资料生态。
- 底层主控：买普通 ESP32 DevKitC / ESP32-WROOM-32E，买 3 块。不要为了第一版追 ESP32-S3/C3/C6；这些能用，但库、教程和引脚兼容会增加采购和调试变量。
- 底盘：买 12V 金属履带底盘，必须带 AB 相编码器。不要买 4WD 小车底盘或 TT 电机底盘，承载和低速控制都不够稳。
- 电机驱动：底盘小电机用 TB6612FNG；如果卖家标称电机电流不清楚或堵转电流较大，直接买 BTS7960。不要买 L298N。
- 收卷：买 JGY370 12V 60rpm 自锁蜗轮蜗杆电机 + BTS7960。不要买高速无自锁电机。
- 张力：真实线缆张力买 S 型 5kg + HX711；台架快速验证可以买 M5Stack Mini Scales。不要第一版买 50kg 拉力传感器。
- 视觉：先买 UVC USB RGB 摄像头 + 补光。深度相机若预算够，近距离优先 D405；国产替代选 Orbbec Gemini 2。不要指望深度相机直接识别细光纤，细线识别主要靠 RGB、补光和背景控制。
- 3D 打印机：如果预算压在 5000 RMB 内，第一批不买打印机；需要带一台就选 Bambu Lab A1 mini 单机版。AMS、多色套件和大量耗材先不买。

## P0：机器人本体硬件

| 类别 | 采购项 | 数量 | 推荐型号/规格 | 预算 RMB | 淘宝链接 | 必须满足 | 避坑 |
| --- | --- | ---: | --- | ---: | --- | --- | --- |
| 主计算机 | Raspberry Pi 5 | 1 | 8GB 主板 + 官方主动散热器；电源建议美国买官方 27W | 550-950 | [淘宝搜索](https://s.taobao.com/search?q=Raspberry%20Pi%205%208GB) | 能跑 Raspberry Pi OS / Ubuntu；用于视觉、日志和控制台 | 不买大量传感器套装；中国电源到美国可能插头不合适 |
| 底层主控 | ESP32 开发板 | 3 | ESP32 DevKitC / ESP32-WROOM-32E，USB 转串口 CP2102/CH340 均可 | 60-150 | [淘宝搜索](https://s.taobao.com/search?q=ESP32%20DevKitC%20ESP32-WROOM-32E%20%E5%BC%80%E5%8F%91%E6%9D%BF) | 引脚排针已焊或附赠；可在 Arduino IDE / ESP-IDF 使用 | 不买 ESP8266；不买资料很少的异形板 |
| 底盘 | 小型履带机器人底盘 | 1 | 金属履带底盘，12 V 减速电机，带编码器优先 | 350-900 | [淘宝搜索](https://s.taobao.com/search?q=%E5%B0%8F%E5%9E%8B%E5%B1%A5%E5%B8%A6%E6%9C%BA%E5%99%A8%E4%BA%BA%E5%BA%95%E7%9B%98%2012V%20%E7%BC%96%E7%A0%81%E5%99%A8%20%E9%87%91%E5%B1%9E) | 能承载 1-3 kg；卖家提供电机电压、电流、编码器线序 | 不买全尺寸重底盘；不买只有玩具 TT 电机的轻塑料底盘 |
| 底盘驱动 | 双路直流电机驱动 | 2 | 小电机用 TB6612FNG；较大底盘用 BTS7960 | 40-160 | [TB6612FNG](https://s.taobao.com/search?q=TB6612FNG%20%E7%94%B5%E6%9C%BA%E9%A9%B1%E5%8A%A8%20%E6%A8%A1%E5%9D%97) / [BTS7960](https://s.taobao.com/search?q=BTS7960%2043A%20%E7%94%B5%E6%9C%BA%E9%A9%B1%E5%8A%A8%20%E6%A8%A1%E5%9D%97) | 电流余量至少为电机堵转电流的 2 倍 | TB6612 不适合大电流收卷电机 |
| 收卷电机 | 蜗轮蜗杆减速电机 | 1 | JGY370 或同级，12 V，30-60 rpm，自锁，带支架更好 | 80-220 | [淘宝搜索](https://s.taobao.com/search?q=JGY370%2012V%20%E8%9C%97%E8%BD%AE%E8%9C%97%E6%9D%86%E5%87%8F%E9%80%9F%E7%94%B5%E6%9C%BA%2060rpm%20%E8%87%AA%E9%94%81) | 低速、大扭矩、断电不倒转；卖家给额定电流 | 不买高速电机；不买无法固定的裸电机 |
| 收卷驱动 | 大电流电机驱动 | 1 | BTS7960 43 A 模块，或同级 H 桥 | 25-80 | [淘宝搜索](https://s.taobao.com/search?q=BTS7960%2043A%20%E7%94%B5%E6%9C%BA%E9%A9%B1%E5%8A%A8%20%E6%A8%A1%E5%9D%97) | 支持 PWM 调速和正反转；散热片完整 | 不把收卷电机接在小电机驱动上 |
| 张力检测 | S 型拉力传感器 + HX711 | 2 套 | 5 kg 优先；如测试线缆较粗可买 10 kg；M5Stack Mini Scales 可做快速验证 | 120-350 | [淘宝搜索](https://s.taobao.com/search?q=S%E5%9E%8B%E6%8B%89%E5%8A%9B%E4%BC%A0%E6%84%9F%E5%99%A8%205kg%20HX711%20%E6%A8%A1%E5%9D%97) | 配套 HX711；带安装孔；有量程和接线说明 | 小比例原型不要先买 50 kg，低张力分辨率差 |
| 长度计量 | 编码器计米轮 | 1-2 | 橡胶轮 + 光电/霍尔编码器，轮径已知 | 60-180 | [淘宝搜索](https://s.taobao.com/search?q=%E7%BC%96%E7%A0%81%E5%99%A8%E8%AE%A1%E7%B1%B3%E8%BD%AE%20%E5%85%89%E7%94%B5%20%E7%BC%96%E7%A0%81%E5%99%A8%20%E6%A9%A1%E8%83%B6%E8%BD%AE) | 能压住线缆不打滑；编码器输出 ESP32 可读 | 不买只有机械计数、没有电信号输出的计米器 |
| 导向机构 | V 槽导向轮/滑轮 | 4-8 | 小型 V 槽轴承滑轮，带 M4/M5 安装孔 | 60-180 | [淘宝搜索](https://s.taobao.com/search?q=V%E6%A7%BD%E5%AF%BC%E5%90%91%E8%BD%AE%20%E6%BB%91%E8%BD%AE%20%E8%BD%B4%E6%89%BF%20%E5%B0%8F%E5%9E%8B) | 线缆接触面光滑；可固定在铝型材或打印件上 | 不买边缘锋利的金属轮直接接触光纤 |
| 收卷结构 | 卷线盘、光轴、轴承座、联轴器 | 1 套 | 小型卷线盘，8 mm 光轴，KFL08/KP08 轴承座，电机联轴器 | 150-350 | [淘宝搜索](https://s.taobao.com/search?q=%E5%8D%B7%E7%BA%BF%E7%9B%98%20%E5%B0%8F%E5%9E%8B%208mm%E5%85%89%E8%BD%B4%20%E8%BD%B4%E6%89%BF%E5%BA%A7%20%E8%81%94%E8%BD%B4%E5%99%A8) | 卷盘可拆；轴承支撑，不让电机轴直接承重 | 不买封闭式自动卷线器，难加传感器 |
| 电源 | 12 V/3S 电源系统 | 1-2 | 3S 11.1 V 锂电池 2200-5000 mAh，XT60；或美国购买同规格 | 200-500 | [淘宝搜索](https://s.taobao.com/search?q=3S%2011.1V%20%E9%94%82%E7%94%B5%E6%B1%A0%20XT60%202200mah%20%E5%85%85%E7%94%B5%E5%99%A8) | 电池有 Wh 标识；充电器支持美国电压或在美国另买 | 航空运输风险高；不买无品牌无保护电池 |
| 降压 | DC-DC 降压模块 | 3 | LM2596 或 MP1584，12 V 转 5 V；ESP32 使用稳定 5 V/USB 或 3.3 V 稳压 | 20-80 | [淘宝搜索](https://s.taobao.com/search?q=LM2596%20%E9%99%8D%E5%8E%8B%E6%A8%A1%E5%9D%97%2012V%E8%BD%AC5V) | 有电位器/固定输出可选；至少 2 A 余量 | 不让电机电源噪声直接进 ESP32 |
| 安全 | 急停、电源开关、保险丝、XT60 | 1 套 | 急停按钮、保险丝座、船型开关、XT60 公母头、硅胶线 | 80-180 | [淘宝搜索](https://s.taobao.com/search?q=%E6%80%A5%E5%81%9C%E6%8C%89%E9%92%AE%20%E4%BF%9D%E9%99%A9%E4%B8%9D%E5%BA%A7%20XT60%20%E8%88%B9%E5%9E%8B%E5%BC%80%E5%85%B3) | 急停能切断收卷电机电源；保险丝靠近电池 | 不只做软件急停 |
| 测试线缆 | 模拟光纤/皮线光缆 | 1 | 50-100 m 单芯皮线光缆，另备 1 mm 尼龙绳 | 50-180 | [淘宝搜索](https://s.taobao.com/search?q=%E7%9A%AE%E7%BA%BF%E5%85%89%E7%BC%86%20100%E7%B1%B3%20%E5%8D%95%E8%8A%AF) | 先用废线/模拟线测试，不处理带电线 | 不用真实运营中的光缆做初测 |

P0 预计合计：约 2100-4400 RMB。若第一批不买树莓派或先用旧笔记本，约 1495-3420 RMB。

## P1：视觉与光纤识别

深度相机可以纳入考虑，但第一代识别细光纤时，普通 RGB 画面、补光、背景控制和线段检测往往比深度更关键。深度相机主要用于判断障碍物、卷盘/导向机构附近距离和较粗线缆空间关系；细光纤本体仍以 RGB 识别为主。

| 类别 | 采购项 | 数量 | 推荐型号/规格 | 预算 RMB | 淘宝链接 | 采购建议 |
| --- | --- | ---: | --- | ---: | --- | --- |
| RGB 视觉 | USB 摄像头 | 1-2 | 1080P/2K，手动对焦，低畸变，UVC 标准 | 80-300 | [淘宝搜索](https://s.taobao.com/search?q=USB%E6%91%84%E5%83%8F%E5%A4%B4%201080P%20%E6%89%8B%E5%8A%A8%E5%AF%B9%E7%84%A6%20%E4%BD%8E%E7%95%B8%E5%8F%98) | 第一批优先买；可直接接电脑/Raspberry Pi |
| 补光 | LED 灯条/环形灯 | 1-2 | 5 V/12 V，亮度可调，漫反射罩 | 30-120 | [淘宝搜索](https://s.taobao.com/search?q=USB%20LED%E8%A1%A5%E5%85%89%E7%81%AF%20%E4%BA%AE%E5%BA%A6%E5%8F%AF%E8%B0%83%20%E6%91%84%E5%83%8F%E5%A4%B4) | 细线识别非常依赖稳定光照 |
| 深度相机 | Intel RealSense D405 | 0-1 | 近距离深度相机，适合台架和车前近距离 | 1500-2500 | [淘宝搜索](https://s.taobao.com/search?q=Intel%20RealSense%20D405%20%E6%B7%B1%E5%BA%A6%E7%9B%B8%E6%9C%BA) | 美国生态、SDK、ROS 支持好；适合 7-50 cm 近距离 |
| 深度相机 | 奥比中光 Orbbec Gemini 2 | 0-1 | 国产主动双目深度相机，USB 3.0 | 1500-3000 | [淘宝搜索](https://s.taobao.com/search?q=%E5%A5%A5%E6%AF%94%E4%B8%AD%E5%85%89%20Gemini%202%20%E6%B7%B1%E5%BA%A6%E7%9B%B8%E6%9C%BA) | 国产方案纳入考虑；机器人场景资料较多 |
| 视觉计算 | Raspberry Pi 5 / 旧笔记本 | 0-1 | 若 P0 已购买树莓派，此项无需重复购买 | 0-900 | [淘宝搜索](https://s.taobao.com/search?q=Raspberry%20Pi%205%208GB) | ESP32 不适合跑深度相机和视觉算法 |

建议：第一批买 RGB 摄像头和补光；深度相机作为第二批。若只选一个深度相机：近距离细节优先选 D405；国产生态/国内采购便利优先选 Gemini 2。

## P1/P2：制造工具、接线工具和耗材

这部分单独给采购。国内购买小工具和连接器很划算，但带到美国要注意电压、液体/胶水和锂电池。

| 类别 | 采购项 | 数量 | 推荐规格 | 预算 RMB | 淘宝链接 | 带到美国注意 |
| --- | --- | ---: | --- | ---: | --- | --- |
| 3D 打印机 | Bambu Lab A1 mini | 0-1 | A1 mini 单机版；预算够可 A1 | 1500-2500 | [淘宝搜索](https://s.taobao.com/search?q=Bambu%20Lab%20A1%20mini%203D%E6%89%93%E5%8D%B0%E6%9C%BA) | 确认电源 100-240 V；美国可直接买耗材 |
| 打印耗材 | PLA/PETG/TPU | 若干 | PLA 用于验证，PETG 用于耐用件，TPU 用于压轮/缓冲 | 100-400 | [淘宝搜索](https://s.taobao.com/search?q=PLA%20PETG%20TPU%201.75mm%20%E8%80%97%E6%9D%90) | 不必大量带，美国也容易买 |
| 焊接 | 可调温烙铁/焊台 | 1 | 100-240 V，T12/C245 类烙铁头生态 | 150-500 | [淘宝搜索](https://s.taobao.com/search?q=%E7%83%99%E9%93%81%20100-240V%20%E5%8F%AF%E8%B0%83%E6%B8%A9%20%E7%84%8A%E5%8F%B0) | 必须确认 100-240 V；液体助焊剂建议美国买 |
| 接线/压接 | 压线钳和端子套装 | 1 套 | SN-28B、JST-XH/PH、杜邦、KF301、冷压端子 | 150-300 | [淘宝搜索](https://s.taobao.com/search?q=SN-28B%20%E5%8E%8B%E7%BA%BF%E9%92%B3%20JST%20%E6%9D%9C%E9%82%A6%E7%AB%AF%E5%AD%90%E5%A5%97%E8%A3%85) | 很值得国内买；分盒标注规格 |
| 基础工具 | 万用表、剥线钳、斜口钳、内六角、螺丝刀 | 1 套 | 国产优利德/胜利即可；万用表带蜂鸣和电流档 | 200-500 | [淘宝搜索](https://s.taobao.com/search?q=%E4%B8%87%E7%94%A8%E8%A1%A8%20%E5%89%A5%E7%BA%BF%E9%92%B3%20%E6%96%9C%E5%8F%A3%E9%92%B3%20%E5%86%85%E5%85%AD%E8%A7%92%20%E5%A5%97%E8%A3%85) | 手工具可托运；带电池的工具注意电池规则 |
| 机械耗材 | 螺丝、铜柱、轴承、光轴、联轴器 | 1 批 | M2/M3/M4 螺丝包，铜柱，8 mm 光轴，KFL08/KP08，联轴器 | 200-500 | [淘宝搜索](https://s.taobao.com/search?q=M3%20%E8%9E%BA%E4%B8%9D%E5%8C%85%20%E9%93%9C%E6%9F%B1%20%E8%BD%B4%E6%89%BF%20%E5%85%89%E8%BD%B4%20%E8%81%94%E8%BD%B4%E5%99%A8) | 国内买规格全且便宜 |
| 电气耗材 | 硅胶线、热缩管、扎带、面包板、杜邦线 | 1 批 | 20/22/24 AWG 线，热缩管套装，扎带 | 100-250 | [淘宝搜索](https://s.taobao.com/search?q=%E7%A1%85%E8%83%B6%E7%BA%BF%20%E7%83%AD%E7%BC%A9%E7%AE%A1%20%E6%9D%9C%E9%82%A6%E7%BA%BF%20%E9%9D%A2%E5%8C%85%E6%9D%BF) | 值得国内带 |

## 美国兼容性与运输注意

- 美国常见市电为 120 V / 60 Hz，插头 Type A/B。所有打印机、焊台、充电器、电源适配器必须确认 `100-240 V, 50/60 Hz` 或美国电压版本。
- 备用锂电池/充电宝通常必须随身携带；100 Wh 以下规则最简单，101-160 Wh 需要航空公司批准，超过 160 Wh 通常禁止。采购电池时要求卖家提供 Wh 标识。
- 电动工具本体一般托运；备用锂电池随身。带工具过安检时以航空公司和 TSA 现场判断为准。
- 胶水、喷剂、清洗剂、助焊剂等液体/易燃品建议到美国购买，不建议从国内带。
- 带到美国后需要继续补货的核心电子件，优先选美国 Amazon、DigiKey、Mouser、Adafruit、SparkFun、RobotShop 也能买到同类替代品的型号。

## 采购批次建议

### 第一批，必须买

- Raspberry Pi 5 8GB + 主动散热：1 套
- ESP32 DevKitC / ESP32-WROOM-32E：3 块
- 小型 12 V 履带底盘，带编码器：1 套
- TB6612FNG 或 BTS7960 电机驱动：2-3 块
- JGY370 12 V 蜗轮蜗杆减速电机：1 个
- S 型 5 kg 拉力传感器 + HX711：2 套
- 编码器计米轮：1-2 套
- V 槽导向轮、卷盘、光轴、轴承座、联轴器：1 批
- 急停、电源开关、保险丝、XT60、硅胶线、降压模块：1 批
- 模拟光纤/皮线光缆 50-100 m：1 卷

第一批目标预算：约 2600-4500 RMB。若暂用旧笔记本替代树莓派，约 2000-3500 RMB。

### 第二批，建议买

- USB 摄像头 + LED 补光
- 3D 打印工具和接线工具
- 结构五金和耗材补充

第二批目标预算：约 800-2500 RMB，不含 3D 打印机。

### 第三批，可选升级

- Intel RealSense D405 或 Orbbec Gemini 2
- Raspberry Pi 5 / Orange Pi 5 / 小主机
- Bambu Lab A1 mini / A1

第三批预算视选择约 2500-8000 RMB。

## 参考资料

- Espressif ESP32-DevKitC V4：官方文档说明该板为小型 ESP32 开发板，大部分 I/O 引脚已引出，适合面包板和跳线原型开发。https://docs.espressif.com/projects/esp-dev-kits/en/latest/esp32/esp32-devkitc/user_guide.html
- Espressif ESP32-WROOM-32E：官方资料列出 Wi-Fi、Bluetooth、GPIO、PWM、I2C、SPI、UART、脉冲计数等外设。https://documentation.espressif.com/esp32-wroom-32e_esp32-wroom-32ue_datasheet_en.html
- Intel RealSense D405：官方规格/资料显示该相机面向近距离深度感知，适合小比例台架和近距测试。https://www.intel.com/content/www/us/en/products/sku/229218/intel-realsense-depth-camera-d405/specifications.html
- Orbbec Gemini 2：官方资料定位为 USB 3.0 主动双目 3D 相机，面向机器人等场景。https://www.orbbec.com/products/stereo-vision-camera/gemini-2/
- Bambu Lab A1 mini：官方技术规格列出输入电压 `100-240 VAC, 50/60 Hz`。https://bambulab.com/pl/a1-mini/tech-specs
- FAA 电池乘机规则：备用锂电池按 Wh 限制，100 Wh 以下最简单，101-160 Wh 需要航司批准。https://www.faa.gov/hazmat/packsafe/airline-passengers-and-batteries
- TSA 电动工具规则：电动工具和备用锂电池有不同携带要求。https://www.tsa.gov/travel/security-screening/whatcanibring/items/power-tools
