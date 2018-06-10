# 사인 그래프 그리기

~~~
import math

for i in range(0, 361, 15):
    signvalue = int(math.sin(math.pi + i / 180) * 20 + 25)
    if signvalue == 25:
        for j in range(25):
            print(" ", end="")
        print("*")
    elif signvalue < 25:
        # sign 표시를 위한 공백 떼기
        for j in range(signvalue-1):
            print(" ", end="")
        print("*", end="")
        
        # 축 표시하기
        for j in range(25-signvalue):
            print(" ", end="")
        print("|")
    else:
        # 축 표시하기
        for j in range(25):
            print(" ", end="")
        print("|", end="")
        # sign 표시를 위한 공백 떼기
        for j in range(signvalue-25):
            print(" ", end="")
        print("*")
~~~