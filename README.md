https://source.android.com/docs/core/tests/debug/read-bug-reports

https://source.android.com/docs/core/tests/debug/native-memory

https://source.android.com/docs/core/tests/debug

https://source.android.com/docs/core/tests/debug/understanding-logging


what is tombstone logs in android?


ans: /data/tombstones/. The tombstone is a file with extra data about the crashed process. 
In particular, it contains stack traces for all the threads in the crashing process (not just the thread that caught the signal),
a full memory map, and a list of all open file descriptors.



In Android 8.0 and higher, crash_dump32 and crash_dump64 are spawned as needed.
*** *** *** *** *** *** *** *** *** *** *** *** *** *** *** ***
Build fingerprint: 'Android/aosp_angler/angler:7.1.1/NYC/enh12211018:eng/test-keys'
Revision: '0'
ABI: 'arm'
pid: 17946, tid: 17949, name: crasher  >>> crasher <<<
signal 11 (SIGSEGV), code 1 (SEGV_MAPERR), fault addr 0xc
    r0 0000000c  r1 00000000  r2 00000000  r3 00000000
    r4 00000000  r5 0000000c  r6 eccdd920  r7 00000078
    r8 0000461a  r9 ffc78c19  sl ab209441  fp fffff924
    ip ed01b834  sp eccdd800  lr ecfa9a1f  pc ecfd693e  cpsr 600e0030

backtrace:
    #00 pc 0004793e  /system/lib/libc.so (pthread_mutex_lock+1)
    #01 pc 0001aa1b  /system/lib/libc.so (readdir+10)
    #02 pc 00001b91  /system/xbin/crasher (readdir_null+20)
    #03 pc 0000184b  /system/xbin/crasher (do_action+978)
    #04 pc 00001459  /system/xbin/crasher (thread_callback+24)
    #05 pc 00047317  /system/lib/libc.so (_ZL15__pthread_startPv+22)
    #06 pc 0001a7e5  /system/lib/libc.so (__start_thread+34)
Tombstone written to: /data/tombstones/tombstone_06



Getting a stack trace/tombstone from a running process
You can use the debuggerd tool to get a stack dump from a running process. From the command line, invoke debuggerd using a process ID (PID) 
to dump a full tombstone to stdout



Reading tombstones



Tombstone written to: /data/tombstones/tombstone_06
This tells you where debuggerd wrote extra information. debuggerd will keep up to 10 tombstones, cycling through the numbers
00 to 09 and overwriting existing tombstones as necessary.

The tombstone contains the same information as the crash dump, plus a few extras. For example, it includes backtraces for
all threads (not just the crashing thread), the floating point registers, raw stack dumps, and memory dumps around the addresses in
registers. Most usefully it also includes a full memory map (similar to /proc/pid/maps). Here's an annotated example from a 32-bit ARM process crash:


System Logging in the Android Runtime (ART)
There are several available classes that are available for system applications and services:

Class	Purpose
android.telephony.Rlog	Radio logging
android.util.Log	General application logging
android.util.EventLog	System integrator diagnostic event logging
android.util.Slog	Platform framework logging

hough android.util.Log and android.util.Slog use the same log level standards, Slog is an @hide class usable only by the platform. 
The EventLog levels are mapped to the entries in the event.logtags file in /system/etc/event-log-tags.
