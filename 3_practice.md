# 3. ���K�F���\�b�h�̒ǉ�

���ۂɁAMRI �Ƀ��\�b�h��ǉ����Ă݂܂��傤�B

## `Array#second`

`Array#second` ���\�b�h��ǉ����Ă݂܂��傤�B`Array#first` �͍ŏ��̗v�f��Ԃ��܂��B`Array#second` �͓�ڂ̗v�f��Ԃ����\�b�h�ł��B

Ruby �Œ�`����Ƃ���Ȋ����ł��B

```ruby
# specification written in Ruby
class Array
  def second
    self[1]
  end
end
```

1. `array.c` ���J���܂��傤�B
2. `ary_second()` �Ƃ����֐���ǉ����܂��傤�B`Init_array()` �̑O���ǂ��Ǝv���܂��B
3. `rb_define_method(rb_cArray, "second", ary_second, 0)` �Ƃ����s�� `Init_array()` �֐��ɒǉ����܂��傤�B
4. �r���h���A`ruby/test.rb` �ɃT���v���R�[�h���L�q���āA`make run` �œ����������Ă݂܂��傤�B
5. �e�X�g�� `ruby/test/ruby/test_array.rb` �ɋL�����܂��傤�Bminitest �t�H�[�}�b�g�ł��B
6. `$ make test-all` �Ǝ��s����ƁA�������e�X�g�����s����܂��B�������A�����̃e�X�g�������Ă��܂��̂ŁAArray �̃e�X�g�����ɍi��܂��傤�B
  * `$ make test-all TESTS='ruby/test_array.rb'` �Ƃ��邱�ƂŁA`ruby/test/ruby/test_array.rb` �����e�X�g���܂��B
  * `$ make test-all TESTS='-j8'` �Ƃ��邱�ƂŁA8 ����Ńe�X�g�𑖂点�܂��B
7. �ق��̃��\�b�h���Q�l�ɁA`Array#second` �� rdoc �h�L�������g���L�����Ă݂܂��傤�B

C �ł̒�`�͂���Ȋ����ɂȂ�܂��i���L diff ������Ă��玞�Ԃ������Ă���̂ŁA�s�ԍ��́A����Ă���Ǝv���܂��j�B

```diff
diff --git a/array.c b/array.c
index bd24216af3..79c1c1d334 100644
--- a/array.c
+++ b/array.c
@@ -6131,6 +6131,12 @@ rb_ary_sum(int argc, VALUE *argv, VALUE ary)
  *
  */
 
