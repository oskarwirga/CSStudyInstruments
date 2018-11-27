## Meta
* Project: nnn
* Current Stage: Bug Hunting
* Brief description: The fastest terminal file manager ever written. 

## Updates


### Week 1

**Current Stage: Bug Hunting**

Searching for a filetype bug within nnn.


### Week 2

**Current Stage: Bug Hunting**

More hurdles were found after a syscall hander for `fstatat64` was not found. 

```
DEBUG:root:Applying return conditions
DEBUG:root:Cleaned up value 0
DEBUG:root:Injecting return value 0
DEBUG:root:Handling syscall
DEBUG:root:Entering getdents64 entry handler
DEBUG:root:Validating integer argument (trace position: 0 execution position: 0)
DEBUG:root:Argument from execution: 4
DEBUG:root:Argument from trace: 4
DEBUG:root:Validating integer argument (trace position: -1 execution position: 2)
DEBUG:root:Argument from execution: 32768
DEBUG:root:Argument from trace: 32768
DEBUG:root:Replaying this system call
DEBUG:root:PID: 13070
DEBUG:root:addr: 4b116c
DEBUG:root:Nooping the current system call in pid: 13070
DEBUG:root:Applying return conditions
DEBUG:root:Cleaned up value 168
DEBUG:root:Injecting return value 168
DEBUG:root:Handling syscall
Traceback (most recent call last):
  File "build/bdist.linux-i686/egg/src/inject.py", line 574, in main
    syscallreplay.entering_syscall)
  File "build/bdist.linux-i686/egg/src/inject.py", line 153, in debug_handle_syscall
    handle_syscall(pid, syscall_id, syscall_object, entering)
  File "build/bdist.linux-i686/egg/src/inject.py", line 385, in handle_syscall
    syscall_object.name))
NotImplementedError: Encountered un-ignored syscall entry with no handler: 300(fstatat64)
Failed to complete trace
Injector for event:rec_pid 428:13041 failed
```

I attempted to fix this issue by enabling the entry in `src/inject.py`:

```
DEBUG:root:Injecting 14962
DEBUG:root:Attached 14962
DEBUG:root:Requesting stop at next system call entry using SIGCONT
DEBUG:root:Second sigcont 14962
DEBUG:root:Entering system call handling loop
DEBUG:root:Handling syscall
Traceback (most recent call last):
  File "build/bdist.linux-i686/egg/src/inject.py", line 574, in main
    syscallreplay.entering_syscall)
  File "build/bdist.linux-i686/egg/src/inject.py", line 153, in debug_handle_syscall
    handle_syscall(pid, syscall_id, syscall_object, entering)
  File "build/bdist.linux-i686/egg/src/inject.py", line 356, in handle_syscall
    (300, True): fstatat64_entry_handler,
NameError: global name 'fstatat64_entry_handler' is not defined
Failed to complete trace
Injector for event:rec_pid 428:13041 failed
```

I then searched up `fstatat64_entry_handler` on github, and found 4 entries (all by pkmoore), one of which was the function in the syscallreplay repo.

### Week 3

**Current Stage: Bug Hunting**

I have hit a dead end with implementing `fstatat64_entry_handler`, the old version is implemented but does NOT work properly. I spoke with Preston about this, simply enabling the entry handler opened up a whole host of problems, such as missing helper functions in `utils.py`.
