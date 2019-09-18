---
layout: post
title: Notes about FreeRTOS
---

Take a note of FreeRTOS analysis  
**BOOT**
1. .s file call main in main.c  
2. main do os initialize & device initialize, create tasks, and call vTaskStartScheduler in tasks.c  
3. vTaskStartScheduler call xPortStartScheduler in port.c(hardware specific)  
4. xPortStartScheduler set up timer interrupt(timer irq handler)  
5. timer irq handler call vTaskSwitchContext in tasks.c to select a new task  
6. timer irq handler call portRESTORE_CONTEXT to run the new task