+static VALUE
+ary_second(VALUE self)
+{
+  return rb_ary_entry(self, 1);
+}
+
 void
 Init_Array(void)
 {
@@ -6251,6 +6257,8 @@ Init_Array(void)
     rb_define_method(rb_cArray, "dig", rb_ary_dig, -1);
     rb_define_method(rb_cArray, "sum", rb_ary_sum, -1);
 
+    rb_define_method(rb_cArray, "second", ary_second, 0);
+
     id_cmp = rb_intern("<=>");
     id_random = rb_intern("random");
     id_div = rb_intern("div");
```

����������Ă����܂��B

* `ary_second()` �������ł��B
* `VALUE` �� Ruby �̃I�u�W�F�N�g�ł���A`self` �̓��\�b�h�Ăяo���ł̃��V�[�o�i`ary.second` �̎��� `ary`�j�ł��B���ׂẴ��\�b�h�Ăяo���́ARuby �̔z���Ԃ��̂ŁA�Ԓl�� `VALUE` �ƂȂ�܂��B
* `rb_ary_entry(self, n)` �� `self[n]` �̈Ӗ��ł���A`n = 1` �Ȃ̂ŁA2�Ԗځi0 origin �Ȃ̂Łj��Ԃ��܂��B
* `Init_Array` �Ƃ����֐����AMRI �N�����Ɏ��s����܂��B
* `rb_define_method(rb_cArray, "second", ary_second, 0);` �ŁA`Array` �N���X�� `second` ���\�b�h���`���Ă��܂��B
  * `rb_cArray` �� Array �N���X�̃I�u�W�F�N�g�ł��B`rb_` �� Ruby �̉����A`c` ���N���X�ł��邱�Ƃ��Ӗ������邽�߁A`rb_cArray` �� Ruby �� `Array` �N���X�ł��邱�Ƃ��킩��܂��B���Ȃ݂ɁA���W���[���̏ꍇ�� `m`�i�Ⴆ�΁A`rb_mEnumerable`�A�G���[�N���X�̏ꍇ�� `e`�i�Ⴆ�΁A`rb_eArgError`�j�B
  * `rb_define_method` ���C���X�^���X���\�b�h���`����֐��ł��B
  * �u`rb_cArray` �ɁA`"second"` �Ƃ������O�̃��\�b�h���`����B���\�b�h���Ă΂ꂽ�� `ary_second` ���Ăяo���B�Ȃ��A�����̐��� 0 �ł���v�Ƃ����Ӗ��ɂȂ�܂��B

## `String#palindrome?`

�񕶔��胁�\�b�h `String#palindrome?` ���`���Ă݂܂��傤�B

���̃R�[�h�� Ruby �ŏ��������́A����т�����Ƃ����e�X�g�ł��B

```ruby
class String
  def palindrome?
    chars = self.gsub(/[^A-z0-9\p{hiragana}\p{katakana}]/, '').downcase
    # p chars
    !chars.empty? && chars == chars.reverse
  end
end

# Small sample program
# Sample palindrome from https://en.wikipedia.org/wiki/Palindrome
[# OK
 "Sator Arepo Tenet Opera Rotas",
 "A man, a plan, a canal - Panama!",
 "Madam, I'm Adam",
 "NisiOisiN",
 "�킩�݂����̂Ƃ��Ȃ��Ƃ̂����݂���",
 "�A�j�}���}�j�A",
 # NG
 "",
 "ab",
].each{|str|
  p [str, str.palindrome?]
}
```

Ruby �R�[�h���AC �̃R�[�h�ɒ��ړI�ɕϊ����Ă݂܂��B
`Array#second` �ł̎菇���Q�l�ɁA���L��ύX���Ă݂Ă��������B

```diff
diff --git a/string.c b/string.c
index c140148778..0f170bd20b 100644
--- a/string.c
+++ b/string.c
@@ -10062,6 +10062,18 @@ rb_to_symbol(VALUE name)
     return rb_str_intern(name);
 }
 
+static VALUE
+str_palindrome_p(VALUE self)
+{
+  const char *pat = "[^A-z0-9\\p{hiragana}\\p{katakana}]";
+  VALUE argv[2] = {rb_reg_regcomp(rb_utf8_str_new_cstr(pat)),
+                  rb_str_new_cstr("")}; 
+  VALUE filtered_str = rb_str_downcase(0, NULL, str_gsub(2, argv, self, FALSE));
+  return rb_str_empty(filtered_str) ? Qfalse : 
+         rb_str_equal(filtered_str, rb_str_reverse(filtered_str));
+                                     
+}
+
 /*
  *  A <code>String</code> object holds and manipulates an arbitrary sequence of
  *  bytes, typically representing characters. String objects may be created
@@ -10223,6 +10235,8 @@ Init_String(void)
     rb_define_method(rb_cString, "valid_encoding?", rb_str_valid_encoding_p, 0);
     rb_define_method(rb_cString, "ascii_only?", rb_str_is_ascii_only_p, 0);
 
+    rb_define_method(rb_cString, "palindrome?", str_palindrome_p, 0);
+
     rb_fs = Qnil;
     rb_define_hooked_variable("$;", &rb_fs, 0, rb_fs_setter);
     rb_define_hooked_variable("$-F", &rb_fs, 0, rb_fs_setter);
```

������܂��B

* `rb_reg_regcomp(pat)` �ɂ���āA`pat` �Ƃ��� C �̕�����𐳋K�\���I�u�W�F�N�g�Ƃ��ăR���p�C�����܂��B
* `rb_str_new_cstr("")` �ŁA��� Ruby ������𐶐����܂��B
* `str_gsub()` �ŁA`String#gsub` �����̏������s���܂��B�����ł́A���K�\�����g���āA���������ȊO������Ă��܂��B
* `rb_str_downcase()` �ŁA���̌��ʂ��������ɂ��낦�܂��B
* `rb_str_empty()` �ŁA�t�B���^���ʂ��󕶎���ł��邩�ǂ������`�F�b�N���܂��B
* `rb_str_reverse()` �ŁA������̏����̋t�]�����Ă��܂��B
* `rb_str_equal()` �ŁA������̔�r�����Ă��܂��B

�Ȃ�ƂȂ��ARuby �̃R�[�h�ƈ�Έ�ɑΉ����Ă���̂��킩��ł��傤���B

## `Integer#add(n)`

`Integer` �N���X�ɁA`n` �������\�b�h�����܂��傤�B

Ruby �ŏ����ƁA����Ȋ����ł��B

```
class Integer
  def add n
    self + n
  end
end

p 1.add(3) #=> 4
p 1.add(4.5) #=> 5.5
```

```
Index: numeric.c
===================================================================
--- numeric.c	(���r�W���� 59647)
+++ numeric.c	(��ƃR�s�[)
@@ -5238,6 +5238,12 @@
     }
 }
 
+static VALUE
+int_add(VALUE self, VALUE n)
+{
+    return rb_int_plus(self, n);
+}
+
 /*
  *  Document-class: ZeroDivisionError
  *
@@ -5449,6 +5455,8 @@
     rb_define_method(rb_cInteger, "bit_length", rb_int_bit_length, 0);
     rb_define_method(rb_cInteger, "digits", rb_int_digits, -1);
 
+    rb_define_method(rb_cInteger, "add", int_add, 1);
+
 #ifndef RUBY_INTEGER_UNIFICATION
     rb_cFixnum = rb_cInteger;
 #endif
```

1�������K�{�Ȃ̂ŁA`rb_define_method()` �̍Ō�̈����� `1` �ɂȂ��Ă���A`int_add()` �̈����� `VALUE n` ���ǉ�����Ă��܂��B

���ۂɁA�����Z���s�������� `rb_int_plus()` ���s���Ă��܂��B���̂��߁A��������͏����Ă��܂���B�����A`self` �� `n` �� `Fixnum`�i������̏����Ȑ��l�AC �� `int` �ւ̕ϊ��A`int` ����̕ϊ����e�Ձj�ł���ꍇ�����AC �ő����Z�����Ă݂܂��傤�B

�Ȃ��ARuby 2.3 �܂ł́A�����l�� `Fixnum` �N���X�� `Bignum` �N���X�ɕ�����Ă��܂������ARuby 2.4 ����� `Integer` �ɓ�������܂����B�������AMRI �����ł́A�i���\��̊ϓ_����j��������ʂ��ĊǗ����Ă��܂��i�Ⴆ�΁A`FIXNUM_P(bignum)` �Ƃ���ƋU���Ԃ�܂��j�B

```
Index: numeric.c
===================================================================
--- numeric.c	(���r�W���� 59647)
+++ numeric.c	(��ƃR�s�[)
@@ -5238,6 +5238,22 @@
     }
 }
 
+static VALUE
+int_add(VALUE self, VALUE n)
+{
+    if (FIXNUM_P(self) && FIXNUM_P(n)) {
+	/* c = a + b */
+	int a = FIX2INT(self);
+	int b = FIX2INT(n);
+	int c = a + b;
+	VALUE result = INT2NUM(c);
+	return result;
+    }
+    else {
+	return rb_int_plus(self, n);
+    }
+}
+
 /*
  *  Document-class: ZeroDivisionError
  *
```

`FIXNUM_P(self) && FIXNUM_P(n)` �ɂ���āA`self` �� `n` �� `Fixnum` �ł��邩�ǂ������`�F�b�N���Ă��܂��B
���������ł���΁A`FIX2INT()` �ɂ���āA`int` �ɕϊ��ł���̂ŁA�ϊ����v�Z���Ă��܂��B�v�Z���ʂ� `FIX2NUM()` �ɂ���āAVALUE �^�i�܂�ARuby �� `Integer` �N���X�̃I�u�W�F�N�g�j�֕ϊ����A

�����ӁF���́A���̏C���ł̃v���O�����ɂ̓o�O������܂��B

## `Time#day_before(n=1)`

`Time` �N���X�� n ���O�̒l�i�������������1���O�j��Ԃ����\�b�h�������Ă݂܂��傤�B

Ruby �ŏ����Ƃ���Ȋ����ł��B24���� * n �̕b�������炵�Ă��܂��B�����ɂ́A���̕��@�� n ���O���v�Z����Ƃ������Ƃ͏o���܂���i�[�b�Ƃ��B�������� n ���O�Ƃ́H�j�B���A����̓T���v���Ȃ̂ŁA���܂�ׂ������Ƃ��l���Ȃ��悤�ɂ��悤�Ǝv���܂��B

```
class Time
  def day_before n = 1
    Time.at(self.to_i - (24 * 60 * 60 * n))
  end
end

p Time.now               #=> 2017-08-24 14:48:44 +0900
p Time.now.day_before    #=> 2017-08-23 14:48:44 +0900
p Time.now.day_before(3) #=> 2017-08-21 14:48:44 +0900
```

C �ŏ����Ă݂�ƁA����Ȋ����ł��B

```
Index: time.c
===================================================================
--- time.c	(���r�W���� 59647)
+++ time.c	(��ƃR�s�[)
@@ -4717,6 +4717,22 @@
     return time;
 }
 
+static VALUE
+time_day_before(int argc, VALUE *argv, VALUE self)
+{
+    VALUE nth;
+    int n, sec, day_before_sec;
+
+    rb_scan_args(argc, argv, "01", &nth);
+    if (nth == Qnil) nth = INT2FIX(1);
+    n = NUM2INT(nth);
+
+    sec = NUM2INT(time_to_i(self));
+    day_before_sec = sec - (60 * 60 * 24 * n);
+
+    return rb_funcall(rb_cTime, rb_intern("at"), 1, INT2NUM(day_before_sec));
+}
+
 /*
  *  Time is an abstraction of dates and times. Time is stored internally as
  *  the number of seconds with fraction since the _Epoch_, January 1, 1970
@@ -4896,6 +4912,8 @@
 
     rb_define_method(rb_cTime, "strftime", time_strftime, 1);
 
+    rb_define_method(rb_cTime, "day_before", time_day_before, -1);
+
     /* methods for marshaling */
     rb_define_private_method(rb_cTime, "_dump", time_dump, -1);
     rb_define_private_method(rb_singleton_class(rb_cTime), "_load", time_load, 1);
```

�|�C���g��������܂��B

* �ϒ������ɂ��邽�߂ɁA`rb_define_method()` �� `-1` ���w�肵�Ă��܂��B�����邩�킩��܂����A�Ƃ����Ӗ��ɂȂ�܂��B
* `time_day_before(int argc, VALUE *argv, VALUE self)` �Ƃ����֐��Ń��\�b�h�̎��̂��`���Ă��܂��B`argc` �Ɉ����̐��A`argv` �ɒ��� `argc` VALUE �̔z��ւ̃|�C���^���i�[����Ă��܂��B
* `rb_scan_args()` ���g���A�������`�F�b�N���Ă��܂��B`"01"` �Ƃ����̂́A�K�{������ 0 �A�I�v�V���i�������� 1 �A�Ƃ����Ӗ��ɂȂ�܂��B�܂�A0 or 1 �̈��������A�Ƃ������ƂɂȂ�A���� 1 ����������Ă���΁A`nth` �Ɋi�[����܂��B�����A������ 0 �̏ꍇ�i�܂�A�����������ꍇ�j�́A`nth` �ɂ� `Qnil` �iRuby �ł� `nil` ���AC �ł͂��̂悤�ɕ\�����Ă���j���i�[����܂��B
* `Time.at()` ���������邽�߂ɁA`rb_funcall(recv, mid, argc, ...)` �𗘗p���Ă��܂��B
  * �������̓��V�[�o�A�܂� `recv.mid(...)` �̎��� `recv` �ɂȂ�܂��B`Time.at` �ł́A���V�[�o�� `Time` �N���X�I�u�W�F�N�g�A�Ƃ������ƂɂȂ�܂��B
  * ���\�b�h���̎w��� ID �ōs���܂��BID �𐶐����邽�߂ɂ́A`rb_intern("...")` �𗘗p���܂��BID �́A���镶����ɑ΂��āAMRI �v���Z�X���ň�ӂȒl�̂��Ƃł��BRuby �ł��� Symbol�AJava �ł��� "intern" ����������ł��B
  * 1 �����Ȃ̂ŁA1 �Ǝw�肵�A���̌�Ŏ��ۂ̈������w�肵�܂��B

�Ȃ��A���̎����ɂ͐F�X�Ɩ�肪����܂��BRuby �����Ɖ����Ⴄ�̂��A�������Ă݂Ă��������B

## �f�o�b�O�� Tips

Ruby �v���O�����������Ƃ��́A`p(obj)` ���\�b�h�𗘗p���邱�Ƃ�����Ǝv���܂��BC �ł� `rb_p(obj)` �Ƃ��邱�ƂŁA���l�ɏo�͂��邱�Ƃ��ł��܂��B

gdb ���g����悤�ł�����A�u���C�N�|�C���g���w�肵�� `$ make gdb` ���g���Ď��s����ƁA�������m�F���邱�Ƃ��ł��܂��B
`#include "debug.h"` �Ƃ���ƁA`bp()` �Ƃ����}�N�����g����悤�ɂȂ�܂��B���� `bp()` �����ߍ��܂ꂽ�Ƃ���̓u���C�N�|�C���g�Ƃ��čŏ�����o�^����Ă��邽�߁A�C�ɂȂ�Ƃ���� `bp()` ��u���ƕ֗���������܂���i�܂�A`binding.pry` �̂悤�Ɏg���܂��j�B

## ���W���K

���̃g�s�b�N���A���ۂɉ������Ă݂Ă��������B�����悤�Ȏ����� MRI �̃\�[�X�R�[�h�� grep ���ĒT���Ă݂Ă��������B

* �����Z���s�� `Integer#sub(n)` ���������Ă݂Ă��������B
* `Array#second` �́A�v�f���� 1 �ȉ��̏ꍇ�� `nil` ��Ԃ��܂��B�Ƃ����̂��A`rb_ary_entry()` �́A���݂��Ȃ��v�f�C���f�b�N�X���w�肳���� `nil`�i`Qnil`�j��Ԃ����߂ł��B�����ŁA2�v�f�ڂ��Ȃ��ꍇ�͗�O�𔭐�����悤�ɂ��Ă݂Ă��������B`rb_raise()` �Ƃ����֐��𗘗p���܂��B
* `String#palindrome?` �́A������Ȏ����ɂȂ��Ă��܂��B�ǂ���������ł���A�ǂ̂悤�ɉ����ł��邩�������Ă݂Ă��������B�܂��A�\�Ȃ琫�\�����P����悤�Ɏ�����ύX���Ă݂Ă��������B
* `Integer#add(n)` �ɂ̓o�O������ƌ����܂����B�ǂ̂悤�ȃo�O������ł��傤���B�܂��A�ǂ̂悤�ɉ����ł���ł��傤���B�܂��͎��s����e�X�g�������܂��傤�B�����āA������������A�e�X�g���ʂ邱�Ƃ��m�F���܂��傤�B
* `Time#day_before` �͖��O�������ł��B�ǂ����O���l���Ă݂Ă��������B
* `Time#day_before` �̎����̖��_�� `Integer#add(n)` �Ɠ��l�ɍl���Ă݂Ă��������B
* MRI �ɂ������炵�Ă݂܂��傤�B`Integer#+` �̌��ʂ��A�����Z�ł͂Ȃ��A�����Z�������ʂɂȂ�悤�ɂ��Ă��������B
