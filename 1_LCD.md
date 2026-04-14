<h1 align="center">LCDʹ��</h1>

## Ŀ¼

- [Ŀ¼](#Ŀ¼)
- [����](#����)
- [��������](#��������)
  - [��������Ŀ¼](#��������Ŀ¼)
  - [����һ��Ǩ��LCD��TOUCH��SYSTEM�ļ���](#����һǨ��lcdtouchsystem�ļ���)
  - [��������޸�cubemx��FSMC](#������޸�cubemx��fsmc)
  - [���������޸�lcd.c��ֱ�Ӹ��ƴ˹��̵�lcd.c�������ⲽ��](#�������޸�lcdcֱ�Ӹ��ƴ˹��̵�lcdc�������ⲽ)
  - [�����ģ�main.c��������#include "lcd.h"��lcd\_init();��lcd\_clear(WHITE);](#������mainc��������include-lcdhlcd_init��lcd_clearwhite)
- [��ע](#��ע)

## ����

- ������
- Ŀ�꣺
- �����

## ��������

### ��������Ŀ¼

- [Ŀ¼](#Ŀ¼)
- [����](#����)
- [��������](#��������)
  - [��������Ŀ¼](#��������Ŀ¼)
  - [����һ��Ǩ��LCD��TOUCH��SYSTEM�ļ���](#����һǨ��lcdtouchsystem�ļ���)
  - [��������޸�cubemx��FSMC](#������޸�cubemx��fsmc)
  - [���������޸�lcd.c��ֱ�Ӹ��ƴ˹��̵�lcd.c�������ⲽ��](#�������޸�lcdcֱ�Ӹ��ƴ˹��̵�lcdc�������ⲽ)
  - [�����ģ�main.c��������#include "lcd.h"��lcd\_init();��lcd\_clear(WHITE);](#������mainc��������include-lcdhlcd_init��lcd_clearwhite)
- [��ע](#��ע)

### ����һ��Ǩ��LCD��TOUCH��SYSTEM�ļ���
ʹ��keil��Ҫ�����ļ������ﲻд��
ʹ��cmake��Ҫ�޸�CMakeLists.txt�ļ�����45�������ҵ�# Add sources to executable

<img src="./image/1_LCD����ʾ/3_cmake�ļ�.png" alt="����1" width="75%" />

- ����˵����

### ��������޸�cubemx��FSMC

<img src="./image/1_LCD����ʾ/1_FSMC����.png" alt="����2" width="75%" />
<img src="./image/1_LCD����ʾ/2_FSMC����2.png" alt="����2" width="75%" />

- ����˵����

### ���������޸�lcd.c��ֱ�Ӹ��ƴ˹��̵�lcd.c�������ⲽ��
ע�͵�lcd.c��32��#include "./SYSTEM/usart/usart.h"��788��printf��606��HAL_SRAM_MspInit(),ǰ����Ҫռ��uart������ʹ�û��ӡ��Ļ�ͺţ���������cubemx�е�FSMC���ó�ͻ

- ����˵����
- 

### �����ģ�main.c��������#include "lcd.h"��lcd_init();��lcd_clear(WHITE);

## ��ע

- ע�����
- ͼƬ����ͳһ���� `./image/��Ŀ¼/` �£�����ά����
- ����Ļ��ʾ���ݵ���Ҫ������

1. `lcd_init()`����ʼ��LCD������� [Drivers/BSP/LCD/lcd.c](../../Drivers/BSP/LCD/lcd.c)����637�У�
2. `lcd_clear(color)`����������������� [Drivers/BSP/LCD/lcd.c](../../Drivers/BSP/LCD/lcd.c)����855�У�
3. `lcd_draw_point(x, y, color)`�����㣬����� [Drivers/BSP/LCD/lcd.c](../../Drivers/BSP/LCD/lcd.c)����426�У�
4. `lcd_draw_line(x1, y1, x2, y2, color)`����ֱ�ߣ������ [Drivers/BSP/LCD/lcd.c](../../Drivers/BSP/LCD/lcd.c)����927�У�
5. `lcd_draw_hline(x, y, len, color)`����ˮƽ�ߣ������� [Drivers/BSP/LCD/lcd.h](../../Drivers/BSP/LCD/lcd.h)����224�У�
6. `lcd_draw_rectangle(x1, y1, x2, y2, color)`�������Σ������ [Drivers/BSP/LCD/lcd.c](../../Drivers/BSP/LCD/lcd.c)����1018�У�
7. `lcd_draw_circle(x0, y0, r, color)`����Բ������� [Drivers/BSP/LCD/lcd.c](../../Drivers/BSP/LCD/lcd.c)����1033�У�
8. `lcd_fill_circle(x, y, r, color)`����ʵ��Բ������� [Drivers/BSP/LCD/lcd.c](../../Drivers/BSP/LCD/lcd.c)����1074�У�
9. `lcd_fill(sx, sy, ex, ey, color)`����ɫ������䣬������ [Drivers/BSP/LCD/lcd.h](../../Drivers/BSP/LCD/lcd.h)����226�У�
10. `lcd_color_fill(sx, sy, ex, ey, color_buf)`����ɫ������䣬������ [Drivers/BSP/LCD/lcd.h](../../Drivers/BSP/LCD/lcd.h)����227�У�
11. `lcd_show_char(x, y, chr, size, mode, color)`����ʾ���ַ�������� [Drivers/BSP/LCD/lcd.c](../../Drivers/BSP/LCD/lcd.c)����1112�У�
12. `lcd_show_num(x, y, num, len, size, color)`����ʾ���֣������ [Drivers/BSP/LCD/lcd.c](../../Drivers/BSP/LCD/lcd.c)����1207�У�
13. `lcd_show_xnum(x, y, num, len, size, mode, color)`����չ������ʾ�������� [Drivers/BSP/LCD/lcd.h](../../Drivers/BSP/LCD/lcd.h)����233�У�
14. `lcd_show_string(x, y, width, height, size, str, color)`����ʾ�ַ���������� [Drivers/BSP/LCD/lcd.c](../../Drivers/BSP/LCD/lcd.c)����1290�У�
15. `lcd_display_on()` / `lcd_display_off()`��������ʾ�������� [Drivers/BSP/LCD/lcd.h](../../Drivers/BSP/LCD/lcd.h)����210�У�
16. `lcd_display_dir(dir)` / `lcd_scan_dir(dir)`��������ɨ�跽ʽ�������� [Drivers/BSP/LCD/lcd.h](../../Drivers/BSP/LCD/lcd.h)����212�У�

- ������ɫ����ת��CLion���ݣ���

1. [WHITE](../../Drivers/BSP/LCD/lcd.h)����159�У�
2. [BLACK](../../Drivers/BSP/LCD/lcd.h)����160�У�
3. [RED](../../Drivers/BSP/LCD/lcd.h)����161�У�
4. [GREEN](../../Drivers/BSP/LCD/lcd.h)����162�У�
5. [BLUE](../../Drivers/BSP/LCD/lcd.h)����163�У�

