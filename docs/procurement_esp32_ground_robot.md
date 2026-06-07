# 触觉优先小比例地面机器人采购清单

更新时间：2026-06-07  
目标：第一代小比例地面机器人，先验证机械触觉搜寻、低张力捕获、计米、安全停机和低速回收。视觉只做远程观察和粗方向辅助，不作为第一版成败核心。无人机暂缓，用人工辅助模拟解缠。

## 采购原则

- 总预算优先控制在 5000 RMB 内；已经购买 Bambu Lab A1 后，机器人第一批不再买深度相机和复杂视觉套件。
- 第一批只买能直接帮助闭环验证的东西：底盘、ESP32、触觉搜寻头、导向捕获、张力检测、计米、收卷、电源、安全。
- 淘宝商品链接易失效，本文保留稳定淘宝搜索链接，并给出具体规格。采购按规格筛选销量、评价、发货地和卖家资料完整度。
- 从国内带到美国使用的设备，所有插电设备必须确认支持 `100-240 V, 50/60 Hz`，或明确支持美国 `120 V, 60 Hz`。
- 锂电池运输限制多。NP-F970 这类相机电池通常低于 100 Wh，但仍建议随身携带并保留 Wh 标识。

## 架构更新：触觉优先 + ESP32 底层控制

第一代建议使用三层思路：

- 主计算机：Raspberry Pi 5 / 旧笔记本，负责录像、简单遥控、日志、参数调试和后续视觉实验。
- ESP32：底层控制器，负责电机 PWM、编码器、HX711/张力采样、触觉开关/霍尔/FSR 输入、急停和通信中断后的本地安全停机。
- 机械触觉搜寻头：弹性探针/梳齿阵列 + 漏斗/V 槽导向 + 软夹持轮 + 张力确认。

不要把第一版做成“先看见细光纤再抓取”。细光纤在真实背景里太难稳定视觉识别。第一版应当“瞎扫 + 触觉确认 + 低张力回收”，视觉只负责观察和粗方向。

## 一页版采购决策

下面这张表是给采购直接执行的版本。`具体候选链接`中，淘宝链接使用商品 ID 形式，可能需要登录或手机端打开；如果链接失效，就用同一行的淘宝搜索链接按“购买规格”筛选。

