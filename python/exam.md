# Python Workshop Exam

?> 悄悄滴进村，打枪的不要。

## 1. height 

考察基本语法。

```python
father_height = input('请输入父亲的身高：\n')
mother_height = input('请输入母亲的身高：\n')
kid_height = (float(father_height) + float(mother_height)) * 0.54
print('预测儿子身高为：')
print(kid_height)
```

## 2. guess

考察控制流。

```python
import random

answer = random.randint(1, 10)

print('——————猜数字游戏——————')
print('请输入1~10之间的任意一个整数：')

user_input = int(input())

while user_input != answer:
    if user_input < answer:
        user_input = print('太小，请重新输入：')
    else:
        user_input = print('太大，请重新输入：')

    if user_input == 0:
        break

    user_input = int(input())

if user_input == answer:
    print(f'恭喜你，你赢了，猜中的数字是：{answer}')

print('——————游戏结束——————')
```

## 3. idcard

考察切片和文本处理。

```python
print('——————身份证信息——————')
user_input = input('请输入身份证信息：\n')

birth_year = user_input[6:10]
birth_month = user_input[10:12]
birth_day = user_input[12:14]

birth_month = int(birth_month)
birth_day = int(birth_day)
print('您的生日是：')
print(f'{birth_year}年{birth_month}月{birth_day}日')

gender = int(user_input[17:18])
gender = '男' if gender % 2 != 0 else '女'
print('您的性别是：')
print(gender)
```

## 4. future

考察正则文本处理和日期函数。

```python
import re
from datetime import datetime, timedelta


def get_date_str(d):
    year, month, day = re.search(r'(\d{4})-(\d{2})-(\d{2})', str(d)).groups()
    weekday = d.strftime('%A')
    return f'{year}年{month}月{day}日 {weekday}'


print('——————未来星期几——————')
now = datetime.now()
print('今天是：')
print(get_date_str(now))

days_to_add = input('请输入未来天数：\n')
future_day = now + timedelta(days=int(days_to_add))
print('未来是：')
print(get_date_str(future_day))
```


## 5. book

考察数据库应用。

```python
import mysql.connector

db_config = {
    'username': 'root',
    'password': '',
    'database': 'BookDB',
    'use_unicode': True,
    'charset': 'utf8'
}


def db_query(config, sql):
    conn = mysql.connector.connect(**config)
    cursor = conn.cursor()
    cursor.execute(sql)
    result = cursor.fetchall()
    conn.close()
    return result


if __name__ == '__main__':
    sql = 'select name,price,date from book'
    result = db_query(db_config, sql)
    for name, price, date in result:
        date = date.strftime('%Y-%m-%d')
        print(f'图书：《{name}》，价格：¥{price}元，出版日期：{date}')

```

## 6. game

考察面向对象设计。

```python
class Person():
    def __init__(self, name, gender, age, capacity):
        self.name = name
        self.gender = gender
        self.age = age
        self.capacity = capacity
        self.actions = {
            '草丛战斗': {'power': -200, 'message': '参加一次草丛战斗'},
            '自我修炼': {'power': 100, 'message': '自我修炼了一次'},
            '多人游戏': {'power': -500, 'message': '参加一次多人游戏'},
        }
        self.print_info()

    def print_info(self):
        print(f'姓名: {self.name}; 性别: {self.gender} ; 年龄: {self.age} ; 战斗力：{self.capacity}')

    def take_action(self, action_name):
        action = self.actions[action_name]
        print(f'{self.name} {action["message"]}')
        self.capacity = self.capacity + action['power']


if __name__ == '__main__':
    print('——————游戏初始——————')
    bb = Person('冰冰', '女', 18, 1000)
    mm = Person('木木', '男', 20, 1900)
    ll = Person('幂幂', '女', 19, 2300)
    print('\n——————开始游戏——————')

    bb.take_action('多人游戏')
    mm.take_action('自我修炼')
    ll.take_action('草丛战斗')
    print('')

    bb.print_info()
    mm.print_info()
    ll.print_info()
```

## 7. lottery

考察内置包和逻辑算法。

```python
# MyModular.__init__.py
import random

def Great_lotter(count):
    for i in range(count):
        print(_generate_lotter())


def _generate_lotter():
    lotter = random.sample(range(1, 36), 5)
    check = random.sample(range(1, 13), 2)

    lotter = ' '.join([str(x).zfill(2) for x in lotter])
    check = ' '.join([str(x).zfill(2) for x in check])

    return f'{lotter}    {check}'

## lotter.py
from MyModular import Great_lotter

print('——————大乐透号码模拟生成器——————')
n = input('请输入要生成的大乐透号码注数：')
n = int(n)

Great_lotter(n)

```

## 8. blessing

考察模块调用和数据结构。

```python
# JiFu.__init__.py
import random

blessing_type = [
    '爱国福',
    '富强福',
    '和谐福',
    '友善福',
    '敬业福',
]

my_blessings = {
    '爱国福': 0,
    '富强福': 0,
    '和谐福': 0,
    '友善福': 0,
    '敬业福': 0,
}


def add_fu(fu):
    for k, v in my_blessings.items():
        if k == fu:
            my_blessings[k] = v + 1


def Ji_Fu():
    input('\n按下<Enter>键获取五福')
    index = random.randint(0, 4)
    fu = blessing_type[index]
    print(f'获取到：{fu}')
    add_fu(fu)


def fus():
    print('当前拥有的福：')
    out = [f'{k}: {v}' for k, v in my_blessings.items()]
    print(' '.join(out))


def five_blessings():
    return all(my_blessings.values())


if __name__ == '__main__':

    print('——————开始集福啦——————')

    while not five_blessings():
        Ji_Fu()
        fus()

    print('恭喜您集成五福！！！')


# wufu.py
from JiFu import Ji_Fu,fus,five_blessings

print('——————开始集福啦——————')

while not five_blessings():
    Ji_Fu()
    fus()

print('恭喜您集成五福！！！')
```