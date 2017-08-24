# (2) MRI �\�[�X�R�[�h�̍\��

## ���̎����ɂ���

MRI �̃\�[�X�R�[�h�̍\���ɂ��ďЉ�܂��B�܂��ARuby �̃\�[�X�R�[�h���n�b�N����Œ���̒m�����Љ�܂��B

* ���K: MRI �̃\�[�X�R�[�h�� clone
* ���K: MRI �̃r���h�A����уC���X�g�[��
* MRI �̍\���̏Љ�
* ���K: �r���h���� Ruby �Ńv���O���������s

## �{�e�őO��Ƃ���f�B���N�g���\��

���L�̃R�}���h�́ALinux �� Mac OSX �Ȃǂ�O��Ƃ��Ă��܂��BWindows �����g���ꍇ�́A�ʓr�撣���Ă��������B

�O��Ƃ���f�B���N�g���\��:

* `workdir/`
  * `ruby/` <- git clone ����f�B���N�g��
  * `build/` <- �r���h�f�B���N�g���i�����ɁA�R���p�C������ `*.o` �Ȃǂ�����j
  * `install/` <- �C���X�g�[���f�B���N�g�� (`workdir/install/bin/ruby` ���C���X�g�[�����ꂽ�f�B���N�g���ɂȂ�܂��j

�O��Ƃ���R�}���h�F

git�Aruby�Aautoconf�Abison�Agcc (or clang, etc�j�Amake ���K�{�ł��B���̑��A�ˑ����C�u����������΁A�g�����C�u��������G�t����܂��B

apt-get ���g������ł́A���L�̂悤�ȃR�}���h�ŃC���X�g�[������܂��B

```
$ sudo apt-get install git ruby autoconf bison gcc make zlib1g-dev libffi-dev libreadline-dev libgdbm-dev libssl-dev
```

## ���K: MRI �̃\�[�X�R�[�h�� clone

1. `$ mkdir workdir`
2. `$ cd workdir`
3. `$ git clone https://github.com/ruby/ruby.git` # workdir/ruby �Ƀ\�[�X�R�[�h�� clone ����܂�

�i�l�b�g���[�N�ш�̖�肪����̂ŁA�ł���ΉƂȂǂōs���Ă��Ă��������j

## ���K: MRI �̃r���h�A����уC���X�g�[��

1. ��L�u�O��Ƃ���R�}���h�v���m�F
2. `$ cd workdir/` # workdir �Ɉړ����܂�
3. `$ cd ruby` # workdir/ruby �Ɉړ����܂�
4. `$ autoconf`
5. `$ cd ..`
6. `$ mkdir build` # `workdir/build` ���쐬���܂�
7. `$ cd build`
8. `$ ../ruby/configure --prefix=$PWD/../install --enable-shared`
  * `prefix` �́A�C���X�g�[�������̃f�B���N�g���ł��B��΃p�X�ŁA�D���ȏꏊ���w�肵�Ă��������i���̗�ł� `workdir/install`�j
  * Homebrew �ŏ��X�C���X�g�[�����Ă���ꍇ�́A `--with-openssl-dir="$(brew --prefix openssl)" --with-readline-dir="$(brew --prefix readline)" --disable-libedit` ��t���Ă��������B
9. `$ make -j` # �r���h���܂��B`-j` �͕���ɃR���p�C���Ȃǂ��s���I�v�V�����ł��B
10. `$ make install` # tips: `make install-nodoc` �Ƃ���ƁArdoc/ri �h�L�������g�̃C���X�g�[�����X�L�b�v���܂�
11. `$ ../install/bin/ruby -v` �ŁARuby ���C���X�g�[�����ꂽ���Ƃ��m�F���Ă�������

NOTE: `V=1` option �ɂ���āA`make` �R�}���h����̓I�ɂǂ̂悤�ȃR�}���h�����s���Ă��邩��\�����܂��B�f�t�H���g�i`V=0`�j�ł́A�����̕\����}�����Ă��܂��B

## ���K�F�r���h���� Ruby �Ńv���O���������s���Ă݂悤

�r���h���� Ruby �Ŏ��ۂ� Ruby �X�N���v�g�����s������@�͂���������܂��B

��Ԃ킩��₷�����@�́A�C���X�g�[���܂ŏI��点�A�C���X�g�[������ Ruby �𗘗p���Ď��s���邱�Ƃł��B�u���� Ruby ���g���Ă�����@�v�ƑS�������ł��B�ł����ARuby ���C�����邽�т� Ruby �̃C���X�g�[���܂ōs���ƁA�኱���Ԃ�������܂��i�}�V���ɂ��܂����A`make install` ���I���܂łɐ��\�b������܂��j�B

�����ł́A����ȊO�́ARuby ���C������Ƃ��ɕ֗��Ȏ��s���@���Љ�܂��B

### miniruby �Ŏ��s���悤

Ruby �̃r���h���I���ƁA�r���h�f�B���N�g���i`workdir/build`�j�ɁA`miniruby` �Ƃ������s�t�@�C������������܂��B`miniruby` �́ARuby �̃r���h���邽�߂ɍ����A�@�\�����ł� Ruby �C���^�v���^�ł��B�����A�����Ƃ����Ă��A�g�����C�u������ǂݍ��ނ��Ƃ��ł��Ȃ��A�G���R�[�f�B���O�ɐ��񂪂���A�Ƃ��������̂ł���ARuby ���@�̂قƂ�ǂ��T�|�[�g���Ă��܂��B

`miniruby` �́ARuby �̃r���h�̏����i�K�Ő�������邽�߁AMRI�̏C�����s���A���̌��ʂ��m�F���邽�߂ɂ́A`miniruby` �����s���ďC�����ʂ��m�F����̂��ǂ��ł��B�܂�A

1. MRI �̃\�[�X�R�[�h���C������
2. `make miniruby` �Ƃ��āA`minirbuy` �𐶐�����i���ׂăr���h���ăC���X�g�[��������������I���j
3. �C���Ɋ֌W����X�N���v�g�� `minirbuy` �Ŏ��s����

�Ƃ�������ŊJ����i�߂�ƌ����I�ł��B

���̗�����s�����߂ɁA`make run` �Ƃ��� make �̃��[��������܂��B������s���� `miniruby` ���r���h���A`workdir/ruby/test.rb` �i�\�[�X�f�B���N�g���j�ɏ����ꂽ���e�����s���܂��B

�܂�A���L�̂悤�ɐi�߂��܂��B

1. `ruby/test.rb` �Ɏ��s������ Ruby �X�N���v�g��\������igem ��g�����C�u�����͎g���Ȃ��̂Œ��Ӂj�B�܂��ARuby �̃\�[�X�R�[�h���C������B
2. �r���h�f�B���N�g���i`workdir/build`�j�� `$ make run` �����s����

### miniruby �ł͂Ȃ��A�t���Z�b�g�� ruby �Ŏ��s���悤

�g�����C�u�������܂ށu���ʂ́vRuby�����s���������́A`make run` �̑���� `make runruby` ���g���܂��B`make install` ���Ȃ��Ŏ��s�ł��邽�߁A�኱�����J�����i�߂��܂��B

1. `ruby/test.rb` �Ɏ��s������ Ruby �X�N���v�g��\������igem ��g�����C�u�����͎g���Ȃ��̂Œ��Ӂj�B�܂��ARuby �̃\�[�X�R�[�h���C������B
2. �r���h�f�B���N�g���i`workdir/build`�j�� `$ make runruby` �����s����

### gdb ��p���ăf�o�b�O���悤

NOTE: Mac OSX �� gdb �𓮂����͓̂���悤�ł��B���L�́ALinux ����O���ɉ�����Ă��܂��B

Ruby �̃\�[�X�R�[�h���C������ƁAC �v���O�����Ȃ̂ŗe�Ղ� SEGV �Ƃ������N���e�B�J���Ȗ����ȒP�ɔ��������邱�Ƃ��ł��܂��i���������Ⴂ�܂��j�B�����ŁAgdb ���g���ăf�o�b�O���邽�߂̕��@��p�ӂ��Ă��܂��B�������A�u���C�N�|�C���g��p�����f�o�b�O�Ȃǂł����p�\�ł��B

1. `ruby/test.rb` �Ƀe�X�g������ Ruby �X�N���v�g���L�q����
2. �r���h�f�B���N�g���i`workdir/build`�j�� `$ make gdb` �����s����i��肪�N����Ȃ���΁A�������Ȃ��I�����܂��j

���̂Ƃ��A���p����̂� `./miniruby` �ɂȂ�܂��B`./ruby` ��p�������ꍇ�� `make gdb-ruby` �Ƃ��Ă��������B

�����A�u���C�N�|�C���g��}���������ꍇ�́A`make gdb` �R�}���h�Ńr���h�f�B���N�g���ɐ�������� `run.gdb` �Ƃ����t�@�C���ɁA�Ⴆ�� `b func_name` �Ƃ������u���C�N�|�C���g�w��������Ă��������B

### Ruby �̃e�X�g�����s����

1. `$ make btest` # run bootstrap tests in `ruby/bootstraptest/`
2. `$ make test-all` # run test-unit tests in `ruby/test/`
3. `$ make test-spec` # run tests provided in `ruby/spec`

�����̎O�́A���ꂼ��ʁX�̖ړI�E�����������ĊJ������Ă��܂��B

## MRI �̃\�[�X�R�[�h�̍\���̏Љ�

### �C���^�v���^

��G�c�ɁA���L�̂悤�ȃf�B���N�g���\���ɂȂ��Ă��܂��B

* `ruby/*.c` MRI core files
    * VM cores
        * VM
            * `vm*.[ch]`: VM �̎���
            * `vm_core.h`: VM �f�[�^�\���̒�`
            * `insns.def`: VM �̖��ߒ�`
        * `compile.c, iseq.[ch]`: ���ߗ�֌W�̏���
        * `gc.c`: GC �ƃ������Ǘ�
        * `thread*.[ch]`: �X���b�h�Ǘ�
        * `variable.c`: �ϐ��Ǘ�
        * `dln*.c`: C�g���̂��߂̃_�C�i�~�b�N�����N���C�u�����Ǘ�
        * `main.c`, `ruby.c`: MRI �̃G���g���[�|�C���g
        * `st.c`: �n�b�V���e�[�u���A���S���Y���̎��� (�Q�l: https://blog.heroku.com/ruby-2-4-features-hashes-integers-rounding)
    * �g�ݍ��݃N���X
        * `string.c`: String class
        * `array.c`: Array class
        * ... (���������A�N���X���ɑΉ�����t�@�C�����ɒ�`���i�[����Ă��܂��j
* `ruby/*.h`: ������`�B�g�����C�u�����͊�{�I�Ɏg���܂���
* `ruby/include/ruby/*`: �O����`�B�g�����C�u�����ŎQ�Ƃł��܂�
* `enc/`: �G���R�[�f�B���O�̂��߂̃\�[�X�R�[�h����
* `defs/`: �e���`
* `tools/`: MRI ���r���h�E���s���邽�߂̃c�[��
* `missing/`: �������� OS �ő���Ȃ����̂̎���
* `cygwin/`, `nacl/`, `win32`, ...: OS/system �ˑ��̃\�[�X�R�[�h

### ���C�u����

���C�u������ 2 ��ނ���܂��B

* `lib/`: �W���Y�t�̃��C�u�����iRuby �ŋL�q���ꂽ���C�u�����j
* `ext/`: �W���Y�t�̊g�����C�u�����iC �ŋL�q���ꂽ���C�u�����j

### �e�X�g

* `basictest/`: place of old test
* `bootstraptest/`: bootstrap test
* `test/`: tests written by test-unit notation
* `spec/`: tests written by RSpec notation

### misc

* `doc/`, `man/`: �h�L�������g

## Ruby �̃r���h�v���Z�X

Ruby �̃r���h�ł́A�\�[�X�R�[�h�𐶐����Ȃ���r���h��i�߂Ă����܂��B�\�[�X�R�[�h�𐶐����邢�����̃c�[���� Ruby ��p���邽�߁ARuby �̃r���h�ɂ� Ruby ���K�v�ɂȂ�܂��B�\�[�X�R�[�h�z�z�p�� tar ball �ɂ́A����琶�����ꂽ�\�[�X�R�[�h�����킹�Ĕz�z���Ă���̂ŁAtar ball ��p����̂ł���΁ARuby �̃r���h�� Ruby �i��A���̑� bison �Ȃǂ̊O���c�[���j�͕s�v�ł��B

�t�Ɍ����ƁASubversion �� Git ���|�W�g������\�[�X�R�[�h���擾�����ꍇ�́ARuby �C���^�v���^���K�v�ɂȂ�܂��B

�r���h�E�C���X�g�[���́A���̂悤�ɐi�݂܂��B

1. miniruby �̃r���h
    1. parse.y -> parse.c: Compile syntax rules to C code by bison
    2. insns.def -> vm.inc: Compile VM instructions to C code by ruby (`BASERUBY`)
    3. `*.c` -> `*.o` (`*.obj` on Windows): Compile C codes to object files.
    4. link object files into miniruby
2. �G���R�[�f�B���O�̃r���h
    1. translate enc/... to appropriate C code by `miniruby`
    2. compile C code
3. C �g�����C�u�����̃r���h
    1. Make `Makefile` from `extconf.rb` by `mkmf.rb` and `miniruby`
    2. Run `make` using generated `Makefile`.
4. `ruby` �R�}���h�̃r���h
5. `rdoc`, `ri` �h�L�������g�̐���
6. �������ꂽ�t�@�C���̃C���X�g�[���i�C���X�g�[����� `configure` �� `--prefix` �Ŏw�肵�����́j

���́A�{���͂����ƐF�X����Ă���̂ł����A�����؂�Ȃ����A�����c�����Ă��Ȃ��̂ŁA�ȗ����Ă��܂��B`common.mk` �Ƃ����� make �p�̃��[���W�ɁA���낢��ȃt�@�C���������Ă��܂��B

# 2.1 �o�[�W�����\�L�̏C���i�����j

�ł́A���ۂ� Ruby ���C�����Ă����܂��傤�B�\�[�X�R�[�h�͂��ׂ� `workdir/ruby/' �ɂ���Ɖ��肵�܂��B

�܂��́A`ruby -v`�i�������� `./miniruby -v`�j�����s�����Ƃ��ɁA������ Ruby ���Ƃ킩��悤�ɁA�����\����ς��Ă݂܂��傤�B

1. �o�[�W�����\�L���s���R�[�h�� `version.c` �ɂ���̂ŁA������J���܂��B
2. �����A�\�[�X�R�[�h�S�̂𒭂߂Ă݂܂��傤�B
3. `ruby_show_version()` �Ƃ����֐������������ł��i�֐�������Ύ����H�j�B
4. `fflush()` ���A�o�͂��m�肷��i�o�̓o�b�t�@��f���o���j C �̊֐��Ȃ̂ŁA���̑O�ɉ��炩�̏o�͂�����Ηǂ��Ɛ����B
5. `printf("...\n");` �i`...` �̕����ɂ́A�D���ȕ�����j���L���B
6. `$ make miniruby` �Ńr���h�i�r���h�f�B���N�g���Ɉړ����Ă����j�B
7. `$ ./miniruby -v` �Ō��ʂ��m�F�B
8. `$ make install` �ŃC���X�g�[���B
9. `$ ../install/bin/ruby -v` �ŃC���X�g�[�����ꂽ ruby �R�}���h�ɂ��ύX�����f���ꂽ���Ƃ��m�F�B

�Ō�� `printf(...)` �����ނ����ł͂Ȃ��A`ruby ...` �Ə����ꂽ�s��ύX���Ă��ʔ�����������܂���ˁB`perl` �Əo�͂��Ă݂�Ƃ��B

