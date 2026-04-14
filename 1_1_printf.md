## printf 使用方法

1. 在 CubeMX 中打开对应 USART（例如 USART1），配置为 Asynchronous，并确认引脚与实际硬件接线一致。
2. 如果要在中断或后台输出，保持串口中断可用；如果只是阻塞发送，普通 UART 初始化也可以。
3. 在 [Core/Src/main.c](../../Core/Src/main.c) 的 `USER CODE BEGIN Includes` 中加入 `#include <stdio.h>`。
4. 在 [Core/Src/main.c](../../Core/Src/main.c) 的 `USER CODE BEGIN 0` 中实现 `_write`，内部调用 `HAL_UART_Transmit(...)`，这样 `printf` 才能输出到串口。
5. 使用 `printf` 输出整型、字符串、浮点数时，优先保证格式串与变量类型匹配，避免错误输出。
6. 如果只想拼接字符串后再输出，建议优先用 `snprintf` 先写入缓冲区，再 `printf` 或串口发送。

### 常用示例

```c
printf("Hello, world!\r\n");
printf("CH0=%ld, CH1=%ld\r\n", (long)adc0, (long)adc1);
snprintf(buf, sizeof(buf), "V=%.3fV\r\n", voltage);
printf("%s", buf);
```

### 注意事项

1. `printf` 走串口会阻塞当前线程，频繁打印会影响采样和控制实时性。
2. 浮点格式化通常会增加代码体积，如果只需要整数显示，尽量用整数换算。
3. 若工程里已经有 `_write` 重定向，不要重复实现同名函数。