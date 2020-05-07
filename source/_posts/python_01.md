---
title: python_01
copyright: true
date: 2020-05-07 11:42:14
tags: python
categories: python
---


# 掷骰子小游戏
> 1. 身份验证模块：共3次认证次数，并包含验证码验证功能，随机生成4位由大小写字母
及数字组成的验证码，验证码验证失败需重新验证。
> 2. 登录成功后判断用户账户余额，若余额不足2游戏币，则需要充值。
> 3. 提示用户充值游戏币，（1块钱充值3游戏币，充值必须是1的倍数，充值不成功可以继续充值）
> 4. 玩一局游戏扣除两个币，猜大小（系统随机生成两个1-6的数字来模拟骰子的点数）
> 5. 猜测正确奖励一个币，并可以继续玩，猜测错误也可以继续玩，直到判断没有游戏币的时候提示充值，或用户输入quit退出游戏。



```
import random


print("+"*50)
print("+"*16,"欢迎来到掷骰子游戏","+"*16)
print("+"*50)
print()

###########################
#         用户定义         #
###########################
user1 = "j"
pass1 = "1"


###########################
money1 = 0
count = 3
###########################


###########################
#      4位验证码库定义      #
###########################
ku = '1Q2W3E4R5T6Y7U8I9O0PASDFGHJKLZXCVBNMqwaszxedcrfvtgbyhnujmiolp'


###########################
#       身份验证模块        #
###########################
for i in range(3):
    print("=" * 50)
    username = input("请输入你的用户名:").strip()
    print("=" * 50)
    password = input("请输入你的密码:").strip()
    print("=" * 50)
    if username == user1 and password == pass1:
        while True:
            code = ''
            for i in range(4):
                num = random.randint(1, len(ku) - 1)
                code += ku[num]
            print('验证码为：', code)
            print("=" * 50)
            yan = input('输入验证码：')
            print("=" * 50)
            if yan.lower() == code.lower():
                break
            else:
                print('验证码输入错误，请重新输入！')
                continue
        print("你的账户余额为:",money1)
        print("=" * 50)
        break
    else:
        count -= 1
        if count == 0:
            exit("验证次数已用完，退出游戏！")
        else:
            print("账号或密码错误，请重新输入")
            print("=" * 50)
            print('剩余输入次数为{}次'.format(count))
        continue
while True:
    ############################
    #         充值模块          #
    ############################
    while money1 < 2:
        yn = input("是否充值（y/n）:").strip()
        print("=" * 50)
        if yn == "y" or yn == "n":
            pass
        else:
            print("你的输入有误，请重新输入！")
            print("=" * 50)
            break
        if yn == "y":
            while money1 < 2:
                money_1 = input("请输入你的充值金额\n(充值金额必须大于1元且为整数，1元=3币):").strip()
                if  money_1.isdigit():
                    money_1 = int(money_1)
                else:
                    print("你的输入有误，请重新输入！")
                    print("=" * 50)
                    continue
                print("=" * 50)
                if money_1 >= 1 and money_1 % 1 == 0:
                    money1 = money1 + money_1 * 3
                    print("充值成功，你的剩余游戏币为:", money1)
                    print("=" * 50)
                    break
                else:
                    print("你的输入有误，请重新输入！")
                    print("=" * 50)
                    continue
        else:
            exit("账户余额不足")
    ############################
    #         游戏模块          #
    ############################
    while money1 >= 2:
        come = input("是否进入游戏（y/n）").strip()
        print("=" * 50)
        if come == "y":
            print("正在进入游戏，请稍后>>>")
            print("=" * 50)
            while True:
                t1 = random.randint(1, 6)
                t2 = random.randint(1, 6)
                cai = input("请选择你押注点数的大小\n（压大输入“1”，压小输入“2”,退出游戏输入“quit”）:").strip()
                print("=" * 50)
                if cai == "quit":
                    exit("游戏已退出！")
                if cai == "1" or cai == "2":
                    cai = int(cai)
                else:
                    print("请输入正确的数字")
                    print("=" * 50)
                    continue
                if (cai == 1 and t1 + t2 <= 6) or (cai == 2 and t1 + t2 > 6):
                    print("不好意思，你猜错了！")
                    print("两个骰子的点数分别为:", t1, " ", t2)
                    print("@" * 50)
                    money1 -= 1
                    print("你当前的余额为:", money1)
                    print("@" * 50)
                    if money1 < 2:
                        print("你的余额不足，请充值！")
                        print("@" * 50)
                    break

                else:
                    print("两个骰子的点数分别为:", t1, " ", t2)
                    print("@" * 50)
                    print("恭喜你猜对啦！您将获得一个游戏币，系统会自动存进你的账户")
                    print("@" * 50)
                    money1 += 1
                    print("你当前的余额为:", money1)
                    print("@" * 50)
                    continue
        elif come == "n":
            exit("退出游戏！")
        else:
            print("你的输入有误，请重新输入！")
            break
```