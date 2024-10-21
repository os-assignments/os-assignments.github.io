---
title: CPU scheduling simulation
archetype: home
weight: 10
---

On this page you find an interactive simulation of the following CPU scheduling
algorithms. 

- First Come First Serve (FCFS)
- Shortest Job First (SJF)
- Preemptive Shortest Job First (PSJF)
- Round Robin (RR) 

You can adjust the arrival times and CPU burst times by hovering over one of the blue
values in the table below and press `-` (decrement) och `+` (increment).

 - When adjusting the arrival times, processes will always need to
arrive in alphabetical order and two processes cannot arrive at the same time. 

When hovering of the last row in the process table, you can chose to delete
this process by pressing the {{% icon trash %}} delete symbol.

{{% elm src="/elm/cpu-scheduling.js" id="elm-cpu-scheduling"%}}