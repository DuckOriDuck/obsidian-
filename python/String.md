# 문자열 생성
```python
x = 'hello'
y = "python"
# 따옴표 3개를 연속으로 쓰면 다중라인 문장도 입력이 가능하다
i = '''안녕하세요.
저는 위니브의 대표 이호준입니다.
파이썬 참 좋아요.
여러분 정말 잘 선택하셨어요.
'''
j = """안녕하세요.
저는 위니브의 대표 이호준입니다.
파이썬 참 좋아요.
여러분 정말 잘 선택하셨어요.
"""
```


# 문자열의 특징
## 덧셈과 곱셈
```python
x = 'hello'
y = 'world'
print(x + y) # 출력 : helloworld
```

```python
x = 'hello'
print(x * 3) # 출력 : hellohellohello
```
## 문자열 인덱싱
```python
s = '위니브 월드!'
print(s[0]) # 출력 : '위'
print(s[-1]) # 출력 : '!'
print(s[-2]) # 출력: '드'
```
## 다양한 슬라이싱 형태
아래의 형식이다
```python
string[start:end:step]
```
활용하면
```python
s = 'weniv CEO licat'
s = 'weniv CEO licat'
print(s[6:]) # 출력 : CEO licat -> start~ stop까지의 문자열
print(s[:]) # 출력 : weniv CEO licat -> 걍 그대로
print(s[::-1]) # 출력 : tacil OEC vinew -> 역순
print(s[::2]) # 출력 : wnvCOlct -> 2배수 인덱스 제외 스킵
```
- start 값 생략시 0, step값 생략시 -1이다.
- **슬라이싱은 인덱스가 범위를 벗어나도  error가 발생하지 않는다**
- 슬라이싱은 C 구현체로 돼 있어 메서드보다 빠릅니다.
## 문자열 메서드
### lower(), upper()
```python
s = 'weniv CEO licat'
s.lower() #전체를 소문자로 바꿔주는 method
s.upper() #전체를 대문자로 바꿔주는 method
```
전체 문자를 소문자, 대문자로 바꿔주는 메서드임.
### find(), index()
```python
s = 'weniv CEO licat'
s.find('CEO') #문자열을 찾아주는 method
s.index('CEO') # 해당 문자열이 시작되는 인덱스를 찾아줌

#찾는 값이 없다면
s.find('none') # -1
s.index('none') # error
```
### count( )
```python
s = 'weniv CEO licat'
s.count('i') #2 
```
### strip()
```python
s = ' weniv CEO licat '
s.strip() #양쪽 공백을 제거해준다.
```
## replace()
```python
str.replace(old, new, count) # 형식이다. count는 대체할 횟수
```

```python
s = 'weniv CEO licat'

s.replace('CEO', 'CTO') #원하는 문자열을 다른 문자열로 대체할 때 사용하는 method
# 출력 
'weniv CTO licat'
```

## split()/ join()
```python
#split
#기본 형식
string.split(separator, maxsplit)

#사용
s = 'weniv CEO licat' 
s.split() #공백을 기준으로 문자열 나누기

# 출력 
['weniv', 'CEO', 'licat']

#join
#기본형식
separator.join(iterable)
s = ['weniv', 'CEO', 'licat']
'-'.join(s) #리스트를 '-' 기준으로 하나의 문자열로 합침
```

## format()
```python
name = 'licat'
age = 29

pring('제 이름은 {}이고, 나이는 {}살입니다.'.format(name, age))
```

```python
name = 'licat'
age = 29

print('제 이름은 {}이고, 나이는 {}살입니다.'.format(name, age))

print('제 이름은 {1}이고, 나이는 {0}살입니다.'.format(age, name))
print('제 이름은 {0}이고, 나이는 {0}살입니다.'.format(age, name))
#이것과 같이 포멧에 있는 특정 스트링을 중복해서 나오게 할 수 ㅣㅇㅆ음
'''
제 이름은 weniv이고, 나이는 29살입니다.
제 이름은 29이고, 나이는 29살입니다.
'''
```

## isalnum( ) / isdigit( ) / isalpha( ) / isascii( )
```python
'abcd1234'.isalnum() # 출력: True
'안녕하세요'.isalnum() # 출력: True
'こんにちは'.isalnum() # 출력: True
#공백이나 특수문자가 있을때는 false, 알파벳, 숫자, 일본어, 중국어, 한국어는 true

```