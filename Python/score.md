# 성적처리

- 코드 수정이 필요함(미완성)
- 파일입출력 응용

~~~
import csv

f = open("C:/score.csv", 'r', encoding='utf-8')

rank = []
lines = f.readline()

while True:
    lines = f.readline()
    if not lines:break
    
    adata = lines.split(',')
    name = int(adata[0])
    kor = int(adata[1])
    eng = int(adata[2])
    math = int(adata[3])
    
    total = kor + eng + math
    
    rank.append(total)
    rank.sort(reverse=True)
    ranking = rank.index(total) + 1
    
    print("번호", "국어", "영어", "수학", "총점", "석차")
    print("%3d %5d %4d %3d %5d %3d" % (name, kor, eng, math, total, ranking))
    
f.close()
~~~