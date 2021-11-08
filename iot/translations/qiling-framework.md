---
description: '@beh1ndy0urback November 16, 2019'
cover: >-
  https://images.unsplash.com/photo-1555664424-778a1e5e1b48?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw3fHxpb3R8ZW58MHx8fHwxNjM2Mzk0Mjc0&ixlib=rb-1.2.1&q=85
coverY: 338.0992430613962
---

# Qiling Framework

## **Угрозы, их исследование и возможные решения**

Уязвимость IoT-устройств на сегодняшний день стала актуальнее, чем когда-либо. О существовании малварей, использующих уязвимые, часто плохо защищенные и настроенные IoT-устройства, известно уже много лет. Поставщики оборудования и вся индустрия безопасности борются с этим, пытаясь создавать наиболее качественные и безопасные продукты, но, к сожалению, угроза уязвимых IoT-устройств и малварей для них остаются двумя самыми большими проблемами в индустрии безопасности.

Современные IoT-угрозы и малвари распространяются на различные платформы и архитектуры ЦП. Реверс-инженеры пытаются разобраться с этим и понять строение различных ОС и архитектур процессорв, но отсутствие актуальных инструментов ухудшает ситуацию. Не за горами момент, когда доступные в настоящее время инструменты перестанут быть актуальны из-за быстроразвивающихся угроз безопасности.

Общих методов, используемых для выполнения анализа, таких как полная эмуляция системы, эмуляция пользовательского режима, бинарные инструменты, дизассемблер и песочница, едва ли достаточно. Эти инструменты либо обслуживают операционную систему одного типа, либо работают на архитектуре с одним ЦП. Кроме того, эти инструменты необходимо использовать отдельно, упорядочивание информации или перекрестные ссылки использовать практически невозможно. По этим причинам реверс-инжиниринг никогда не бывает легкой задачей.

> Исследования, проведенные SonicWall, показали, что в 2018 году был зафиксирован рекордный уровень вредоносных атак в количестве 10,52 млрд., что указывает на рост объема кибератак, а также на новую тактику, используемую киберпреступниками.

## **Решение**

Цель Qiling Framework - оптимизировать исследования в области безопасности IoT, анализа малварей и реверс-инжиниринга. Основная цель - создать кроссплатформенную и мультиархитектурную среду, а не просто инструмент для ревер-инжиниринга.

Qiling Framework содержит бинарный инструментарий и фреймворк для эмуляции, который поддерживает кроссплатформенность и мульти-архитектуру. Он обладает мощными функциями, такими как перехват кода и внедрение произвольного кода до или во время выполнения. Он также умеет патчить упакованный бинарный файл во время выполнения.

Qiling Framework имеет открытый исходный код и написан на Python, простом и широко используемом языке программирования. Это будет стимулировать постоянный вклад со стороны сообщества и открытых источников, делая Qiling Framework устойчивым проектом.

## **Что такое Qiling Framework**

Qiling Framework - это не просто платформа эмуляции или инструмент реверс-инжиниринга, он объединяет бинарный инструментарий и бинарную эмуляцию в единую структуру. С Qiling Framework вы можете:

* Перенаправить поток выполнения на лету
* Пропатчить файл во время исполнения
* Внедренить код во время исполнения
* Выполнить файл частично
* Пропатчить «распакованное» содержимое двоичного файла

Qiling Framework может эмулировать:

* Windows X86 32/64bit
* Linux X86 32/64bit, ARM, AARCH64, MIPS
* MacOS X86 32/64bit
* FreeBSD X86 32/64bit

Qiling Framework может работать поверх Windows/MacOS/Linux/FreeBSD и на любой архитектуре ЦП

## **Демонстрация работы Qiling Framework**

Используемая среда:

* _Аппаратное обеспечение: X86-64_
* _ОС: Ubuntu 18.04 64bit_

### **Демо #1: Ловим kill switch Wannacry**

Qiling Framework выполняет двоичный файл Wannacry, перехватывая адрес 0x40819a, чтобы перехватить URL killerswitch

