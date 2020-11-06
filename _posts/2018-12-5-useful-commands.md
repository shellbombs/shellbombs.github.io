---
layout: post
title: Useful commands
---

Take a note of some useful commands that are hard to remember!

#Windbg

```
bp kernel32!CreateFileW "as /mu ${/v:filename} poi(@esp+4); .block {.if ($spat(@\"${filename}\", \"*.txt\")) {ad /q *; kb;} .else {ad /q *; gc;}}"
```

break into debugger when the process opens or creates a file with a *.txt* file extension.

#Xperf

because windows performance toolkit are not avaliable on win7, we can use *Xperf* instead.
```
xperf -start -on base -stackwalk profile
```

avaliable *-on* groups:
Base           : PROC_THREAD+LOADER+DISK_IO+HARD_FAULTS+PROFILE+MEMINFO+MEMINFO_WS
Diag           : PROC_THREAD+LOADER+DISK_IO+HARD_FAULTS+DPC+INTERRUPT+CSWITCH+PERF_COUNTER+COMPACT_CSWITCH
DiagEasy       : PROC_THREAD+LOADER+DISK_IO+HARD_FAULTS+DPC+INTERRUPT+CSWITCH+PERF_COUNTER
Latency        : PROC_THREAD+LOADER+DISK_IO+HARD_FAULTS+DPC+INTERRUPT+CSWITCH+PROFILE
FileIO         : PROC_THREAD+LOADER+DISK_IO+HARD_FAULTS+FILE_IO+FILE_IO_INIT
IOTrace        : PROC_THREAD+LOADER+DISK_IO+HARD_FAULTS+CSWITCH
ResumeTrace    : PROC_THREAD+LOADER+DISK_IO+HARD_FAULTS+PROFILE+POWER
SysProf        : PROC_THREAD+LOADER+PROFILE
ResidentSet    : PROC_THREAD+LOADER+DISK_IO+HARD_FAULTS+MEMORY+MEMINFO+VAMAP+SESSION+VIRT_ALLOC
ReferenceSet   : PROC_THREAD+LOADER+HARD_FAULTS+MEMORY+FOOTPRINT+VIRT_ALLOC+MEMINFO+VAMAP+SESSION+REFSET+MEMINFO_WS
Network        : PROC_THREAD+LOADER+NETWORKTRACE

avaliable *-stackwalk* flags:
AlpcClosePort
AlpcConnectFail
AlpcConnectRequest
AlpcConnectSuccess
AlpcReceiveMessage
AlpcSendMessage
AlpcUnwait
AlpcWaitForNewMessage
AlpcWaitForReply
CcCanIWriteFail
CcFlushCache
CcFlushSection
CcLazyWriteScan
CcReadAhead
CcWorkitemComplete
CcWorkitemDequeue
CcWorkitemEnqueue
CcWriteBehind
ContiguousMemoryGeneration
CritSecCollision
CSwitch
DiskFlushInit
DiskReadInit
DiskWriteInit
ExecutiveResource
FileCleanup
FileClose
FileCreate
FileDelete
FileDirEnum
FileDirNotify
FileFlush
FileFSCTL
FileOpEnd
FileQueryInformation
FileRead
FileRename
FileSetInformation
FileWrite
HardFault
HeapAlloc
HeapCreate
HeapDestroy
HeapFree
HeapRangeCreate
HeapRangeDestroy
HeapRangeRelease
HeapRangeReserve
HeapRealloc
ImageLoad
ImageUnload
KernelQueueDequeue
KernelQueueEnqueue
KernelSignal
KernelSignalInit
KernelSync
KernelSyncAll
KernelWaitSync
KernelWaitSyncAll
MapFile
Mark
MiniFilterPostOpInit
MiniFilterPreOpInit
PagefaultAV
PagefaultCopyOnWrite
PagefaultDemandZero
PagefaultGuard
PagefaultHard
PagefaultTransition
PagefileBackedImageMapping
PageRangeAccess
PageRangeRelease
PoolAlloc
PoolAllocSession
PoolFree
PoolFreeSession
PowerDeviceNotify
PowerDeviceNotifyComplete
PowerIdleStateChange
PowerPerfStateChange
PowerPostSleep
PowerPreSleep
PowerSessionCallout
PowerSessionCalloutReturn
PowerSetDevicesState
PowerSetDevicesStateReturn
PowerSetPowerAction
PowerSetPowerActionReturn
PowerThermalConstraint
ProcessCreate
ProcessDelete
Profile
ProfileSetInterval
ReadyThread
RegCloseKey
RegCreateKey
RegDeleteKey
RegDeleteValue
RegEnumerateKey
RegEnumerateValueKey
RegFlush
RegKcbCreate
RegKcbDelete
RegOpenKey
RegQueryKey
RegQueryMultipleValue
RegQueryValue
RegSetInformation
RegSetValue
RegVirtualize
SplitIO
SyscallEnter
SyscallExit
ThreadCreate
ThreadDelete
ThreadPoolCallbackCancel
ThreadPoolCallbackDequeue
ThreadPoolCallbackEnqueue
ThreadPoolCallbackStart
ThreadPoolCallbackStop
ThreadPoolClose
ThreadPoolCreate
ThreadPoolSetMaxThreads
ThreadPoolSetMinThreads
ThreadSetBasePriority
ThreadSetPriority
TimerSetOneShot
TimerSetPeriodic
UnMapFile
VirtualAlloc
VirtualFree
