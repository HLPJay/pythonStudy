###windows上python第三方库的安装：
第三方库获取路径 https://www.lfd.uci.edu/~gohlke/pythonlibs/
下载对应的第三方库whl
解压  然后放进python安装处 D:\Program Files (x86)\Python\Lib\site-packages
*注意有的第三方库要主要版本对应**

### 基础练习：

```
for i in range(1, 21):
    print('apple'[i%3*len('apple')::]+'orange'[i%5*len('orange')::] or i)
```
```
def replace_num(i):
    if i%3==0 and i%5==0:
        return "appleorange"
    if i%3==0:
        return 'apple'
    if i%5==0:
        return 'orange'
    else:
        return i
for i in range(1, 21):
    print(replace_num(i))
```

### 字符串练习：

```
names=(' Kunpen Ji, Li XIAO, Caron Li, Donl SHI, Ji ZHAO,Fia YUAN Y, Weue\
DING, Xiu XU, Haiying WANG, Hai LIN,Jey JIANG, Joson WANG E, Aiyang ZHANG,Hay\
MENG, Jak ZHANG E, Chang Zhang, Coro ZHANG')

def sort_names(names):
    return (sorted([name.strip() for name in names.split(',')]))

def get_chinese_names(names):
    chinese_names = []
    for name in sort_names(names):
        if len(name.split())>=2:
            first_name=name.split()[0].capitalize()
            last_name=name.split()[1].capitalize()
            chinese_names.append(last_name+' '+first_name)
        else:
            chinese_names.append(name)
    return chinese_names

def find_name(names, key='Zhang'):
    return [name for name in get_chinese_names(names) if name.startswith(key)]

print(sort_names(names))
print("********************")
print(get_chinese_names(names))
print("********************")
print(find_name(names, key='Zhang'))
```

### 猜数字游戏
```
import random
def guess_num():
    max_retry=5
    i=0
    random_num=random.randint(1,21)
    while i<max_retry:
        try:
            num=int(input("please input num :1-20\n"))
            print (f'Your input is :{num}')
            if num>random_num:
                print ('>>Biger')
            elif num<random_num:
                print ('>>Small')
            else:
                print ('!!Great,you guess the num!')
                break
        except Exception as e:
            print (f'Your input incorrect,Please input num 1-20\n')
        finally:
            i+=1
            print (f'retry time:{max_retry-i}')
    else:
        print ('Your retry time>5,Game over!')

guess_num()
```

###九宫格

```
import itertools

# 获取所有的排序方式
def JiuGongGe():
    nums = [x for x in range(1,10)]
    # seq_3nums = [p for p in itertools.permutations(nums, 3)]

    seq_3nums = [p for p in itertools.permutations(nums,3) if sum(p)==15]

    matrix = []
    for row1_1,row1_2,row1_3 in seq_3nums:
        for row2_1,row2_2,row2_3 in seq_3nums:
            for row3_1,row3_2,row3_3 in seq_3nums:
                if row1_1+row1_2+row1_3==15 \
                    and row1_1+row2_2+row3_3==15 \
                    and row1_1+row2_1+row3_1==15 \
                    and row1_3+row2_2+row3_1==15 \
                    and row1_2+row2_2+row3_2==15:

                    row1=[row1_1,row1_2,row1_3]
                    row2=[row2_1,row2_2,row2_3]
                    row3=[row3_1, row3_2, row3_3]
                    print(str(set(row1)) +"   "+str(set(row2)))
                    if len(set(row1) & set(row2)) == 0:#取交集 和三个数加起来为15
                        if row1 not in matrix:
                            matrix = [row1, row2, row3]
                            print (matrix)
                        print ('-'*100)

JiuGongGe()
```

### 海龟turtle画花

```
import turtle
def draw_diamond(turtle):
    for i in range(1,3):
        turtle.forward(100)
        turtle.right(45)
        turtle.forward(100)
        turtle.right(135)

def draw_stem(turtle):
    turtle.seth(-90)
    turtle.forward(700)

def draw_art():
    window=turtle.Screen()
    window.bgcolor("blue")

    brad=turtle.Turtle()
    brad.shape("turtle")
    brad.color("orange")
    brad.speed('fast')

    # num = 360%10
    for _ in range(1,37):
        draw_diamond(brad)
        brad.right(10)
    draw_stem(brad)
    window.exitonclick()

draw_art()
```

###文件操作
```
import os
def get_file_size(file):
    if os.path.exists(file):
        return os.path.getsize(file)
    else:
        return 0

def statist_files(path):
    files = []
    for dirpath,dirnames,filenames in os.walk(path):
        for file_name in filenames:
            files.append(os.path.join(dirpath,file_name))

    return [{'name':f,'size':get_file_size(f)} for f in files]

def count_files_size(files_info,size_display_mode='M'):
    sizes=[file_info.get('size',0) for file_info in files_info]
    if size_display_mode=='K' or size_display_mode=='k':
        return str(round(sum(sizes)/1024,3))+'K'
    elif size_display_mode=='M' or size_display_mode=='m':
        return str(round(sum(sizes) / (1024*1024), 3)) + 'M'
    elif size_display_mode=='G' or size_display_mode=='g':
        return str(round(sum(sizes) / (1024 * 1024 *1024), 3)) + 'G'
    else:
        return str(round(sum(sizes)))+'Byte'

def get_sorted_files(files):
    sorted_files=sorted(files,key=lambda x:x['size'],reverse=True)
    return sorted_files

path = 'C:/Users/Administrator/Desktop'
print(count_files_size(statist_files(path)))
print(count_files_size(statist_files(path), 'G'))
print(count_files_size(statist_files(path), 'K'))
print(count_files_size(statist_files(path), 'B'))
print(get_sorted_files(statist_files(path)))
```

