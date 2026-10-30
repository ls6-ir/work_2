# 电控组第二次培训作业

- **张俊宏**
- ***20263008258***

> ##实现思路
>
> 第一步，在 CubeMX 里把 PA0、PA1、PA2 设成 GPIO_Output，并且给它们填了 User Label：LED0、LED1、LED2。这样生成代码后，main.h 里就有了 LED0_Pin 这些宏，写代码时直接调用宏就行，不用记是哪个引脚。
>
> 第二步，我把三个灯的显示状态编成号：0 号是灯 0 亮，1 号是灯 1 亮，2 号是灯 2 亮。用一个变量 led_state 记住现在是几号，再用一个函数 LED_Show 负责把三个灯的电平都设一遍，亮的那一路置高，另外两路置低。一开始我只改了要亮的那一个灯，结果两个灯一起亮，后来才明白三个都得写。
>
> 第三步是控制时间。最开始我用的是 HAL_Delay(500)，但这样程序会卡在那里等，什么都干不了。后来改成用 HAL_GetTick() 记时间：进循环前先记一个 last_time，循环里每次读当前时间 now，算一下 now - last_time 有没有到 500。到了就换下一个状态，并且把 last_time 更新成 now，重新开始计时；没到就直接跳过，继续下一圈。
>
> 换状态我用的是 led_state = (led_state + 1) % 3，2 加 1 变成 3，对 3 取余又回到 0，这样就不用另外写 if 判断了。
>
> ##代码
>
> ```
> uint32_t last_time = 0;  while (1)  {    uint32_t now = HAL_GetTick();        if (now - last_time >= 500)    {      last_time = now;      led_state = (led_state + 1) % 3;      LED_Show(led_state);    }    /* USER CODE END WHILE */    /* USER CODE BEGIN 3 */      }  /* USER CODE END 3 */}
> ```
>
>
>
>



---
