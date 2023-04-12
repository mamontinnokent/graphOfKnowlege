Слово "грепать" входит в топ самых популярных терминов, используемых разработчиками. Оно происходит от одноимённой консольной утилиты `grep` (**g**lobal **r**egular **e**xpression **p**rint), выполняющей поиск по файлу или файлам определённого текста. Грепать для разработчиков — то же самое, что гуглить для тех, кто активно пользуется интернетом. Как правило, грепают файлы с исходным кодом или логи во время отладки.

```bash
man grep

SYNOPSIS
       grep [OPTIONS] PATTERN [FILE...]
       grep [OPTIONS] [-e PATTERN]...  [-f FILE]...  [FILE...]
```

PATTERN — это то, что ищется, необязательно конкретная строчка, возможно определённый шаблон (см. [регулярные выражения](https://ru.wikipedia.org/wiki/%D0%A0%D0%B5%D0%B3%D1%83%D0%BB%D1%8F%D1%80%D0%BD%D1%8B%D0%B5_%D0%B2%D1%8B%D1%80%D0%B0%D0%B6%D0%B5%D0%BD%D0%B8%D1%8F)). FILE — путь до файла, в котором нужно искать

```bash
# Поиск всех строк в файле .bashrc, в которых встречается слово aliases
grep aliases .bashrc

# enable color support of ls and also add handy aliases
# some more ls aliases
# ~/.bash_aliases, instead of adding them here directly.
if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
```

В примере выше `grep` нашёл 5 строк. Найденные строчки выводятся на экран в том же порядке, в котором они встречаются в исходном файле. В некоторых ситуациях бывает важно увидеть не только саму строку, содержащую подстроку, но и то, что находится вокруг неё. Количество выводимых соседних строк регулируется опциями `-B`, `-A` и `-C`. Первая определяет количество отображаемых строк до искомой (`-B`, `--before-context`), вторая — после (`-A`, `--after-context`), а третья — до и после одновременно (`-C`, `--context`). Ниже пример использования `-C` со значением 1. Это значит, что для каждой найденной строки будет выведена одна строка выше и одна строка ниже.

```bash
grep -C 1 aliases .bashrc

# enable color support of ls and also add handy aliases
if [ -x /usr/bin/dircolors ]; then
--

# some more ls aliases
alias ll='ls -alF'
--
# You may want to put all your additions into a separate file like
# ~/.bash_aliases, instead of adding them here directly.
# See /usr/share/doc/bash-doc/examples in the bash-doc package.

if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi
```

Иногда мы не знаем, в каком файле то, что мы ищем, но знаем директорию, в которой лежит этот файл. В такой ситуации нужно сделать два изменения:

1.  Добавить опцию `-r`, которая говорит о том, что надо искать внутри директории (рекурсивно, то есть включая все поддиректории).
2.  Указать путь до директории, а не файла.

```bash
grep -r bashrc .

./.profile:    # include .bashrc if it exists
./.profile:    if [ -f "$HOME/.bashrc" ]; then
./.profile: . "$HOME/.bashrc"
./.bash_history:du -sh .bashrc
./.bash_history:stat .bashrc
./.bash_history:stat -h .bashrc
./.bash_history:file .bashrc
./.bash_history:stat .bashrc
./.bash_history:cat .bashrc
./.bashrc:# ~/.bashrc: executed by bash(1) for non-login shells.
./.bashrc:# this, if it's already enabled in /etc/bash.bashrc and /etc/profile
./.bashrc:# sources /etc/bash.bashrc).
```

При таком поиске в выводе указывается файл, в котором была найдена строка. Если добавить опцию `n`, то дополнительно отобразится номер строки.

```bash
grep -rn bashrc .

./.profile:13:    # include .bashrc if it exists
./.profile:14:    if [ -f "$HOME/.bashrc" ]; then
./.profile:15:  . "$HOME/.bashrc"
./.bash_history:56:du -sh .bashrc
./.bash_history:57:stat .bashrc
./.bash_history:58:stat -h .bashrc
./.bash_history:60:file .bashrc
./.bash_history:61:stat .bashrc
./.bash_history:63:cat .bashrc
./.bashrc:1:# ~/.bashrc: executed by bash(1) for non-login shells.
./.bashrc:109:# this, if it's already enabled in /etc/bash.bashrc and /etc/profile
./.bashrc:110:# sources /etc/bash.bashrc).
```

