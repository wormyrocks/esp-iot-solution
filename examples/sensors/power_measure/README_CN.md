# 功率测量示例

## 概述

本示例演示了如何使用**BL0937**功率测量芯片来检测电压、电流、有功功率和能耗等电气参数。使用 FreeRTOS 在 **ESP32** 上实现，展示了如何配置**BL0937**功率测量芯片并与开发芯片连接。该示例初始化功率测量系统、获取各种参数并定期记录。

本示例目前支持**BL0937**功率测量芯片，它能够测量以下参数

1. **电压**
2. **电流**
3. **有功功率**
4. **能量**

项目的主要目的是演示如何配置硬件引脚、初始化功率测量系统以及从芯片中获取数据。

## 功能

* 测量**电压**、**电流**、**有功功率**和**能量**。
* 配置**BL0937**功率测量芯片。
* 支持过流、过压和欠压保护。
* 启用能量检测以获得准确读数。
* 每秒定期获取功率读数并记录。

## 硬件要求

本示例使用**BL0937**功率测量芯片。要连接该芯片，须在 ESP32 上配置以下引脚：

| 变量                | GPIO 引脚      | 芯片引脚 |
| ------------------- | -------------- | -------- |
| `BL0937_CF_GPIO`  | `GPIO_NUM_3` | CF 引脚  |
| `BL0937_SEL_GPIO` | `GPIO_NUM_4` | SEL 引脚 |
| `BL0937_CF1_GPIO` | `GPIO_NUM_7` | CF1 引脚 |

确保这些 GPIO 引脚正确连接到硬件设置中**BL0937**芯片上的相应引脚。

## 软件要求

* ESP-IDF
* 用于任务管理的FreeRTOS

## 工作原理

power_measure_example.c文件中的 `app_main()` 函数是应用程序的入口。下面是它的工作原理：

1. **校准因子初始化** ：

* 应用程序使用 [power_measure_get_calibration_factor()](vscode-file://vscode-app/snap/code/176/usr/share/code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html) 函数获取功率测量芯片的默认校准因子。
* 测量时使用默认校准因子 (`DEFAULT_KI`, `DEFAULT_KU`, `DEFAULT_KP`)。

2. **功率测量系统配置** ：

* power_measure_init_config_t 结构配置了芯片的 GPIO 引脚、电阻值和校准因子。
* 设置过流、过压和欠压的特定阈值，并选择是否启用能量检测。

3. **获取测量数据** ：

* 应用程序进入一个循环，每秒重复获取并记录以下参数：
* **电压** ([power_measure_get_voltage](vscode-file://vscode-app/snap/code/176/usr/share/code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html))
* **Current** ([power_measure_get_current](vscode-file://vscode-app/snap/code/176/usr/share/code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html))
* **有功功率** ([power_measure_get_active_power](vscode-file://vscode-app/snap/code/176/usr/share/code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html))
* **Energy** ([power_measure_get_energy](vscode-file://vscode-app/snap/code/176/usr/share/code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html))

4. **错误处理** ：

* 如果获取任何测量值失败，将使用 `ESP_LOGE`记录错误信息。

5. **释放** ：

* 退出循环后，功率测量系统将使用 [power_measure_deinit()](vscode-file://vscode-app/snap/code/176/usr/share/code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html) 去初始化。

## 配置

您可以根据具体设置修改以下参数：

* **GPIO引脚定义**：如果 **CF**、**SEL** 或 **CF1** 使用不同的 GPIO 引脚，请修改相应的 `#define`值。
* **校准系数** ：更新默认校准因子，使其与所连接硬件的规格相匹配，以实现更精确的测量。
* **阈值** ：调整 “过流”、“过压 ”和 “欠压 ”设置，以适应应用的安全限制。

## 日志

应用程序使用 `ESP_LOGI`记录信息日志，使用 `ESP_LOGE`记录错误日志。日志信息将显示在串行监视器或日志控制台中。

日志输出示例：

**I (12345) PowerMeasureExample： 电压：230.15 V**

**I (12346) PowerMeasureExample： 电流：0.45 A**

**I (12347) PowerMeasureExample： 功率： 103.35 W**

**I (12348) PowerMeasureExample： 能量： 0.12 Kw/h**

## 故障排除

1. **初始化失败** ： 如果初始化失败，请确保所有 GPIO 引脚都已正确定义并连接到 **BL0937**芯片。
2. **测量失败** ： 如果测量失败（如电压、电流），请检查 **BL0937** 芯片是否正确供电并与 ESP32 通信。