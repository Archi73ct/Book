# Linux Kernel VR
## File operation handler exploits
### POSIX Breaking Things
Interestingly, in the kernel, we cannot `write()` more than 0x7ffff000 bytes in one go.
This is worth noticing, because it does not align with the type of a normal write handler.

This is a normal write handler:
```c
static ssize_t write(struct file *file, const char __user *buffer, size_t count, loff_t *ppos)
```

A `size_t` is a typedef for `unsigned long long`. This does not align, at all.

This probably breaks POSIX.
I do not know the reason for this fuckery.