| 优先级 | 模块 | 买这个版本 | 数量 | 具体候选链接 | 保留搜索链接 | 购买规格 | 备注 |
| --- | --- | --- | ---: | --- | --- | --- | --- |
| 必买 | 底层控制 | ESP32 DevKitC / ESP32-WROOM-32E | 3 | [淘宝候选：ESP32 CP2102 Type-C 模块](https://item.taobao.com/item.htm?id=805161973303) / [乐鑫官方 DevKitC 页面](https://www.espressif.com.cn/zh-hans/products/devkits/esp32-devkitc) | [淘宝搜索](https://s.taobao.com/search?q=ESP32%20DevKitC%20ESP32-WROOM-32E%20%E5%BC%80%E5%8F%91%E6%9D%BF) | ESP32-WROOM-32E、CP2102 优先、排针已焊优先 | 买 3 块备用；底层控制不要依赖树莓派实时性 |
| 必买 | 底盘 | 小型金属履带底盘，12V，带编码器 | 1 | 暂无稳定单品链接，用搜索链接筛选 | [淘宝搜索](https://s.taobao.com/search?q=%E5%B0%8F%E5%9E%8B%E5%B1%A5%E5%B8%A6%E6%9C%BA%E5%99%A8%E4%BA%BA%E5%BA%95%E7%9B%98%2012V%20%E7%BC%96%E7%A0%81%E5%99%A8%20%E9%87%91%E5%B1%9E) | 12V 减速电机、左右双电机、带 AB 相编码器、承载 1-3kg、卖家给线序 | 不买 4WD 玩具小车底盘，不买全尺寸底盘 |
| 必买 | 底盘电机驱动 | TB6612FNG 两块，或 BTS7960 两块 | 2 | [DFRobot TB6612FNG 参考](https://www.robotshop.com/products/dfrobot-tb6612fng-2x12a-dc-motor-driver-module) | [TB6612FNG 搜索](https://s.taobao.com/search?q=TB6612FNG%20%E7%94%B5%E6%9C%BA%E9%A9%B1%E5%8A%A8%20%E6%A8%A1%E5%9D%97) / [BTS7960 搜索](https://s.taobao.com/search?q=BTS7960%2043A%20%E7%94%B5%E6%9C%BA%E9%A9%B1%E5%8A%A8%20%E6%A8%A1%E5%9D%97) | 小底盘电机额定电流 <1A 用 TB6612；电流不清楚直接 BTS7960 | 不要用 L298N |
| 必买 | 触觉探针 | 弹性梳齿/探针材料包 | 1 批 | 暂无稳定单品链接，用搜索链接筛选 | [弹簧钢丝](https://s.taobao.com/search?q=0.8mm%201mm%20%E5%BC%B9%E7%B0%A7%E9%92%A2%E4%B8%9D%20304%20%E4%B8%8D%E9%94%88%E9%92%A2) / [尼龙扎带](https://s.taobao.com/search?q=%E5%B0%BC%E9%BE%99%E6%89%8E%E5%B8%A6%20%E5%AE%BD%20%E9%95%BF%20%E9%BB%91%E8%89%B2) / [PET 打包带](https://s.taobao.com/search?q=PET%E6%89%93%E5%8C%85%E5%B8%A6%20%E5%A1%91%E9%92%A2%E5%B8%A6) | 0.6-1.0mm 弹簧钢丝、宽扎带、PET 条都可以买；末端必须圆滑 | 先做被动弹性梳齿，不要第一版做复杂多自由度机械手 |
| 必买 | 触觉传感 | 微动开关 + 霍尔方案 + FSR 三选二 | 1 批 | 暂无稳定单品链接，用搜索链接筛选 | [微动开关](https://s.taobao.com/search?q=%E5%BE%AE%E5%8A%A8%E5%BC%80%E5%85%B3%20%E6%BB%9A%E8%BD%AE%E6%9D%86%20%E9%99%90%E4%BD%8D%E5%BC%80%E5%85%B3) / [霍尔 A3144](https://s.taobao.com/search?q=A3144%20%E9%9C%8D%E5%B0%94%E4%BC%A0%E6%84%9F%E5%99%A8%20%E7%A3%81%E9%93%81) / [FSR 压力传感器](https://s.taobao.com/search?q=FSR%20%E5%8E%8B%E5%8A%9B%E4%BC%A0%E6%84%9F%E5%99%A8%20%E8%96%84%E8%86%9C) | 微动开关最可靠；霍尔适合无接触检测弯曲；FSR 只做实验 | 不要全靠 FSR，漂移和一致性一般 |
| 必买 | 捕获导向 | 宽口漏斗 + V 槽导向轮/滑轮 | 1 批 | 暂无稳定单品链接，用搜索链接筛选 | [V 槽导向轮](https://s.taobao.com/search?q=V%E6%A7%BD%E5%AF%BC%E5%90%91%E8%BD%AE%20%E6%BB%91%E8%BD%AE%20%E8%BD%B4%E6%89%BF%20%E5%B0%8F%E5%9E%8B) / [硅胶轮](https://s.taobao.com/search?q=%E7%A1%85%E8%83%B6%E8%BD%AE%20%E6%A9%A1%E8%83%B6%E8%BD%AE%20%E5%B0%8F%E5%9E%8B%20%E8%BD%B4%E6%89%BF) | 入口做宽，中心做 V 槽；接触边缘大圆角；轮面光滑或软 | 不买边缘锋利的金属轮直接接触光纤 |
| 必买 | 夹持确认 | 软压线轮/夹线轮 + 弹簧预紧 | 1 套 | 暂无稳定单品链接，用搜索链接筛选 | [橡胶压轮](https://s.taobao.com/search?q=%E6%A9%A1%E8%83%B6%E5%8E%8B%E8%BD%AE%20%E7%A1%85%E8%83%B6%E8%BD%AE%20%E5%BC%B9%E7%B0%A7%20%E5%8E%8B%E7%B4%A7) / [小弹簧](https://s.taobao.com/search?q=%E5%B0%8F%E5%BC%B9%E7%B0%A7%20%E6%8B%89%E7%B0%A7%20%E5%8E%8B%E7%B0%A7%20%E5%A5%97%E8%A3%85) | 两个软轮轻夹线，弹簧限力；配合张力传感器确认是否抓到 | 不要刚性夹死，避免损伤光纤 |
| 必买 | 张力/称重 | S 型拉力传感器 5kg + HX711 | 2 | [M5Stack Mini Scales U177](https://shop.m5stack.com/products/mini-scales-unit-hx711) / [淘宝候选：HX711+5kg 套件](https://item.taobao.com/item.htm?id=611254073645) | [S 型搜索](https://s.taobao.com/search?q=S%E5%9E%8B%E6%8B%89%E5%8A%9B%E4%BC%A0%E6%84%9F%E5%99%A8%205kg%20HX711%20%E6%A8%A1%E5%9D%97) | 真实线缆张力优先 S 型 5kg；台架验证可用 M5Stack | 不买 50kg，低张力分辨率差 |
| 必买 | 收卷电机 | JGY370 12V 蜗轮蜗杆减速电机，30-60rpm，自锁 | 1 | [淘宝候选：JGY370 12V 60转](https://item.taobao.com/item.htm?id=550600227459) / [候选 2](https://item.taobao.com/item.htm?id=580096302461) | [淘宝搜索](https://s.taobao.com/search?q=JGY370%2012V%20%E8%9C%97%E8%BD%AE%E8%9C%97%E6%9D%86%E5%87%8F%E9%80%9F%E7%94%B5%E6%9C%BA%2060rpm%20%E8%87%AA%E9%94%81) | 优先 12V 60rpm；如果拉力偏大或控制粗糙，换 30rpm | 低速自锁比高速电机重要 |
| 必买 | 收卷电机驱动 | BTS7960 43A / IBT-2 H 桥 | 1-2 | [BTS7960 参考](https://www.icstation.com/mobile/high-power-motor-drive-module-bts7960-driver-chip-current-limit-control-semiconductor-refrigeration-drive-p-15887.html) | [淘宝搜索](https://s.taobao.com/search?q=BTS7960%2043A%20%E7%94%B5%E6%9C%BA%E9%A9%B1%E5%8A%A8%20%E6%A8%A1%E5%9D%97) | 6-27V，PWM/方向控制，散热片完整，端子牢固 | 淘宝 43A 是峰值宣传，实际降额使用 |
| 必买 | 计米 | 橡胶计米轮 + AB 相编码器 | 1-2 | 暂无稳定单品链接，用搜索链接筛选 | [淘宝搜索](https://s.taobao.com/search?q=%E7%BC%96%E7%A0%81%E5%99%A8%E8%AE%A1%E7%B1%B3%E8%BD%AE%20%E5%85%89%E7%94%B5%20%E7%BC%96%E7%A0%81%E5%99%A8%20%E6%A9%A1%E8%83%B6%E8%BD%AE) | 轮径已知、橡胶轮、弹簧压紧、AB 相输出 | 不买纯机械计数器 |
| 必买 | 电源 | NP-F970 电池 + 电池板/电源板 | 2-4 块电池 + 1-2 板 | 暂无稳定单品链接，用搜索链接筛选 | [NP-F970 电池](https://s.taobao.com/search?q=NP-F970%20%E7%94%B5%E6%B1%A0%207.2V%2045Wh) / [NP-F970 电池板 12V USB-C](https://s.taobao.com/search?q=NP-F970%20%E7%94%B5%E6%B1%A0%E6%9D%BF%2012V%20USB-C%20PD) | 选有 12V 输出和 USB-C/5V 输出的电池板；确认单路输出电流 | 适合小比例原型；高电流大电机仍应换 3S/4S 动力电池 |
| 必买 | 树莓派供电 | 12V/7.4V 输入转 5.1V 5A USB-C 降压 | 1-2 | 暂无稳定单品链接，用搜索链接筛选 | [5.1V 5A USB-C 降压](https://s.taobao.com/search?q=12V%20%E8%BD%AC%205.1V%205A%20USB-C%20%E9%99%8D%E5%8E%8B%20%E6%A0%91%E8%8E%93%E6%B4%BE5) / [USB-C PD 触发降压](https://s.taobao.com/search?q=USB-C%20PD%20%E8%A7%A6%E5%8F%91%20%E9%99%8D%E5%8E%8B%205V%205A) | Raspberry Pi 5 按 5.1V/5A 设计；线短且粗 | 不用 LM2596 小板直接供树莓派 5 |
| 必买 | 安全 | 急停、电源开关、保险丝、XT60/DC5521、硅胶线 | 1 批 | 暂无稳定单品链接，用搜索链接筛选 | [急停/保险丝/XT60](https://s.taobao.com/search?q=%E6%80%A5%E5%81%9C%E6%8C%89%E9%92%AE%20%E4%BF%9D%E9%99%A9%E4%B8%9D%E5%BA%A7%20XT60%20%E8%88%B9%E5%9E%8B%E5%BC%80%E5%85%B3) / [DC5521 线材](https://s.taobao.com/search?q=DC5521%20%E7%94%B5%E6%BA%90%E7%BA%BF%20%E8%BD%AC%E6%8E%A5%E5%A4%B4) | 急停必须物理切断收卷电机；保险丝靠近电池 | 不只做软件急停 |
| 建议 | 主计算机 | Raspberry Pi 5 8GB，或先用旧笔记本 | 0-1 | [淘宝候选：makebit 树莓派5 套件/主板](https://item.taobao.com/item.htm?id=604580136526) / [Seeed 树莓派目录](https://www.seeedstudio.com.cn/raspberry-pi) | [淘宝搜索](https://s.taobao.com/search?q=Raspberry%20Pi%205%208GB) | 如果已有旧笔记本，第一版可暂不买树莓派 | 主计算不是第一版触觉回收的瓶颈 |
| 建议 | 观察视觉 | 1080P UVC USB 摄像头 + 补光 | 1 | [淘宝候选：OV5640 USB 摄像模块](https://item.taobao.com/item.htm?id=731931288180) | [USB 摄像头](https://s.taobao.com/search?q=USB%E6%91%84%E5%83%8F%E5%A4%B4%201080P%20%E6%89%8B%E5%8A%A8%E5%AF%B9%E7%84%A6%20%E4%BD%8E%E7%95%B8%E5%8F%98) / [LED 补光](https://s.taobao.com/search?q=USB%20LED%E8%A1%A5%E5%85%89%E7%81%AF%20%E4%BA%AE%E5%BA%A6%E5%8F%AF%E8%B0%83%20%E6%91%84%E5%83%8F%E5%A4%B4) | UVC 免驱，手动对焦，固定孔位 | 只做远程观察和记录，不做核心识别 |
| 暂缓 | 深度相机 | Intel RealSense D405 / Orbbec Gemini 2 | 0 | [RealSense 官方 D405](https://store.realsenseai.com/buy-intel-realsense-depth-camera-d405.html) / [Orbbec Gemini 2](https://store.orbbec.com/collections/gemini-2-series) | [D405 搜索](https://s.taobao.com/search?q=Intel%20RealSense%20D405%20%E6%B7%B1%E5%BA%A6%E7%9B%B8%E6%9C%BA) / [Gemini 2 搜索](https://s.taobao.com/search?q=%E5%A5%A5%E6%AF%94%E4%B8%AD%E5%85%89%20Gemini%202%20%E6%B7%B1%E5%BA%A6%E7%9B%B8%E6%9C%BA) | 第二版再买 | 第一版不靠深度相机识别细光纤 |

### 采购筛选结论

采购时每类只按下面的结论挑，不要在相似型号里反复比较：

- 搜寻方式：买弹性探针/梳齿材料 + 微动开关/霍尔/FSR，不买复杂机械手。章鱼式柔性机械手可以作为灵感，但第一版先做被动扫线和触觉确认。
- 捕获方式：买漏斗/V 槽导向 + 软压线轮 + 弹簧预紧。线进入中心后用张力传感器确认，不靠视觉判断。
- 回收方式：JGY370 12V 30-60rpm 自锁电机 + BTS7960，必须低速、限力、可反转释放。
- 视觉：只买普通 USB 摄像头和补光用于观察。深度相机第一版暂缓。
- 主计算：如果已有旧笔记本，树莓派可以暂缓；如果想机器人独立运行，再买 Raspberry Pi 5 8GB。
- 电源：NP-F970 可以用，但要通过合适的电池板和降压模块分路供电。不要把树莓派、电机、ESP32 全接到一个便宜小降压板上。

## P0：机器人本体硬件

| 类别 | 采购项 | 数量 | 推荐型号/规格 | 预算 RMB | 淘宝链接 | 必须满足 | 避坑 |
| --- | --- | ---: | --- | ---: | --- | --- | --- |
| 底层主控 | ESP32 开发板 | 3 | ESP32 DevKitC / ESP32-WROOM-32E，USB 转串口 CP2102/CH340 均可 | 60-150 | [淘宝搜索](https://s.taobao.com/search?q=ESP32%20DevKitC%20ESP32-WROOM-32E%20%E5%BC%80%E5%8F%91%E6%9D%BF) | 引脚排针已焊或附赠；可在 Arduino IDE / ESP-IDF 使用 | 不买 ESP8266；不买资料很少的异形板 |
| 底盘 | 小型履带机器人底盘 | 1 | 金属履带底盘，12 V 减速电机，带编码器优先 | 350-900 | [淘宝搜索](https://s.taobao.com/search?q=%E5%B0%8F%E5%9E%8B%E5%B1%A5%E5%B8%A6%E6%9C%BA%E5%99%A8%E4%BA%BA%E5%BA%95%E7%9B%98%2012V%20%E7%BC%96%E7%A0%81%E5%99%A8%20%E9%87%91%E5%B1%9E) | 能承载 1-3 kg；卖家提供电机电压、电流、编码器线序 | 不买全尺寸重底盘；不买只有玩具 TT 电机的轻塑料底盘 |
| 底盘驱动 | 双路直流电机驱动 | 2 | 小电机用 TB6612FNG；较大底盘用 BTS7960 | 40-160 | [TB6612FNG](https://s.taobao.com/search?q=TB6612FNG%20%E7%94%B5%E6%9C%BA%E9%A9%B1%E5%8A%A8%20%E6%A8%A1%E5%9D%97) / [BTS7960](https://s.taobao.com/search?q=BTS7960%2043A%20%E7%94%B5%E6%9C%BA%E9%A9%B1%E5%8A%A8%20%E6%A8%A1%E5%9D%97) | 电流余量至少为电机堵转电流的 2 倍 | TB6612 不适合大电流收卷电机 |
| 触觉搜寻 | 弹性梳齿/探针材料 | 1 批 | 0.6-1.0mm 弹簧钢丝、宽尼龙扎带、PET 打包带，配 3D 打印固定座 | 50-180 | [弹簧钢丝](https://s.taobao.com/search?q=0.8mm%201mm%20%E5%BC%B9%E7%B0%A7%E9%92%A2%E4%B8%9D%20304%20%E4%B8%8D%E9%94%88%E9%92%A2) / [尼龙扎带](https://s.taobao.com/search?q=%E5%B0%BC%E9%BE%99%E6%89%8E%E5%B8%A6%20%E5%AE%BD%20%E9%95%BF%20%E9%BB%91%E8%89%B2) | 末端圆滑；可单根更换；不会割伤光纤 | 不做硬爪直接戳线 |
| 触觉传感 | 微动开关/霍尔/FSR | 1 批 | 滚轮杆微动开关 10-20 个，A3144 霍尔 + 小磁铁，FSR 若干 | 80-250 | [微动开关](https://s.taobao.com/search?q=%E5%BE%AE%E5%8A%A8%E5%BC%80%E5%85%B3%20%E6%BB%9A%E8%BD%AE%E6%9D%86%20%E9%99%90%E4%BD%8D%E5%BC%80%E5%85%B3) / [霍尔](https://s.taobao.com/search?q=A3144%20%E9%9C%8D%E5%B0%94%E4%BC%A0%E6%84%9F%E5%99%A8%20%E7%A3%81%E9%93%81) / [FSR](https://s.taobao.com/search?q=FSR%20%E5%8E%8B%E5%8A%9B%E4%BC%A0%E6%84%9F%E5%99%A8%20%E8%96%84%E8%86%9C) | 每种先少量买，快速试验哪种稳定 | 不全压在 FSR 上 |
| 捕获导向 | V 槽导向轮/滑轮 | 4-8 | 小型 V 槽轴承滑轮，带 M4/M5 安装孔 | 60-180 | [淘宝搜索](https://s.taobao.com/search?q=V%E6%A7%BD%E5%AF%BC%E5%90%91%E8%BD%AE%20%E6%BB%91%E8%BD%AE%20%E8%BD%B4%E6%89%BF%20%E5%B0%8F%E5%9E%8B) | 线缆接触面光滑；可固定在铝型材或打印件上 | 不买边缘锋利的金属轮直接接触光纤 |
| 夹持机构 | 软压线轮 + 弹簧预紧 | 1 套 | 硅胶轮/橡胶轮，小压簧/拉簧，M3/M4 调节螺丝 | 80-220 | [橡胶压轮](https://s.taobao.com/search?q=%E6%A9%A1%E8%83%B6%E5%8E%8B%E8%BD%AE%20%E7%A1%85%E8%83%B6%E8%BD%AE%20%E5%BC%B9%E7%B0%A7%20%E5%8E%8B%E7%B4%A7) / [弹簧套装](https://s.taobao.com/search?q=%E5%B0%8F%E5%BC%B9%E7%B0%A7%20%E6%8B%89%E7%B0%A7%20%E5%8E%8B%E7%B0%A7%20%E5%A5%97%E8%A3%85) | 夹持力可调；必要时打滑保护线缆 | 不刚性夹死 |
| 收卷电机 | 蜗轮蜗杆减速电机 | 1 | JGY370 或同级，12 V，30-60 rpm，自锁，带支架更好 | 80-220 | [淘宝搜索](https://s.taobao.com/search?q=JGY370%2012V%20%E8%9C%97%E8%BD%AE%E8%9C%97%E6%9D%86%E5%87%8F%E9%80%9F%E7%94%B5%E6%9C%BA%2060rpm%20%E8%87%AA%E9%94%81) | 低速、大扭矩、断电不倒转；卖家给额定电流 | 不买高速电机；不买无法固定的裸电机 |
| 收卷驱动 | 大电流电机驱动 | 1 | BTS7960 43 A 模块，或同级 H 桥 | 25-80 | [淘宝搜索](https://s.taobao.com/search?q=BTS7960%2043A%20%E7%94%B5%E6%9C%BA%E9%A9%B1%E5%8A%A8%20%E6%A8%A1%E5%9D%97) | 支持 PWM 调速和正反转；散热片完整 | 不把收卷电机接在小电机驱动上 |
| 张力检测 | S 型拉力传感器 + HX711 | 2 套 | 5 kg 优先；如测试线缆较粗可买 10 kg；M5Stack Mini Scales 可做快速验证 | 120-350 | [淘宝搜索](https://s.taobao.com/search?q=S%E5%9E%8B%E6%8B%89%E5%8A%9B%E4%BC%A0%E6%84%9F%E5%99%A8%205kg%20HX711%20%E6%A8%A1%E5%9D%97) | 配套 HX711；带安装孔；有量程和接线说明 | 小比例原型不要先买 50 kg |
| 长度计量 | 编码器计米轮 | 1-2 | 橡胶轮 + 光电/霍尔编码器，轮径已知 | 60-180 | [淘宝搜索](https://s.taobao.com/search?q=%E7%BC%96%E7%A0%81%E5%99%A8%E8%AE%A1%E7%B1%B3%E8%BD%AE%20%E5%85%89%E7%94%B5%20%E7%BC%96%E7%A0%81%E5%99%A8%20%E6%A9%A1%E8%83%B6%E8%BD%AE) | 能压住线缆不打滑；编码器输出 ESP32 可读 | 不买只有机械计数、没有电信号输出的计米器 |
| 收卷结构 | 卷线盘、光轴、轴承座、联轴器 | 1 套 | 小型卷线盘，8 mm 光轴，KFL08/KP08 轴承座，电机联轴器 | 150-350 | [淘宝搜索](https://s.taobao.com/search?q=%E5%8D%B7%E7%BA%BF%E7%9B%98%20%E5%B0%8F%E5%9E%8B%208mm%E5%85%89%E8%BD%B4%20%E8%BD%B4%E6%89%BF%E5%BA%A7%20%E8%81%94%E8%BD%B4%E5%99%A8) | 卷盘可拆；轴承支撑，不让电机轴直接承重 | 不买封闭式自动卷线器，难加传感器 |
| 电源 | NP-F970 电池系统 | 1 套 | NP-F970 电池 2-4 块，电池板 1-2 个，12V 输出，USB-C/5V 输出 | 300-900 | [NP-F970 电池](https://s.taobao.com/search?q=NP-F970%20%E7%94%B5%E6%B1%A0%207.2V%2045Wh) / [NP-F970 电池板](https://s.taobao.com/search?q=NP-F970%20%E7%94%B5%E6%B1%A0%E6%9D%BF%2012V%20USB-C%20PD) | 电池有 Wh 标识；电池板输出电流明确；接口牢固 | 不买无品牌虚标大容量电池 |
| 降压 | DC-DC 降压模块 | 3 | 树莓派用 5.1V/5A USB-C；ESP32/传感器可用 5V/3A；小模块备用 | 80-220 | [5.1V 5A USB-C 降压](https://s.taobao.com/search?q=12V%20%E8%BD%AC%205.1V%205A%20USB-C%20%E9%99%8D%E5%8E%8B%20%E6%A0%91%E8%8E%93%E6%B4%BE5) / [LM2596](https://s.taobao.com/search?q=LM2596%20%E9%99%8D%E5%8E%8B%E6%A8%A1%E5%9D%97%2012V%E8%BD%AC5V) | 树莓派供电和电机供电分路；共地但不共大电流回路 | 不用 LM2596 给 Pi 5 + 摄像头当主供电 |
| 安全 | 急停、电源开关、保险丝、XT60/DC5521 | 1 套 | 急停按钮、保险丝座、船型开关、XT60 公母头、DC5521 转接、硅胶线 | 100-250 | [淘宝搜索](https://s.taobao.com/search?q=%E6%80%A5%E5%81%9C%E6%8C%89%E9%92%AE%20%E4%BF%9D%E9%99%A9%E4%B8%9D%E5%BA%A7%20XT60%20%E8%88%B9%E5%9E%8B%E5%BC%80%E5%85%B3) | 急停能切断收卷电机电源；保险丝靠近电池 | 不只做软件急停 |
| 测试线缆 | 模拟光纤/皮线光缆 | 1 | 50-100 m 单芯皮线光缆，另备 1 mm 尼龙绳 | 50-180 | [淘宝搜索](https://s.taobao.com/search?q=%E7%9A%AE%E7%BA%BF%E5%85%89%E7%BC%86%20100%E7%B1%B3%20%E5%8D%95%E8%8A%AF) | 先用废线/模拟线测试，不处理带电线 | 不用真实运营中的光缆做初测 |

P0 预计合计：约 1735-4240 RMB。若已有旧笔记本、暂不买树莓派和 USB 摄像头，预算主要集中在机械触觉、底盘、电机、张力和电源。

## F970 供电方案

NP-F970 可以作为第一版小比例机器人电源，尤其适合你已经有相机电池生态的情况。它的优点是通用、安全性相对好、航空携带比大动力电池简单、换电方便。限制是：原生电压约 7.2V，容量约 45Wh；通过电池板升到 12V 后输出电流有限，不适合大功率堵转电机。

推荐接法：

```text
NP-F970 电池
    |
    +-- NP-F970 电池板 12V 输出
    |       |
    |       +-- 保险丝 + 急停 + BTS7960 + 收卷电机
    |       |
    |       +-- 底盘电机驱动
    |
    +-- 单独 5.1V/5A 降压/USB-C
            |
            +-- Raspberry Pi 5

另一路 5V/3.3V：
    +-- ESP32、HX711、霍尔、微动开关上拉、电平模块
```

关键要求：

- 树莓派 5 按 `5.1V / 5A` 供电设计，不建议用普通 LM2596 小板。
- 电机和树莓派分路供电，信号地共地，但电机大电流不要穿过树莓派地线。
- NP-F970 电池板的 `12V 输出` 要看清楚额定电流；常见摄影电池板可能只有 12V/2A 左右，适合小电机，不适合大堵转电流。
- 如果底盘或收卷电机一启动就导致树莓派重启，优先把电机电源和树莓派电源分成两块 NP-F970，或换 3S 动力电池只供电机。
- 急停必须切断电机电源，不能只让 ESP32 停 PWM。

结论：F970 适合第一版小比例原型，但它更像“便携电子/小功率机器人电源”，不是重载动力电源。采购时至少买 2 块电池，最好 3-4 块轮换。

## P1：观察视觉与记录

第一版不靠视觉识别细光纤。摄像头只做远程观察、录像、粗方向提示和调试记录。

| 类别 | 采购项 | 数量 | 推荐型号/规格 | 预算 RMB | 淘宝链接 | 采购建议 |
| --- | --- | ---: | --- | ---: | --- | --- |
| RGB 观察 | USB 摄像头 | 1 | 1080P/2K，手动对焦，低畸变，UVC 标准 | 80-300 | [淘宝搜索](https://s.taobao.com/search?q=USB%E6%91%84%E5%83%8F%E5%A4%B4%201080P%20%E6%89%8B%E5%8A%A8%E5%AF%B9%E7%84%A6%20%E4%BD%8E%E7%95%B8%E5%8F%98) | 第一版可买一只即可 |
| 补光 | LED 灯条/环形灯 | 1 | 5 V/12 V，亮度可调，漫反射罩 | 30-120 | [淘宝搜索](https://s.taobao.com/search?q=USB%20LED%E8%A1%A5%E5%85%89%E7%81%AF%20%E4%BA%AE%E5%BA%A6%E5%8F%AF%E8%B0%83%20%E6%91%84%E5%83%8F%E5%A4%B4) | 用于观察触觉头和线缆进入 V 槽的状态 |
| 深度相机 | Intel RealSense D405 / Orbbec Gemini 2 | 0 | 第二版再考虑 | 0 | [D405 搜索](https://s.taobao.com/search?q=Intel%20RealSense%20D405%20%E6%B7%B1%E5%BA%A6%E7%9B%B8%E6%9C%BA) / [Gemini 2 搜索](https://s.taobao.com/search?q=%E5%A5%A5%E6%AF%94%E4%B8%AD%E5%85%89%20Gemini%202%20%E6%B7%B1%E5%BA%A6%E7%9B%B8%E6%9C%BA) | 第一版暂缓 |

## P1/P2：制造工具、接线工具和耗材

你已经买了 Bambu Lab A1，所以这部分不再采购打印机本体，重点买耗材和维护件。

| 类别 | 采购项 | 数量 | 推荐规格 | 预算 RMB | 淘宝链接 | 带到美国注意 |
| --- | --- | ---: | --- | ---: | --- | --- |
| 打印耗材 | PLA / PLA+ | 3-4 卷 | 黑、白、亮橙/亮黄、灰；1.75mm | 250-600 | [淘宝搜索](https://s.taobao.com/search?q=PLA%2B%201.75mm%20%E8%80%97%E6%9D%90%20%E9%BB%91%20%E7%99%BD%20%E6%A9%99) | 第一版暂不买 PETG/TPU 也可以 |
| 打印配件 | A1 0.4mm 热端、0.6mm 热端、PEI 板 | 1 批 | A1 专用；0.4mm 备用，0.6mm 打大件 | 300-800 | [淘宝搜索](https://s.taobao.com/search?q=Bambu%20A1%200.4mm%20%E7%83%AD%E7%AB%AF%200.6mm%20PEI%E6%9D%BF) | 注意买 A1 兼容，不是 X1/P1 专用 |
| 干燥储存 | 真空袋、干燥剂、湿度卡 | 1 套 | 耗材密封储存 | 80-200 | [淘宝搜索](https://s.taobao.com/search?q=3D%E6%89%93%E5%8D%B0%E8%80%97%E6%9D%90%20%E7%9C%9F%E7%A9%BA%E8%A2%8B%20%E5%B9%B2%E7%87%A5%E5%89%82%20%E6%B9%BF%E5%BA%A6%E5%8D%A1) | 美国也容易买，国内带少量即可 |
| 焊接 | 可调温烙铁/焊台 | 1 | 100-240 V，T12/C245 类烙铁头生态 | 150-500 | [淘宝搜索](https://s.taobao.com/search?q=%E7%83%99%E9%93%81%20100-240V%20%E5%8F%AF%E8%B0%83%E6%B8%A9%20%E7%84%8A%E5%8F%B0) | 必须确认 100-240 V；液体助焊剂建议美国买 |
| 接线/压接 | 压线钳和端子套装 | 1 套 | SN-28B、JST-XH/PH、杜邦、KF301、冷压端子 | 150-300 | [淘宝搜索](https://s.taobao.com/search?q=SN-28B%20%E5%8E%8B%E7%BA%BF%E9%92%B3%20JST%20%E6%9D%9C%E9%82%A6%E7%AB%AF%E5%AD%90%E5%A5%97%E8%A3%85) | 很值得国内买；分盒标注规格 |
| 基础工具 | 万用表、剥线钳、斜口钳、内六角、螺丝刀 | 1 套 | 国产优利德/胜利即可；万用表带蜂鸣和电流档 | 200-500 | [淘宝搜索](https://s.taobao.com/search?q=%E4%B8%87%E7%94%A8%E8%A1%A8%20%E5%89%A5%E7%BA%BF%E9%92%B3%20%E6%96%9C%E5%8F%A3%E9%92%B3%20%E5%86%85%E5%85%AD%E8%A7%92%20%E5%A5%97%E8%A3%85) | 手工具可托运；带电池的工具注意电池规则 |
| 机械耗材 | 螺丝、铜柱、轴承、光轴、联轴器 | 1 批 | M2/M3/M4 螺丝包，铜柱，8 mm 光轴，KFL08/KP08，联轴器 | 200-500 | [淘宝搜索](https://s.taobao.com/search?q=M3%20%E8%9E%BA%E4%B8%9D%E5%8C%85%20%E9%93%9C%E6%9F%B1%20%E8%BD%B4%E6%89%BF%20%E5%85%89%E8%BD%B4%20%E8%81%94%E8%BD%B4%E5%99%A8) | 国内买规格全且便宜 |
| 电气耗材 | 硅胶线、热缩管、扎带、面包板、杜邦线 | 1 批 | 20/22/24 AWG 线，热缩管套装，扎带 | 100-250 | [淘宝搜索](https://s.taobao.com/search?q=%E7%A1%85%E8%83%B6%E7%BA%BF%20%E7%83%AD%E7%BC%A9%E7%AE%A1%20%E6%9D%9C%E9%82%A6%E7%BA%BF%20%E9%9D%A2%E5%8C%85%E6%9D%BF) | 值得国内带 |

## 美国兼容性与运输注意

- 美国常见市电为 120 V / 60 Hz，插头 Type A/B。所有焊台、充电器、电源适配器必须确认 `100-240 V, 50/60 Hz` 或美国电压版本。
- NP-F970 通常低于 100 Wh，航空携带相对简单，但备用锂电池仍应随身携带，不建议托运。保留电池 Wh 标识。
- 胶水、喷剂、清洗剂、助焊剂等液体/易燃品建议到美国购买，不建议从国内带。
- 带到美国后需要继续补货的核心电子件，优先选美国 Amazon、DigiKey、Mouser、Adafruit、SparkFun、RobotShop 也能买到同类替代品的型号。

## 采购批次建议

### 第一批，必须买

- ESP32 DevKitC / ESP32-WROOM-32E：3 块
- 小型 12 V 履带底盘，带编码器：1 套
- TB6612FNG 或 BTS7960 电机驱动：2-3 块
- 弹性探针材料：弹簧钢丝/尼龙扎带/PET 条：1 批
- 触觉传感：滚轮微动开关、A3144 霍尔、小磁铁、少量 FSR：1 批
- 宽口漏斗/V 槽导向轮、软压线轮、弹簧预紧件：1 批
- JGY370 12 V 蜗轮蜗杆减速电机：1 个
- S 型 5 kg 拉力传感器 + HX711：2 套
- 编码器计米轮：1-2 套
- 卷盘、光轴、轴承座、联轴器：1 批
- NP-F970 电池、电池板、5.1V/5A 降压、急停、保险丝、线材：1 批
- 模拟光纤/皮线光缆 50-100 m：1 卷

第一批目标预算：约 2500-4500 RMB。若暂用旧笔记本、不买树莓派，预算更容易压在 5000 RMB 内。

### 第二批，建议买

- USB 摄像头 + LED 补光
- Raspberry Pi 5 8GB + 官方主动散热器
- A1 备用热端、PEI 板、PLA/PLA+ 耗材
- 接线工具、压线钳、端子、机械耗材补充

### 第三批，可选升级

- Intel RealSense D405 或 Orbbec Gemini 2
- 更可靠的软体/柔性触觉手指结构
- 滑动离合器、力矩限制器或电流采样模块
- 3S/4S 动力电池系统，用于更大底盘或更大收卷负载

## 淘宝购物车复核状态

本轮尝试用浏览器打开 `https://cart.taobao.com/cart.htm` 时，浏览器安全策略阻止了对淘宝购物车页面的自动访问。因此目前不能直接读取你的购物车内容，也不能判断购物车里哪些已买、缺哪些、哪些没必要。

可行替代方式：

- 发购物车截图；
- 复制购物车商品标题和规格；
- 导出/整理成文本清单；
- 逐个把商品链接发到项目里，我按本文规格帮你筛。

收到购物车内容后，按下面规则删减：

- 删除：深度相机、复杂机械手、多自由度舵机臂、L298N、大容量无品牌电池、全尺寸底盘、ABS/ASA/PC/PA 耗材。
- 保留：ESP32、带编码器小履带底盘、BTS7960/TB6612、JGY370、S 型 5kg + HX711、计米轮、急停/保险丝、NP-F970 电源板、PLA/PLA+、压线工具和机械耗材。
- 新增：弹性探针材料、滚轮微动开关、霍尔传感器、小磁铁、软压线轮、弹簧套装、V 槽导向轮、5.1V/5A 树莓派降压模块。

## 参考资料

- Sony NP-F970：官方资料列出电压 7.2V、容量 45Wh / 6300mAh。https://www.sony.jp/handycam/products/NP-F970/
- Raspberry Pi 5：官方文档推荐 27W USB-C Power Supply；Pi 5 使用 USB-C，供电需求随外设增加。https://www.raspberrypi.com/documentation/hardware/displays/raspberry-pi-5.html
- Raspberry Pi 27W USB-C Power Supply：官方电源规格为 5.1V/5A，并支持 USB PD。https://www.raspberrypi.com/products/27w-power-supply/
- CAMVATE NP-F970/L-Series Battery Adapter Plate：示例 NP-F970 电池板，提供 12V/2A、8V/3A 和 USB-C PD 输入/输出。https://www.camvate.com/products/camvate-np-f970-l-series-battery-adapter-plate-3592
- Espressif ESP32-DevKitC V4：官方文档说明该板为小型 ESP32 开发板，大部分 I/O 引脚已引出，适合面包板和跳线原型开发。https://docs.espressif.com/projects/esp-dev-kits/en/latest/esp32/esp32-devkitc/user_guide.html
- Espressif ESP32-WROOM-32E：官方资料列出 Wi-Fi、Bluetooth、GPIO、PWM、I2C、SPI、UART、脉冲计数等外设。https://documentation.espressif.com/esp32-wroom-32e_esp32-wroom-32ue_datasheet_en.html
- FAA 电池乘机规则：备用锂电池按 Wh 限制，100 Wh 以下最简单，101-160 Wh 需要航司批准。https://www.faa.gov/hazmat/packsafe/airline-passengers-and-batteries
- TSA 电动工具规则：电动工具和备用锂电池有不同携带要求。https://www.tsa.gov/travel/security-screening/whatcanibring/items/power-tools