[https://youtu.be/gVtpcXBxwE8](https://youtu.be/gVtpcXBxwE8)

#### **Образец кода**

```
from qiling import *

def stopatkillerswtich(ql):
    ql.uc.emu_stop()

if __name__ == "__main__":
    ql = Qiling(["rootfs/x86_windows/bin/wannacry.bin"], "rootfs/x86_windows")
    ql.hook_address(stopatkillerswtich, 0x40819a)
    ql.run()
```

#### **Вывод**

```
0x1333804: __set_app_type(0x2)
0x13337ce: __p__fmode() = 0x500007ec
0x13337c3: __p__commode() = 0x500007f0
0x132f1e1: _controlfp(0x10000, 0x30000) = 0x8001f
0x132d151: _initterm(0x40b00c, 0x40b010)
0x1333bc0: __getmainargs(0xffffdf9c, 0xffffdf8c, 0xffffdf98, 0x0, 0xffffdf90) = 0
0x132d151: _initterm(0x40b000, 0x40b008)
0x1001e10: GetStartupInfo(0xffffdfa0)
0x104d9f3: GetModuleHandleA(0x00) = 400000
0x125b18e: InternetOpenA(0x0, 0x1, 0x0, 0x0, 0x0)
0x126f0f1: InternetOpenUrlA(0x0, "http://www.iuqerfsodp9ifjaposdfjhgosurijfaewrwergwea.com", "", 0x0, 0x84000000, 0x0)
```

### **Демо #2: Эмуляция прошивки роутера ARM на Ubuntu X64**

Хот-патч с помощью Qiling Framework и эмуляция /usr/bin/httpd ARM роутера на X86-64, Ubuntu

[https://youtu.be/Nxu742-SNvw](https://youtu.be/Nxu742-SNvw)

```
from qiling import *

def my_sandbox(path, rootfs):
    ql = Qiling(path, rootfs, stdin = sys.stdin, stdout = sys.stdout, stderr = sys.stderr)
    # Patch 0x00005930 from br0 to ens33
    ql.patch(0x00005930, b'ens33\x00', file_name = b'libChipApi.so')
    ql.root = False
    ql.run()


if __name__ == "__main__":
    my_sandbox(["rootfs/tendaac15/bin/httpd"], "rootfs/tendaac15")
```

### **Демо #3: хот-патч crackme в Windows**

Использование Qiling Framework для хот-патча crackme в Windows, чтоб он всегда отображал диалоговое окно «Congratulation»

[https://youtu.be/p17ONUbCnUU](https://youtu.be/p17ONUbCnUU)

#### **Образец кода**

```
from qiling import *

def force_call_dialog_func(ql):
    # get DialogFunc address
    lpDialogFunc = ql.unpack32(ql.mem_read(ql.sp - 0x8, 4))
    # setup stack memory for DialogFunc
    ql.stack_push(0)
    ql.stack_push(1001)
    ql.stack_push(273)
    ql.stack_push(0)
    ql.stack_push(0x0401018)
    # force EIP to DialogFunc
    ql.pc = lpDialogFunc


def my_sandbox(path, rootfs):
    ql = Qiling(path, rootfs)
    # NOP out some code
    ql.patch(0x004010B5, b'\x90\x90')
    ql.patch(0x004010CD, b'\x90\x90')
    ql.patch(0x0040110B, b'\x90\x90')
    ql.patch(0x00401112, b'\x90\x90')
    # hook at an address with a callback
    ql.hook_address(0x00401016, force_call_dialog_func)
    ql.run()


if __name__ == "__main__":
    my_sandbox(["rootfs/x86_windows/bin/Easy_CrackMe.exe"], "rootfs/x86_windows")
```

#### **Вывод**

```
0x10cae10: GetStartupInfo(0xffffdf40)
0x1121fa7: GetStdHandle(0xfffffff6) = 0xfffffff6
0x111fbc4: GetFileType(0xfffffff6) = 0x2
0x1121fa7: GetStdHandle(0xfffffff5) = 0xfffffff5
0x111fbc4: GetFileType(0xfffffff5) = 0x2
0x1121fa7: GetStdHandle(0xfffffff4) = 0xfffffff4
0x111fbc4: GetFileType(0xfffffff4) = 0x2
0x1121fd1: SetHandleCount(0x20) = 32
0x1121fbf: GetCommandLineA() = 0x501091b8
0x111fcd4: GetEnvironmentStringsW() = 0x501091e4
0x1117ffa: WideCharToMultiByte(0x0, 0x0, 0x501091e4, 0x1, 0x0, 0x0, 0x0, 0x0) = 2
0x1117ffa: WideCharToMultiByte(0x0, 0x0, 0x501091e4, 0x1, 0x50002098, 0x2, 0x0, 0x0) = 1
0x111fcbc: FreeEnvironmentStringsW(0x501091e4) = 1
0x1116a0b: GetACP() = 437
0x1121f8f: GetCPInfo(0x1b5, 0xffffdf44) = 1
0x1121f8f: GetCPInfo(0x1b5, 0xffffdf1c) = 1
0x111e43e: GetStringTypeW(0x1, 0x40541c, 0x1, 0xffffd9d8) = 0
0x10ffc95: GetStringTypeExA(0x0, 0x1, 0x405418, 0x1, 0xffffd9d8) = 0
0x111e39c: LCMapStringW(0x0, 0x100, 0x40541c, 0x1, 0x0, 0x0) = 0
0x1128a50: LCMapStringA(0x0, 0x100, 0x405418, 0x1, 0x0, 0x0) = 0
0x111e39c: LCMapStringW(0x0, 0x100, 0x40541c, 0x1, 0x0, 0x0) = 0
0x1128a50: LCMapStringA(0x0, 0x100, 0x405418, 0x1, 0x0, 0x0) = 0
0x111685a: GetModuleFileNameA(0x0, 0x40856c, 0x104) = 42
0x10cae10: GetStartupInfo(0xffffdfa0)
0x11169f3: GetModuleHandleA(0x00) = 400000
0x104cf42: DialogBoxParamA(0x400000, 0x65, 0x00, 0x401020, 0x00) = 0
Input DlgItemText :

        << enter any string or number here >>

0x1063d14: GetDlgItemTextA(0x00, 0x3e8, 0xffffdef4, 0x64) = 3
0x105ea11: MessageBoxA(0x00, "Congratulation !!", "EasyCrackMe", 0x40) = 2
0x1033ba3: EndDialog(0x00, 0x00) = 1
0x1124d12: ExitProcess(0x01)
```
