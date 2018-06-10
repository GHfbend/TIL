# 간단한 게임

~~~
import random
count = 0
player1 = input('1 플레이어의 이름은? ')
player2 = input('2 플레이어의 이름은? ')
p1_place = 0
p2_place = 0
while True:
    count += 1
    p1_number = random.randint(1, 6)
    p2_number = random.randint(1, 6)
    p1_place += p1_number
    p2_place += p2_number
    print('%d 라운드 : %s은 %d가 나왔습니다. 현재 위치 %d \n %s은 %d가 나왔습니다. 현재 위치 %d' % (count, player1, p1_number, p1_place, player2, p2_number, p2_place))

    if p1_place >= 30:
        print('%d라운드만에 %s이 이겼습니다' % (count, player1))
        break

    elif p2_place >= 30:
        print('%d라운드만에 %s이 이겼습니다' % (count, player2))
        break
~~~