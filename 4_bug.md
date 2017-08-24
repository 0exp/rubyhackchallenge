# �o�O�̏C��

## ���̎����ɂ���

MRI �̕ύX�̑啔���́A�o�O�̏C���ɂȂ�܂��B�{�e�ł́A�o�O�C�����ǂ̂悤�ɍs���Ă����̂��A������Ă����܂��B

## `Kernel#hello(name)`

### `Kernel#hello(name)` �̎���

�܂��͕��K�ł��B`hello` �Ƃ����֐����ۂ����\�b�h���`���܂��傤�B`p` ���\�b�h�̂悤�� `Kernel` �ɒ�`���܂��B

���̃��\�b�h�́A"Hello #{name}\n" �Ƃ�����������o�͂��܂��B

Ruby �Ŏ�������ƁA����Ȋ����ł��B

```
def hello name
  puts "Hello #{name}"
end

hello 'ko1' #=> "Hello ko1" �Əo��
```

������AC �ŏ��������AMRI �ɖ��ߍ��݂܂��傤�B
`rb_define_global_function()` ���g�����ƂŁA`Kernel#hello` �Ƃ��� private ���\�b�h����邱�Ƃ��ł��܂��B

```
Index: io.c
===================================================================
--- io.c	(���r�W���� 59647)
+++ io.c	(��ƃR�s�[)
@@ -12327,6 +12327,14 @@
     }
 }
 
+static VALUE
+f_hello(VALUE self, VALUE name)
+{
+    const char *name_ptr = RSTRING_PTR(name);
+    fprintf(stdout, "Hello %s\n", name_ptr);
+    return Qnil;
+}
+
 /*
  * Document-class: IOError
  *
@@ -12530,6 +12538,8 @@
     rb_define_global_function("p", rb_f_p, -1);
     rb_define_method(rb_mKernel, "display", rb_obj_display, -1);
 
+    rb_define_global_function("hello", f_hello, 1);
+
     rb_cIO = rb_define_class("IO", rb_cObject);
     rb_include_module(rb_cIO, rb_mEnumerable);
 
```

�|�C���g�� `RSTRING_PTR(name)` �ŁA������I�u�W�F�N�g���� C ������̃|�C���^�𓾂邱�Ƃ��ł��܂��B

`test.rb` �ɃT���v���R�[�h���L�q���A`$ make run` ��p���Ď��s���Ă݂܂��傤�B�����Ɠ����܂������H�@�����A�����Ă�񂶂�Ȃ����Ǝv���܂��B

### �o�O��

`hello()` ���\�b�h���܂߂� Ruby �̍ŐV�o�[�W�����������[�X���ꂽ�ƍl���Ă��������B���E���ő�l�C�ɂȂ�A�����̃��[�U�[�� `hello()` ���g�����Ƃ��܂��B
�����̃��[�U�[���g���Ă���ƁA�s�����������̂ŁA����� Redmine �̂ق��ɁA���̂悤�ȃo�O�񍐂����܂����B

```
My script causes SEGV.

See attached log for details.
```

�Y�t����Ă������O�ɂ͎��̂悤�ɏ�����Ă��܂����B

```
../../trunk/test.rb:2: [BUG] Segmentation fault at 0x0000000000000008
ruby 2.5.0dev (2017-08-23 trunk 59647) [x86_64-linux]

-- Control frame information -----------------------------------------------
c:0003 p:---- s:0011 e:000010 CFUNC  :hello
c:0002 p:0007 s:0006 e:000005 EVAL   ../../trunk/test.rb:2 [FINISH]
c:0001 p:0000 s:0003 E:000b00 (none) [FINISH]

-- Ruby level backtrace information ----------------------------------------
../../trunk/test.rb:2:in `<main>'
../../trunk/test.rb:2:in `hello'

-- Machine register context ------------------------------------------------
 RIP: 0x00000000004c17f4 RBP: 0x0000000000df5430 RSP: 0x00007fff031d4680
 RAX: 0x0000000000000000 RBX: 0x00002ba4beccefb0 RCX: 0x00002ba4bebcf048
 RDX: 0x00000000004c17f0 RDI: 0x0000000000e562d0 RSI: 0x0000000000000008
  R8: 0x00002ba4bebcf068  R9: 0x00002ba4beccef80 R10: 0x0000000000000000
 R11: 0x0000000000000001 R12: 0x00002ba4beccefb0 R13: 0x0000000000e1d4f8
 R14: 0x0000000000ec78f0 R15: 0x0000000000e562d0 EFL: 0x0000000000010202

-- C level backtrace information -------------------------------------------
/mnt/sdb1/ruby/build/trunk/miniruby(rb_vm_bugreport+0x528) [0x61a088] ../../trunk/vm_dump.c:671
/mnt/sdb1/ruby/build/trunk/miniruby(rb_bug_context+0xd0) [0x4939c0] ../../trunk/error.c:539
/mnt/sdb1/ruby/build/trunk/miniruby(sigsegv+0x42) [0x58a622] ../../trunk/signal.c:932
/lib/x86_64-linux-gnu/libpthread.so.0 [0x2ba4bedc7330]
/mnt/sdb1/ruby/build/trunk/miniruby(f_hello+0x4) [0x4c17f4] ../../trunk/io.c:12332
/mnt/sdb1/ruby/build/trunk/miniruby(vm_call_cfunc+0x12d) [0x5fbe6d] ../../trunk/vm_insnhelper.c:1903
/mnt/sdb1/ruby/build/trunk/miniruby(vm_call_method+0xf7) [0x60d417] ../../trunk/vm_insnhelper.c:2364
/mnt/sdb1/ruby/build/trunk/miniruby(vm_exec_core+0x2051) [0x607a01] ../../trunk/insns.def:854
/mnt/sdb1/ruby/build/trunk/miniruby(vm_exec+0x98) [0x60b7f8] ../../trunk/vm.c:1793
/mnt/sdb1/ruby/build/trunk/miniruby(ruby_exec_internal+0xb2) [0x499a02] ../../trunk/eval.c:246
/mnt/sdb1/ruby/build/trunk/miniruby(ruby_exec_node+0x1d) [0x49b78d] ../../trunk/eval.c:310
/mnt/sdb1/ruby/build/trunk/miniruby(ruby_run_node+0x1c) [0x49dffc] ../../trunk/eval.c:302
/mnt/sdb1/ruby/build/trunk/miniruby(main+0x5f) [0x41a61f] ../../trunk/main.c:42

-- Other runtime information -----------------------------------------------

* Loaded script: ../../trunk/test.rb

* Loaded features:

    0 enumerator.so
    1 thread.rb
    2 rational.so
    3 complex.so

* Process memory map:

00400000-006f7000 r-xp 00000000 08:11 262436                             /mnt/sdb1/ruby/build/trunk/miniruby
008f6000-008fb000 r--p 002f6000 08:11 262436                             /mnt/sdb1/ruby/build/trunk/miniruby
008fb000-008fc000 rw-p 002fb000 08:11 262436                             /mnt/sdb1/ruby/build/trunk/miniruby
008fc000-0090e000 rw-p 00000000 00:00 0
00df4000-00f2c000 rw-p 00000000 00:00 0                                  [heap]
2ba4beb92000-2ba4bebb5000 r-xp 00000000 08:01 310550                     /lib/x86_64-linux-gnu/ld-2.19.so
2ba4bebb5000-2ba4bebb7000 rw-p 00000000 00:00 0
2ba4bebb7000-2ba4bebb8000 ---p 00000000 00:00 0
2ba4bebb8000-2ba4bebbb000 rw-p 00000000 00:00 0                          [stack:13848]
2ba4bebbb000-2ba4bebc2000 r--s 00000000 08:01 216019                     /usr/lib/x86_64-linux-gnu/gconv/gconv-modules.cache
2ba4bebc2000-2ba4bebc3000 rw-p 00000000 00:00 0
2ba4bebcb000-2ba4becd0000 rw-p 00000000 00:00 0
2ba4becd0000-2ba4becf3000 r--s 00000000 08:01 309830                     /lib/x86_64-linux-gnu/libpthread-2.19.so
2ba4becf3000-2ba4bed9a000 r--s 00000000 08:01 6641                       /usr/lib/debug/lib/x86_64-linux-gnu/libpthread-2.19.so
2ba4bedb4000-2ba4bedb5000 r--p 00022000 08:01 310550                     /lib/x86_64-linux-gnu/ld-2.19.so
2ba4bedb5000-2ba4bedb6000 rw-p 00023000 08:01 310550                     /lib/x86_64-linux-gnu/ld-2.19.so
2ba4bedb6000-2ba4bedb7000 rw-p 00000000 00:00 0
2ba4bedb7000-2ba4bedd0000 r-xp 00000000 08:01 309830                     /lib/x86_64-linux-gnu/libpthread-2.19.so
2ba4bedd0000-2ba4befcf000 ---p 00019000 08:01 309830                     /lib/x86_64-linux-gnu/libpthread-2.19.so
2ba4befcf000-2ba4befd0000 r--p 00018000 08:01 309830                     /lib/x86_64-linux-gnu/libpthread-2.19.so
2ba4befd0000-2ba4befd1000 rw-p 00019000 08:01 309830                     /lib/x86_64-linux-gnu/libpthread-2.19.so
2ba4befd1000-2ba4befd5000 rw-p 00000000 00:00 0
2ba4befd5000-2ba4befd8000 r-xp 00000000 08:01 309835                     /lib/x86_64-linux-gnu/libdl-2.19.so
2ba4befd8000-2ba4bf1d7000 ---p 00003000 08:01 309835                     /lib/x86_64-linux-gnu/libdl-2.19.so
2ba4bf1d7000-2ba4bf1d8000 r--p 00002000 08:01 309835                     /lib/x86_64-linux-gnu/libdl-2.19.so
2ba4bf1d8000-2ba4bf1d9000 rw-p 00003000 08:01 309835                     /lib/x86_64-linux-gnu/libdl-2.19.so
2ba4bf1d9000-2ba4bf1e2000 r-xp 00000000 08:01 309837                     /lib/x86_64-linux-gnu/libcrypt-2.19.so
2ba4bf1e2000-2ba4bf3e2000 ---p 00009000 08:01 309837                     /lib/x86_64-linux-gnu/libcrypt-2.19.so
2ba4bf3e2000-2ba4bf3e3000 r--p 00009000 08:01 309837                     /lib/x86_64-linux-gnu/libcrypt-2.19.so
2ba4bf3e3000-2ba4bf3e4000 rw-p 0000a000 08:01 309837                     /lib/x86_64-linux-gnu/libcrypt-2.19.so
2ba4bf3e4000-2ba4bf412000 rw-p 00000000 00:00 0
2ba4bf412000-2ba4bf517000 r-xp 00000000 08:01 309816                     /lib/x86_64-linux-gnu/libm-2.19.so
2ba4bf517000-2ba4bf716000 ---p 00105000 08:01 309816                     /lib/x86_64-linux-gnu/libm-2.19.so
2ba4bf716000-2ba4bf717000 r--p 00104000 08:01 309816                     /lib/x86_64-linux-gnu/libm-2.19.so
2ba4bf717000-2ba4bf718000 rw-p 00105000 08:01 309816                     /lib/x86_64-linux-gnu/libm-2.19.so
2ba4bf718000-2ba4bf8d6000 r-xp 00000000 08:01 309818                     /lib/x86_64-linux-gnu/libc-2.19.so
2ba4bf8d6000-2ba4bfad6000 ---p 001be000 08:01 309818                     /lib/x86_64-linux-gnu/libc-2.19.so
2ba4bfad6000-2ba4bfada000 r--p 001be000 08:01 309818                     /lib/x86_64-linux-gnu/libc-2.19.so
2ba4bfada000-2ba4bfadc000 rw-p 001c2000 08:01 309818                     /lib/x86_64-linux-gnu/libc-2.19.so
2ba4bfadc000-2ba4bfae1000 rw-p 00000000 00:00 0
2ba4bfae1000-2ba4bfdaa000 r--p 00000000 08:01 376                        /usr/lib/locale/locale-archive
2ba4bfdaa000-2ba4bfdc0000 r-xp 00000000 08:01 266058                     /lib/x86_64-linux-gnu/libgcc_s.so.1
2ba4bfdc0000-2ba4bffbf000 ---p 00016000 08:01 266058                     /lib/x86_64-linux-gnu/libgcc_s.so.1
2ba4bffbf000-2ba4bffc0000 rw-p 00015000 08:01 266058                     /lib/x86_64-linux-gnu/libgcc_s.so.1
2ba4bffc0000-2ba4c0f1c000 r--s 00000000 08:11 262436                     /mnt/sdb1/ruby/build/trunk/miniruby
2ba4c0f1c000-2ba4c10e2000 r--s 00000000 08:01 309818                     /lib/x86_64-linux-gnu/libc-2.19.so
7fff031b5000-7fff031d6000 rw-p 00000000 00:00 0
7fff031e5000-7fff031e7000 r-xp 00000000 00:00 0                          [vdso]
ffffffffff600000-ffffffffff601000 r-xp 00000000 00:00 0                  [vsyscall]


[NOTE]
You may have encountered a bug in the Ruby interpreter or extension libraries.
Bug reports are welcome.
For details: http://www.ruby-lang.org/bugreport.html

make: *** [run] Aborted (core dumped)
```

���̃o�O�񍐂ɂ͎��̓_�������Ă��܂��B

* �Č��R�[�h
* ���s��

�����A�Y�t���ꂽ���O�t�@�C��������ƁA`ruby 2.5.0dev (2017-08-23 trunk 59647) [x86_64-linux]` �Ə����Ă���̂ŁALinux ���� Ruby 2.5.0dev �i�J���Łj���g���Ă���̂��ȁA�Ƃ������Ƃ��킩��܂��B

�Č��R�[�h���Ȃ��̂͂悭�Ȃ��̂ŁA�u�Č��R�[�h�����������v�ƕԎ������邱�ɂ��܂����B
�i���̓Y�t�̃��O��ǂނƁA���͎肪���肪�������񂠂�̂ŁA���̒��x�������炷���Ɏ���̂ł����A����͂킩��Ȃ������Ƃ��܂��j

```
Please send us your reproducible code. Small code is awesome.
```

�Г��� Rails �A�v���P�[�V�����Ȃǂ��ƁA���̂܂ܑ���킯�ɂ͂����Ȃ��̂ŁA�܂Ƃ߂đ��邱�Ƃ͂ł��܂���B
�܂��A�u���X������v�Ƃ��������́A�Č��R�[�h�����͓̂���ł��B

������A���͑傫�ȃA�v���P�[�V�����̈ꕔ�������Ɖ��肵�A���肩����uSorry we can't make such repro�v�Ƃ������Ԏ��������Ƃ��܂��B

�����ŁA�f�o�b�O���J�n���邱�Ƃɂ��܂����B

### `[BUG]` �̌���

`[BUG]` �́AMRI �ɉ�����肪�N�������Ƃ��ɐ����܂��B��{�I�ɂ́A�C���^�v���^�̃o�O�ɂȂ�܂��B

```
../../trunk/test.rb:2: [BUG] Segmentation fault at 0x0000000000000008
```

�܂��A�s���ł����A����� `../../trunk/test.rb:2` �Ƃ����ꏊ�����s���ɉ�����肪�������A�Ƃ������Ƃ������Ă��܂��B
���ɁA`Segmentation fault at 0x0000000000000008` �́A`[BUG]` �̌����ł��B���̏ꍇ�A0x0000000000000008 �Ԓn�ւ̃������̓ǂݏ����ŁASegmentation fault �����������A�Ƃ����Ӗ��ɂȂ�܂��B��ʓI�ɂ́A�ǂݏ����֎~�̈�ɑ΂���ǂݏ����ɂ����Đ����܂��B�u�����ԁv�Ƃ��A�u�����ӂ��v�Ƃ����������̂ŁAC �v���O�����Ńo�O������ƁA��r�I�������邱�Ƃ��ł��܂��B

���� `ruby 2.5.0dev (2017-08-23 trunk 59647) [x86_64-linux]` �ŁA`ruby -v` �œ�����o�[�W�����ԍ��i����ю��s���j�������Ă���܂��B

```
-- Control frame information -----------------------------------------------
c:0003 p:---- s:0011 e:000010 CFUNC  :hello
c:0002 p:0007 s:0006 e:000005 EVAL   ../../trunk/test.rb:2 [FINISH]
c:0001 p:0000 s:0003 E:000b00 (none) [FINISH]
```

���̃u���b�N�ł́A�uControl frame information�v�Ə����Ă���Ƃ���ARuby �� VM �̐���t���[����񂪋L�q����Ă��܂��B
�����ŕ\���������e�́AVM �̓����\���ɋ����ˑ����邽�߁AVM �f�o�b�O�ȊO�ł͎g���܂��񂪁A�e�s�ɂ͎��̓��e���܂܂�Ă��܂��B

* `c`: �t���[���ԍ��icf �C���f�b�N�X�j
* `p`: �v���O�����J�E���^
* `s`: �X�^�b�N�̐[��
* `e`: ���|�C���^�iep�j�̒l�i�X�^�b�N�̐[���A�������� heap �Ɋm�ۂ������̃A�h���X�j
* �t���[���^�C�v�B`EVAL` �� `eval` �Őς񂾃t���[���A`CFUNC` �� C �Ŏ������ꂽ���\�b�h
* �t���[���̏ꏊ�BRuby ���x���Ȃ�t�@�C�����ƍs�ԍ��AC �֐��Ȃ烁�\�b�h��

�Ō�̏ꏊ��������ƁA�����镁�ʂ̃o�b�N�g���[�X���Ƃ��ė��p�ł��܂��B

```
-- Ruby level backtrace information ----------------------------------------
../../trunk/test.rb:2:in `<main>'
../../trunk/test.rb:2:in `hello'
```

���̂��̃u���b�N�́A�uRuby level backtrace information�v�A�܂� Ruby �Œʏ퓾�邱�Ƃ��ł���o�b�N�g���[�X���ł��B

```
-- Machine register context ------------------------------------------------
 RIP: 0x00000000004c17f4 RBP: 0x0000000000df5430 RSP: 0x00007fff031d4680
 RAX: 0x0000000000000000 RBX: 0x00002ba4beccefb0 RCX: 0x00002ba4bebcf048
 RDX: 0x00000000004c17f0 RDI: 0x0000000000e562d0 RSI: 0x0000000000000008
  R8: 0x00002ba4bebcf068  R9: 0x00002ba4beccef80 R10: 0x0000000000000000
 R11: 0x0000000000000001 R12: 0x00002ba4beccefb0 R13: 0x0000000000e1d4f8
 R14: 0x0000000000ec78f0 R15: 0x0000000000e562d0 EFL: 0x0000000000010202
```

���̂����肩����Ɉˑ������b�ɂȂ��Ă��܂����A�uMachine register context�v�܂� CPU ���W�X�^�̏��ł��B

```
-- C level backtrace information -------------------------------------------
/mnt/sdb1/ruby/build/trunk/miniruby(rb_vm_bugreport+0x528) [0x61a088] ../../trunk/vm_dump.c:671
/mnt/sdb1/ruby/build/trunk/miniruby(rb_bug_context+0xd0) [0x4939c0] ../../trunk/error.c:539
/mnt/sdb1/ruby/build/trunk/miniruby(sigsegv+0x42) [0x58a622] ../../trunk/signal.c:932
/lib/x86_64-linux-gnu/libpthread.so.0 [0x2ba4bedc7330]
/mnt/sdb1/ruby/build/trunk/miniruby(f_hello+0x4) [0x4c17f4] ../../trunk/io.c:12332
/mnt/sdb1/ruby/build/trunk/miniruby(vm_call_cfunc+0x12d) [0x5fbe6d] ../../trunk/vm_insnhelper.c:1903
/mnt/sdb1/ruby/build/trunk/miniruby(vm_call_method+0xf7) [0x60d417] ../../trunk/vm_insnhelper.c:2364
/mnt/sdb1/ruby/build/trunk/miniruby(vm_exec_core+0x2051) [0x607a01] ../../trunk/insns.def:854
/mnt/sdb1/ruby/build/trunk/miniruby(vm_exec+0x98) [0x60b7f8] ../../trunk/vm.c:1793
/mnt/sdb1/ruby/build/trunk/miniruby(ruby_exec_internal+0xb2) [0x499a02] ../../trunk/eval.c:246
/mnt/sdb1/ruby/build/trunk/miniruby(ruby_exec_node+0x1d) [0x49b78d] ../../trunk/eval.c:310
/mnt/sdb1/ruby/build/trunk/miniruby(ruby_run_node+0x1c) [0x49dffc] ../../trunk/eval.c:302
/mnt/sdb1/ruby/build/trunk/miniruby(main+0x5f) [0x41a61f] ../../trunk/main.c:42
```

�́AC ���x���ł̃o�b�N�g���[�X�ł��BOS �Ȃǂɂ���āA�Ƃꂽ����Ȃ�������A�ʂ̃t�@�C���ɕۑ�����Ă��邱�Ƃ��������b�Z�[�W��\�����邱�Ƃ�����܂��B

```
* Loaded script: ../../trunk/test.rb
```

�ł́A�ǂ̃t�@�C���� ruby �R�}���h�ɓn��������������Ă��܂��B

```
* Loaded features:

    0 enumerator.so
    1 thread.rb
    2 rational.so
    3 complex.so
```

�ł́A�ǂ̃t�@�C���� `require` �ȂǂŃ��[�h���Ă��邩�������Ă��܂��i`$LOADED_FEATURES` �̓��e�ł��j�B

```
* Process memory map:

00400000-006f7000 r-xp 00000000 08:11 262436                             /mnt/sdb1/ruby/build/trunk/miniruby
008f6000-008fb000 r--p 002f6000 08:11 262436                             /mnt/sdb1/ruby/build/trunk/miniruby
008fb000-008fc000 rw-p 002fb000 08:11 262436                             /mnt/sdb1/ruby/build/trunk/miniruby
008fc000-0090e000 rw-p 00000000 00:00 0
00df4000-00f2c000 rw-p 00000000 00:00 0                                  [heap]
2ba4beb92000-2ba4bebb5000 r-xp 00000000 08:01 310550                     /lib/x86_64-linux-gnu/ld-2.19.so
2ba4bebb5000-2ba4bebb7000 rw-p 00000000 00:00 0
2ba4bebb7000-2ba4bebb8000 ---p 00000000 00:00 0
2ba4bebb8000-2ba4bebbb000 rw-p 00000000 00:00 0                          [stack:13848]
2ba4bebbb000-2ba4bebc2000 r--s 00000000 08:01 216019                     /usr/lib/x86_64-linux-gnu/gconv/gconv-modules.cache
2ba4bebc2000-2ba4bebc3000 rw-p 00000000 00:00 0
2ba4bebcb000-2ba4becd0000 rw-p 00000000 00:00 0
2ba4becd0000-2ba4becf3000 r--s 00000000 08:01 309830                     /lib/x86_64-linux-gnu/libpthread-2.19.so
2ba4becf3000-2ba4bed9a000 r--s 00000000 08:01 6641                       /usr/lib/debug/lib/x86_64-linux-gnu/libpthread-2.19.so
2ba4bedb4000-2ba4bedb5000 r--p 00022000 08:01 310550                     /lib/x86_64-linux-gnu/ld-2.19.so
2ba4bedb5000-2ba4bedb6000 rw-p 00023000 08:01 310550                     /lib/x86_64-linux-gnu/ld-2.19.so
2ba4bedb6000-2ba4bedb7000 rw-p 00000000 00:00 0
2ba4bedb7000-2ba4bedd0000 r-xp 00000000 08:01 309830                     /lib/x86_64-linux-gnu/libpthread-2.19.so
2ba4bedd0000-2ba4befcf000 ---p 00019000 08:01 309830                     /lib/x86_64-linux-gnu/libpthread-2.19.so
2ba4befcf000-2ba4befd0000 r--p 00018000 08:01 309830                     /lib/x86_64-linux-gnu/libpthread-2.19.so
2ba4befd0000-2ba4befd1000 rw-p 00019000 08:01 309830                     /lib/x86_64-linux-gnu/libpthread-2.19.so
2ba4befd1000-2ba4befd5000 rw-p 00000000 00:00 0
2ba4befd5000-2ba4befd8000 r-xp 00000000 08:01 309835                     /lib/x86_64-linux-gnu/libdl-2.19.so
2ba4befd8000-2ba4bf1d7000 ---p 00003000 08:01 309835                     /lib/x86_64-linux-gnu/libdl-2.19.so
2ba4bf1d7000-2ba4bf1d8000 r--p 00002000 08:01 309835                     /lib/x86_64-linux-gnu/libdl-2.19.so
2ba4bf1d8000-2ba4bf1d9000 rw-p 00003000 08:01 309835                     /lib/x86_64-linux-gnu/libdl-2.19.so
2ba4bf1d9000-2ba4bf1e2000 r-xp 00000000 08:01 309837                     /lib/x86_64-linux-gnu/libcrypt-2.19.so
2ba4bf1e2000-2ba4bf3e2000 ---p 00009000 08:01 309837                     /lib/x86_64-linux-gnu/libcrypt-2.19.so
2ba4bf3e2000-2ba4bf3e3000 r--p 00009000 08:01 309837                     /lib/x86_64-linux-gnu/libcrypt-2.19.so
2ba4bf3e3000-2ba4bf3e4000 rw-p 0000a000 08:01 309837                     /lib/x86_64-linux-gnu/libcrypt-2.19.so
2ba4bf3e4000-2ba4bf412000 rw-p 00000000 00:00 0
2ba4bf412000-2ba4bf517000 r-xp 00000000 08:01 309816                     /lib/x86_64-linux-gnu/libm-2.19.so
2ba4bf517000-2ba4bf716000 ---p 00105000 08:01 309816                     /lib/x86_64-linux-gnu/libm-2.19.so
2ba4bf716000-2ba4bf717000 r--p 00104000 08:01 309816                     /lib/x86_64-linux-gnu/libm-2.19.so
2ba4bf717000-2ba4bf718000 rw-p 00105000 08:01 309816                     /lib/x86_64-linux-gnu/libm-2.19.so
2ba4bf718000-2ba4bf8d6000 r-xp 00000000 08:01 309818                     /lib/x86_64-linux-gnu/libc-2.19.so
2ba4bf8d6000-2ba4bfad6000 ---p 001be000 08:01 309818                     /lib/x86_64-linux-gnu/libc-2.19.so
2ba4bfad6000-2ba4bfada000 r--p 001be000 08:01 309818                     /lib/x86_64-linux-gnu/libc-2.19.so
2ba4bfada000-2ba4bfadc000 rw-p 001c2000 08:01 309818                     /lib/x86_64-linux-gnu/libc-2.19.so
2ba4bfadc000-2ba4bfae1000 rw-p 00000000 00:00 0
2ba4bfae1000-2ba4bfdaa000 r--p 00000000 08:01 376                        /usr/lib/locale/locale-archive
2ba4bfdaa000-2ba4bfdc0000 r-xp 00000000 08:01 266058                     /lib/x86_64-linux-gnu/libgcc_s.so.1
2ba4bfdc0000-2ba4bffbf000 ---p 00016000 08:01 266058                     /lib/x86_64-linux-gnu/libgcc_s.so.1
2ba4bffbf000-2ba4bffc0000 rw-p 00015000 08:01 266058                     /lib/x86_64-linux-gnu/libgcc_s.so.1
2ba4bffc0000-2ba4c0f1c000 r--s 00000000 08:11 262436                     /mnt/sdb1/ruby/build/trunk/miniruby
2ba4c0f1c000-2ba4c10e2000 r--s 00000000 08:01 309818                     /lib/x86_64-linux-gnu/libc-2.19.so
7fff031b5000-7fff031d6000 rw-p 00000000 00:00 0
7fff031e5000-7fff031e7000 r-xp 00000000 00:00 0                          [vdso]
ffffffffff600000-ffffffffff601000 r-xp 00000000 00:00 0                  [vsyscall]
```

����́A���� Linux ��������Ȃ����Ǝv���܂����AOS ���Ǘ�����v���Z�X�̃������}�b�v�������Ă��܂��B`/proc/self/maps` �ŏo�Ă�����e�ł��B

�f�o�b�O����Ƃ��A���ɒ��ڂ���ׂ��̓o�b�N�g���[�X���ł��B�ǂ����AControl frame information �ɂ��ƁA`hello` �ŃG���[���N�����Ă��邱�Ƃ��킩��܂��B

�����ŁA`hello` �̎������A������x�����ƌ������Ă݂܂��傤�B

NOTE: ���ӏ����o�b�N�g���[�X�Ɍ����o�O�́A��r�I�ȒP�ȃo�O�ł��B����o�O�ɂȂ�ƁA�Ⴆ�΃v���O�����̊֌W�Ȃ��ӏ��Ńf�[�^�����Ă���A��ɂ��̏����Q�Ƃ����ӏ��� `[BUG]` �ɂȂ�ƁA�����̉ӏ����킩��Ȃ��A�Ƃ������Ƃ��悭�N���܂��B

### `f_hello()` �֐��̌�����

`hello` ���\�b�h�̎��͎̂��� C �֐��ł����B

```
static VALUE
f_hello(VALUE self, VALUE name)
{
    const char *name_ptr = RSTRING_PTR(name);
    fprintf(stdout, "Hello %s\n", name_ptr);
    return Qnil;
}
```

�����ƌ��܂��ƁA`name` �œn���Ă��������ɑ΂��� `RSTRING_PTR()` ���g���Ă��܂��B
`RSTRING_PTR()` �́A������I�u�W�F�N�g�i`T_STRING` �ƌ^�t�����ꂽ�I�u�W�F�N�g�j�ɂ̂ݗL���ȃ}�N���ł���A���̑��̃I�u�W�F�N�g�ɂ͑Ή����܂���i�����N���邩�ۏ؂���Ă��܂���j�B
�����A���ꂪ�����Ȃ񂶂�Ȃ��ł��傤���B

NOTE: �u�����ƌ���v���ƂŖ�肪�킩�邩�́AMRI �̓����\�����ǂ̒��x�c�����Ă��邩�ɂ��܂��B����͋K�͂����������ߊȒP�ł����A�ʏ�͂����Ɠ���ł��B

�ł́A���������؂��邽�߂ɁA`hello(nil)` �Ƃł����Ă݂܂��傤�B���l�� `[BUG]` ���o�͂��ꂽ�̂ł͂Ȃ����Ǝv���܂��B

�����ŁA�`�P�b�g�ɍČ��R�[�h�𓊍e���Ă����܂��傤�B

```
The following code can reproduce this issue:

  hello(nil)
```

���ꂾ�������ȍČ��R�[�h������΁A���Ƃ͓��ӂȐl�ɔC���Ă����Ȃ��Ǝv���܂��B����́A�p�b�`�̍쐬�܂ōs���܂��傤�B

### gdb ��p�����f�o�b�O

`test.rb` �� `hello(nil)` �ƋL�����A`make gdb` �Ǝ��s���܂��傤�B

```
Program received signal SIGSEGV, Segmentation fault.
f_hello (self=9904840, name=8) at ../../trunk/io.c:12333
12333       const char *name_ptr = RSTRING_PTR(name);
```

�Əo�͂����ΐ����ł��B����́A`SEGV` �V�O�i�������������߁A`gdb` ���f�o�b�O�Ώۃv���O�������ꎞ��~�����A�Ƃ����Ӗ��ł��B

������ƁA`name` �ɉ��������Ă��邩�m�F���Ă݂܂��傤�B

```
(gdb) p name
$1 = 8
(gdb) rp name
nil
```

`p name` �ɂ���āA`name` �̒l�i���l�j�� 8 �ł��邱�Ƃ��킩��܂��B���A8 �������Ƃ悭�킩��܂���B
`rp name` �ɂ���āA���� 8 �Ƃ����l�� `nil` �ł��邱�Ƃ��킩��܂����B

SEGV �ɂ���Ē�~�����ꏊ�� io.c:12333 �ł���A`const char *name_ptr = RSTRING_PTR(name);` �Ƃ����s�ŋN�����Ă��邱�Ƃ��킩��܂��B
�ǂ����A�u`RSTRING_PTR()`����肾�v�Ƃ��������͐����������悤�ł��B

����I�u�W�F�N�g���� C �̕�����|�C���^���Ƃ�ɂ́A`StringValueCStr()` ���g���܂��B�K�v�Ȃ� `to_s` �����s���A������ɕϊ����Ă��� C �̕�����|�C���^�ɕϊ����܂��B
��������Ă݂܂��傤�B

```
  const char *name_ptr = StringValueCStr(name);
```

���̂悤�ɏC�����A������x `$ make gdb` �����s���Ă݂܂��傤�B

```
ko1@u64:~/ruby/build/trunk$ make gdb
compiling ../../trunk/io.c
linking miniruby
gdb -x run.gdb --quiet --args ./miniruby -I../../trunk/lib -I. -I.ext/common  ../../trunk/test.rb
Reading symbols from ./miniruby...done.
Breakpoint 1 at 0x474900: file ../../trunk/debug.c, line 127.
warning: ../../trunk/breakpoints.gdb: No such file or directory
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
[New Thread 0x2aaaaaad5700 (LWP 17714)]
Traceback (most recent call last):
        1: from ../../trunk/test.rb:2:in `<main>'
../../trunk/test.rb:2:in `hello': no implicit conversion of nil into String (TypeError)
[Thread 0x2aaaaaad5700 (LWP 17714) exited]
[Inferior 1 (process 17710) exited with code 01]
```

�܂��A�C�������̂� `io.c` ���ăR���p�C�����A�C���𔽉f���� `miniruby` �𐶐����܂��B
���ɁAgdb ��� `test.rb` �����s���Ă��܂��B

���s���ʂ́A`nil.to_s` ���Ȃ����߁A`TypeError` ���������A�I�����܂��BRuby �I�ɂ͗�O�����ŏI�����܂������AMRI �I�ɂ́A���̂悤�Ɉُ�I�����邱�Ƃ�����i�킩��Â炢�ł��ˁj�Ȃ̂ŁA���Ȃ��Ɣ��f���Agdb ���I�������܂��B

�ł́A�������̂ŁA`f_hello()` �֐��ɍs�����C�����A�`�P�b�g�ɔ��f�����܂��傤�B�C�����@��`����̂́A�ǂ̂悤�ȕ��@�ł����܂��܂���B

�悭����͎̂��̂悤�ȕ��@�ł��B

* Redmine �`�P�b�g�ɁAdiff ���܂߃R�����g����
* github �� pull request �����A���� URL ���`�P�b�g�ɋL�q����

## `Integer#add(n)` �̃o�O

��قǎ������� `Integer#add(n)` �ɂ͖�肪����Ɖ��ׂ܂����B

TBD
