# *STM32*驱动库，函数库，封装库
在使用 Keil 开发*STM32F1*系列单片机时，`.S` 文件（汇编文件）是非常重要的，因为它包含了微控制器的启动代码。具体来说，这些 `.S` 文件包含了硬件初始化、堆栈设置、向量表和中断向量等内容。它们为程序的启动和系统的基本配置提供了必要的基础。



Start目录里只需要添加一个XXXmd.s的启动文件即可

### **1. 启动文件（Startup Code）**

`.S` 文件通常是启动代码文件（如 `startup_stm32f10x.s`），在程序启动时由处理器执行。启动文件完成的任务包括：

- **初始化堆栈指针**：在 *MCU* 启动时，堆栈指针会被设置为预定的值，通常是 RAM 的顶端地址。
- **清零 `.bss` 段**：`.bss` 段用于未初始化的全局变量。启动代码会清零这些变量的内存空间。
- **初始化 `.data` 段**：`.data` 段包含已初始化的全局变量。启动文件会将程序中定义的初始化数据从 Flash 复制到 RAM 中。
- **设置中断向量表**：中断向量表是 MCU 的一部分，存储了中断处理函数的地址。启动文件会将向量表初始化，并使处理器能够跳转到正确的中断处理函数。

### **设置系统时钟**

STM32 的启动文件通常也会包含设置系统时钟的代码。在 STM32F1 系列中，系统时钟的配置很重要，因为它决定了 MCU 运行的速度和外设的时钟源。

### **3. 中断向量表**

`.S` 文件还负责定义 **中断向量表**，它是一个函数指针数组，存储了所有中断的处理函数地址。比如，外部中断、定时器中断等。在启动时，处理器会根据中断源跳转到相应的中断服务例程（ISR）。

### **4. 处理器启动时的默认操作**

启动文件（`.S` 文件）在处理器复位时执行的默认操作包括：

- 设置初始堆栈指针。
- 清零未初始化数据（`.bss`）。
- 初始化已初始化的数据（`.data`）。
- 调用 `main()` 函数，开始应用程序的执行。

没有这些启动文件，微控制器将无法正常启动或执行应用程序代码。

### **5. 适配不同的 STM32 系列**

对于 STM32 系列的不同型号，启动文件也会有所不同。每个型号的 *STM32* 可能有不同的内存布局、外设和中断配置，因此需要特定的启动代码文件。

例如：

- *STM32F1* 的启动文件（通常命名为 `startup_stm32f10x.s`）与 *STM32F4* 或 *STM32L4* 的启动文件会有所不同。
- 每个文件都会根据具体的芯片型号来配置正确的系统时钟、外设、堆栈大小等。
