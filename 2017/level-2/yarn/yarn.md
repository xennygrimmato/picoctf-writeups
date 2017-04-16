# YARN

I was told to use the linux strings command on yarn, but it doesn't work.
Can you help? I lost the flag in the binary somewhere, and would like it back.

Binary: https://webshell2017.picoctf.com/static/9adb6ebf01d8755201564dba69bc1a92/yarn

## Solution

On running `strings -n 2` on the given binary, in the output we get some
interesting text:

```
Sub
mit
_me
_fo
r_I
_am
_th
e_f
lag
;(
```

That's all, we have the flag!
