---
layout: post
title: Linux Perf
---

Take a note of linux perf tool, more details will be added.

# usermode part
```
builtin-record.c
------------------------------------------
\-- cmd_record
|   \-- __cmd_record
|   |  \-- record__open
|   |  |   \-- evsel__open

evsel.c
------------------------------------------
\-- evsel__open_cpu
|   \-- sys_perf_event_open
```

# kernelmode part

