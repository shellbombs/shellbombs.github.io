---
layout: post
title: Linux Perf
---

Take a note of linux perf tool, more details will be added.

# usermode part

tools/perf/builtin-record.c 
---------------------------------
```
\-- cmd_record
|   \-- __cmd_record
|   |  \-- record__open
|   |  |   \-- evsel__open
```

tools/lib/perf/evsel.c
---------------------------------
```
\-- evsel__open
|   \-- evsel__open_cpu
|   |   \-- sys_perf_event_open
```

# kernelmode part

kernel/events/core.c
--------------------------------
```
\-- sys_perf_event_open
|   \-- perf_event_alloc
|   |   \-- perf_init_event
|   |   |   \-- perf_try_init_event
|   |   |   |   \-- pmu->event_init
```

# pmu register

kernel/events/core.c
--------------------------------
```
perf_pmu_register(&perf_swevent, "software", PERF_TYPE_SOFTWARE);
perf_pmu_register(&perf_cpu_clock, NULL, -1);
perf_pmu_register(&perf_task_clock, NULL, -1);
perf_tp_register();
```

```
static inline void perf_tp_register(void)
{
	perf_pmu_register(&perf_tracepoint, "tracepoint", PERF_TYPE_TRACEPOINT);
#ifdef CONFIG_KPROBE_EVENTS
	perf_pmu_register(&perf_kprobe, "kprobe", -1);
#endif
#ifdef CONFIG_UPROBE_EVENTS
	perf_pmu_register(&perf_uprobe, "uprobe", -1);
#endif
}
```

# take tracepoint as example

kernel/events/core.c
--------------------------------
```
static struct pmu perf_tracepoint = {
	.task_ctx_nr	= perf_sw_context,

	.event_init	= perf_tp_event_init,
	.add		= perf_trace_add,
	.del		= perf_trace_del,
	.start		= perf_swevent_start,
	.stop		= perf_swevent_stop,
	.read		= perf_swevent_read,
};
```

```
\-- pmu->event_init
|   \-- perf_tp_event_init
|   |   \-- perf_trace_init
```

kernel/trace/trace_event_perf.c
--------------------------------
```
\-- perf_trace_init
|   \-- perf_trace_event_init
|   |   \-- perf_trace_event_reg
|   |   |   \-- tp_event->class->reg
```
tp_event is a **trace_event_call** structure which is defined when we use the **very complicated macro TRACE_EVENT** for specific tracepoint, all **trace_event_call** structures will be linked to a global list **ftrace_events**.

for tracepoint, the **tp_event->class->reg** will be **trace_event_reg**, it will register the perf_event callback **perf_trace_##call** to the specific tracepoint, the callback **perf_trace_##call** which is also define by the **very complicated macro TRACE_EVENT**, will write collected data to the ring buffer
