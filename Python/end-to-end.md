# 끝말잇기

- 간단한 프로그램(지속적 업데이트 예정)
- 사전 파일 활용을 통한 존재하지 않는 단어 식별

~~~
f = open("C:/dict_exam.txt", 'r')
lines = f.readlines()
a = list()
start = 'apple'
user_answer = input('%s 끝말 잇기? ' % start)
if user_answer + '\n' in lines:
    print('존재하는 단어입니다.')
else:
    print('존재하지 않는 단어입니다.')
while True:
    if len(user_answer) == 5:
        a.append(user_answer)
        user_answer = input('%s 끝말 잇기? ' % user_answer)
    elif len(user_answer) > 5:
        print('단어가 길어요.')
        user_answer = input('%s 끝말 잇기?' % a[-1])
    elif len(user_answer) < 5:
        print('단어가 짧아요.')
        user_answer = input('%s 끝말 잇기?' % a[-1])
    if user_answer in a:
        print('이미 입력한 단어입니다.')
        user_answer = input('%s 끝말 잇기?' % a[-1])
    if user_answer+'\n' in lines:
        print('존재하는 단어입니다.')
    else:
        print('존재하지 않는 단어입니다.')
f.close()
~~~