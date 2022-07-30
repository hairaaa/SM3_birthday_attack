# SM3_birthday_attack
SM3简易生日攻击：

简化操作，随机生成256个128位16进制数串（512bit），在这个集合里随机找两个串进行碰撞，比对其杂凑值的前8bit。

def random_hex(length):#生成长度为length的十六进制串
    result =hex(random.randint(0,16**length)).replace('0x','').upper()
    if(len(result)<length):
        result ='0'*(length-len(result))+result
    return result
    
def getRandomList(n):#随机生成n个串放在一个列表里
    numbers = []
    while len(numbers) < n:
        i = random_hex(128)
        if i not in numbers:
            numbers.append(i)
    return numbers
    

def brithAttack():#随机生成两个长度为128x4bit的数据进行生日攻击使得两串数据在进行了sm3之后杂凑值相同
    #在此只进行简单的操作，比对杂凑值的前8bit
    # 初始化两个列表
    list_x = []
    # 生成 sqrt(p) 长度的随机数集合
    list_x = getRandomList(256)
    for i in range(128):
        # 计算出它们的杂凑值
        message1 = list_x[i]
        message2 = list_x[i+128]
        m1 = Fill(message1)  # 填充后消息
        m2 = Fill(message2)  # 填充后消息
        M1 = Group(m1)  # 数据分组
        M2 = Group(m2)  # 数据分组
        Vn1 = Iterate(M1)
        Vn2 = Iterate(M2)  # 迭代
        result1 = ''
        for x in Vn1:
            result1 += (hex(x)[2:] + ' ')
        result2 = ''
        for y in Vn2:
            result2 += (hex(y)[2:] + ' ')
        print(result1[:4])
        print(result2[:4])
        if(result1[:2]==result2[:2]):
            print("找到两组数据的杂凑值相同")
            print("1:", message1)
            print("2:", message2)
            print("杂凑值前8位为", result1[:2])
            print("杂凑值前8位为", result2[:2])
            return 1
            
            
结果为：
![image](https://user-images.githubusercontent.com/104775629/181994483-f385a474-2506-43c6-b7df-050ebf070b15.png)
