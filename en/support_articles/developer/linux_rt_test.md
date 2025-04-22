# Real-time Performance Test of Raspberry Pi 5 and Intel i5 6300HQ Laptop


Real-time performance is critical when controlling robotic arms in servo mode. In general, UFACTORY robotic arms require that the operating system used by the user achieves a **maximum latency of no more than 500 microseconds** in real-time performance tests.

Since Windows and macOS cannot meet this real-time requirement, **do not use UFACTORY robotic arms in servo mode on Windows or macOS systems**.

Recommended solutions:

* **Raspberry Pi 5** with a Linux system, with or without real-time patches.  
* A computer with an **x86 processor**, running **Linux with real-time patches installed**.




Test command:

```
cyclictest -t1 -p 80 -i 1000 -l 100000 --mlockall

```

## Test Results

### Raspberry Pi 5/Ubuntu 24.04, without RT patch

| Test Run | Min (us) | Act (us) | Avg (us) | Max (us) |
|----------|----------|----------|----------|----------|
| First    | 3        | 6        | 5        | 16       |
| Second   | 2        | 5        | 5        | 86       |
| Third    | 3        | 5        | 5        | 16       |


### Intel i5 6300HQ Laptop/Ubuntu 24.04, without RT patch

| Test Run | Min (us) | Act (us) | Avg (us) | Max (us) |
|----------|----------|----------|----------|----------|
| First    | 2        | 3        | 3        | 96       |
| Second   | 3        | 3        | 3        | 296      |
| Third    | 2        | 3        | 3        | 3769     |


### Intel i5 6300HQ Laptop/Ubuntu 24.04, with RT patch
| Test Run | Min (us) | Act (us) | Avg (us) | Max (us) |
|----------|----------|----------|----------|----------|
| First    | 2        | 2        | 2        | 64       |
| Second   | 2        | 2        | 2        | 69       |
| Third    | 2        | 2        | 2        | 106      |


## Conclusion

Raspberry Pi 5 (Linux Ubuntu 24.04), without using RT patch, also has good real-time performance. The maximum latency in three tests was 86 microseconds, which meets the requirements for controlling the robotic arm in servo mode.

Intel i5 6300HQ (Linux Ubuntu 24.04) without RT patch, the maximum latency in 3 tests was 3769 microseconds, which does not meet the requirements for controlling the robotic arm in servo mode. After using the RT patch, the maximum latency was 106 microseconds, which meets the requirements for controlling the robotic arm in servo mode.