###实现简单的计算器
```
import re
def verify_num(n):
    patt=re.compile(r'^(-?\d+)(\.\d+)?$')
    return True if re.match(patt,n) else False

def verify_opt(opt):
    return opt in ['+','-','*','/']

def mini_calculator():
    my_input=input('Please two nums and operation,such as 1,2,+:\n')
    try:
        args=my_input.split(',')
        if len(args)==3:
            a,b,opt=args
            if not verify_num(str(a)) or not verify_num(str(b)) or not verify_opt(opt):
                print ('Input format is incorrect!')
                return
            res=eval('{}{}{}'.format(a,opt,b))
            print ('{}{}{}={}'.format(a,opt,b,res))
        else:
            print ('Args len should be 3!')
    except ValueError as e:
        print ('Value error',e)
    except Exception as e:
        print (e)
mini_calculator()
```

###cvs和Excel互相转换

###生日练习
```
import calendar
import datetime
import time
import collections

def get_birthday_weekday(birthday_str):
    weekday=datetime.datetime.strptime(birthday_str,'%Y-%m-%d').weekday()
    weekdays=['Mon','Tue','Wes','Thur','Fri','Sat','Sun']
    return weekdays[weekday]

def internal_days(name,your_birthday):
    try:
        birthday=datetime.datetime.strptime(your_birthday, '%Y-%m-%d')
        delta_days=(birthday-datetime.datetime.today()).days
        if delta_days>0:
            return (f'距离{name}的生日还有 {delta_days} 天')
        elif delta_days<0:
            return (f'距离{name}的生日还有 {abs(delta_days)} 天')
        else:
            return (f'今天是{name}的生日，生日快乐!')
    except ValueError as e:
        print ('Please input the birthday format as :1991-2-3')

def statist_birthday():
    birthday_list=[str(y)+'-1-1' for y in range(1990,2020)]
    res=[]
    for b in birthday_list:
        res.append(get_birthday_weekday(b))
        print (collections.Counter(res))

if __name__=='__main__':
    statist_birthday()
    print(internal_days('TE#S', '1992-11-07'))
    pass
```

###获取有道翻译上单词的意思
```
from requests import ConnectTimeout
from pyquery import PyQuery as pq
from concurrent.futures import ThreadPoolExecutor
import requests

COUNT = 1
def parse_html(doc):
    proc_text = ''
    desc_text = ''
    for pro in doc.items('.baav .pronounce'):
        proc_text += pro.text()
    for li in doc.items('#phrsListTab .trans-container ul li'):
        desc_text += li.text()
    return {'Proc': proc_text, 'Desc': desc_text}

def translate_word(word):
    global COUNT
    output = {}
    url = 'http://dict.youdao.com/w/eng/{}/'.format(word)
    print('{},Fetch:{}'.format(COUNT, url))
    COUNT += 1
    headers = {
        'Cookie': 'DICT_UGC=be3af0da19b5c5e6aa4e17bd8d90b28a|;\
webDict_HdAD=%7B%22req%22%3A%22http%3A//dict.youdao.com%22%2C%22\
width%22%3A960%2C%22height%22%3A240%2C%22showtime%22%3A5000%2C%22\
fadetime%22%3A500%2C%22notShowInterval%22%3A3%2C%22notShowInDays\
%22%3Afalse%2C%22lastShowDate%22%3A%22Mon\
%20Nov%2008%202010%22%7D; \
 ___rl__test__cookies=1515809612366; \
_ntes_nnid=7c7071e6ff37a1f321a28ebd3450279f,1503241340929; \
OUTFOX_SEARCH_USER_ID_NCOO=1630604769.092356; \
OUTFOX_SEARCH_USER_ID=-224682605@10.169.0.76; \
JSESSIONID=abcr9OlYVoIM8dOeXsTdw', \
        'Host': 'dict.youdao.com', \
        'Upgrade-Insecure-Requests': '1', \
        'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) \
AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36'
    }
    try:
        r = requests.get(url, headers=headers)
        if r.status_code == 200:
            doc = pq(r.text)
            info = parse_html(doc)
            output['Word'] = word
            output = dict(output, **info)

    except ConnectTimeout as e:
        print('Get url:{} timeout'.format(url))
    except ConnectionError as e:
        print('Get url:{} error'.format(url))
    except Exception as e:
        print(e)
    return output

def async_task(words):
    pool = ThreadPoolExecutor(4)
    threads = [pool.submit(translate_word, (word)) for word in words]
    for t in threads:
        print(t.result())
words = ['china', 'english', 'temperaments']
async_task(words)

```