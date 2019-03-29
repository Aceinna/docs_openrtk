RTOS
====

.. contents:: Contents
    :local:

The applications for all OpenIMU units use the FreeRTOS Real-Time Operating System (https://www.freertos.org).
FreeRTOS is very widely used, as it is feature-rich, has a small footprint, and can be used in commercial application without
having to expose intellectual property.

FreeRTOS is licensed under the MIT Open Source License (https://www.freertos.org/a00114.html).

The critical feature of FreeRTOS:

*   Scheduling Options

	*   Pre-emptive 
	*   Co-operative 
	*   Round robin with time slicing
..
*   Fast task notifications	
..
*   Configurable & scalable with a	6K to 12K ROM footprint	
..
*   Mutexes & semaphores

	*   Mutexes with priority inheritance
	*   Recursive mutexes
	*   Binary and counting semaphores
..
*   Chip and compiler agnostic	
..
*   Very efficient software timers
..
*   Can be configured to never completely disable interrupts	
..
*   Easy to use API
..
*   Easy to use message passing
