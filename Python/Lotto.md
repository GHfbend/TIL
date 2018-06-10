# 로또

~~~
import random
from collections import Counter
from operator import it

game = []

def lotto_generator():
        list = []

        for i in range(1, 7):
            list.append(random.randint(1, 45))

        while (list[0] == list[1]):
            list[1] = random.randint(1, 45)
        while (list[0] == list[2] or list[1] == list[2]):
            list[2] = random.randint(1, 45)
        while (list[0] == list[3] or list[1] == list[3] or list[2] == list[3]):
            list[3] = random.randint(1, 45)
        while (list[0] == list[4] or list[1] == list[4] or list[2] == list[4] or list[3] == list[4]):
            list[4] = random.randint(1, 45)
        while (list[0] == list[5] or list[1] == list[5] or list[2] == list[5] or list[3] == list[5] or list[4] == list[5]):
            list[5] = random.randint(1, 45)

        list.sort()
        game.append(list)
        print(list)

for j in range(1001):
    lotto_generator()

total = [y for x in game for y in x]

C = Counter((total))

sorted_C = sorted(C.items(), key=itemgetter(1), reverse=True)
sorted_C = sorted(sorted_C)
print(sorted_C)

thisweek_lotto_num = C.most_common(6)
thisweek_lotto = [thisweek_lotto_num[0][0], thisweek_lotto_num[1][0], thisweek_lotto_num[2][0], thisweek_lotto_num[3][0], thisweek_lotto_num[4][0], thisweek_lotto_num[5][0]]
print(thisweek_lotto)
~~~