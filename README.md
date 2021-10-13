# DFS&BFS, SAMSUNG SW 문제
  
  
  
## Day 1 (2021-08-31)

### **백준 14503번**  

**문제**
로봇 청소기가 주어졌을 때, 청소하는 영역의 개수를 구하는 프로그램을 작성하시오.

로봇 청소기가 있는 장소는 N×M 크기의 직사각형으로 나타낼 수 있으며, 1×1크기의 정사각형 칸으로 나누어져 있다. 각각의 칸은 벽 또는 빈 칸이다. 청소기는 바라보는 방향이 있으며, 이 방향은 동, 서, 남, 북중 하나이다. 지도의 각 칸은 (r, c)로 나타낼 수 있고, r은 북쪽으로부터 떨어진 칸의 개수, c는 서쪽으로 부터 떨어진 칸의 개수이다.

로봇 청소기는 다음과 같이 작동한다.

1. 현재 위치를 청소한다.
2. 현재 위치에서 현재 방향을 기준으로 왼쪽 방향부터 차례대로 인접한 칸을 탐색한다.
  a. 왼쪽 방향에 아직 청소하지 않은 공간이 존재한다면, 그 방향으로 회전한 다음 한 칸을 전진하고 1번부터 진행한다.
  b. 왼쪽 방향에 청소할 공간이 없다면, 그 방향으로 회전하고 2번으로 돌아간다.
  c. 네 방향 모두 청소가 이미 되어있거나 벽인 경우에는, 바라보는 방향을 유지한 채로 한 칸 후진을 하고 2번으로 돌아간다.
  d. 네 방향 모두 청소가 이미 되어있거나 벽이면서, 뒤쪽 방향이 벽이라 후진도 할 수 없는 경우에는 작동을 멈춘다.
* 로봇 청소기는 이미 청소되어있는 칸을 또 청소하지 않으며, 벽을 통과할 수 없다.

**입력**
첫째 줄에 세로 크기 N과 가로 크기 M이 주어진다. (3 ≤ N, M ≤ 50)

둘째 줄에 로봇 청소기가 있는 칸의 좌표 (r, c)와 바라보는 방향 d가 주어진다. d가 0인 경우에는 북쪽을, 1인 경우에는 동쪽을, 2인 경우에는 남쪽을, 3인 경우에는 서쪽을 바라보고 있는 것이다.

셋째 줄부터 N개의 줄에 장소의 상태가 북쪽부터 남쪽 순서대로, 각 줄은 서쪽부터 동쪽 순서대로 주어진다. 빈 칸은 0, 벽은 1로 주어진다. 지도의 첫 행, 마지막 행, 첫 열, 마지막 열에 있는 모든 칸은 벽이다.

로봇 청소기가 있는 칸의 상태는 항상 빈 칸이다.

**출력**
로봇 청소기가 청소하는 칸의 개수를 출력한다.

---

**풀이**

``` n,m = map(int, input().split()) # 세로크기, 가로크기
r,c,d = map(int,input().split()) # (r,c)좌표와 방향d
s = [list(map(int,input().split())) for _ in range(n)] # 지도 데이터

### 0 -> 북쪽 = 왼쪽방향 적용시 '서쪽' 3
1 -> 동쪽 = 왼쪽방향 적용시 '북쪽' 0
2 -> 남쪽 = 왼쪽방향 적용시 '동쪽' 1
3 -> 서쪽 = 왼쪽방향 적용시 '남쪽' 2 ###

dx = [-1, 0, 1, 0] # y축이동
dy = [0, 1, 0, -1] # x축이동

def turn_left(d):
  global d
  d = (d-1) % 4 # 위 조건 성립
  
x,y = r,c 
s[x][y] = 2 # 청소함(2)
count = 1 # 청소기 사용횟수

while True:
    check = False # check 함수로 구분
    for i in range(4):
      turn_left()
      nx = x + dx[d]
      ny = y + dy[d]
      if 0<=nx<n and 0<=ny<m and s[nx][ny] == 0:
        s[nx][ny] = 2 # a번 조항
        x,y = nx,ny
        count += 1 # 청소기 이동수 추가
        check = True # 아래 식 continue와 비슷한 기능
        break
    if not check:
      nx = x - dx[d]
      ny = y - dy[d]
      if 0<=nx<n and 0<=ny<m:
        if s[nx][ny] == 2:
          x,y = nx, ny # 안맞아서 c번조항에 따름
        elif s[nx][ny] == 1:
          print(count) # d번조항
          break
      else:
        print(count)
        break # d번조항 
```
        
--

## Day 2 (2021-09-01)

### **백준 1260번**  

**문제**  
그래프를 DFS로 탐색한 결과와 BFS로 탐색한 결과를 출력하는 프로그램을 작성하시오. 단, 방문할 수 있는 정점이 여러 개인 경우에는 정점 번호가 작은 것을 먼저 방문하고, 더 이상 방문할 수 있는 점이 없는 경우 종료한다. 정점 번호는 1번부터 N번까지이다.  

**입력**  
첫째 줄에 정점의 개수 N(1 ≤ N ≤ 1,000), 간선의 개수 M(1 ≤ M ≤ 10,000), 탐색을 시작할 정점의 번호 V가 주어진다. 다음 M개의 줄에는 간선이 연결하는 두 정점의 번호가 주어진다. 어떤 두 정점 사이에 여러 개의 간선이 있을 수 있다. 입력으로 주어지는 간선은 양방향이다.  

**출력**  
첫째 줄에 DFS를 수행한 결과를, 그 다음 줄에는 BFS를 수행한 결과를 출력한다. V부터 방문된 점을 순서대로 출력하면 된다. 

--

**풀이**  

``` n,m,v = map(int,input().split())
visit = [0 for _ in range(n+1)]
s = [[0]*(n+1) for _ in range(n+1)]
for _ in range(m):
  x,y = map(int,input().split())
  s[x][y] = 1
  s[y][x] = 1 # branch는 x,y/y,x 의 그래프에 입력하여 그림

def dfs(v):
  print(v, end=' ')
  visit[v] = 1
  for i in range(1, n+1):
    if visit[i] == 0 and s[v][i] == 1:
      dfs(i)

def bfs(v):
  q = [v]
  visit[v] = 0
  while(q):
    v = q[0]
    print(v, end=' ')
    del q[0]
    for i in range(1, n+1):
      if visit[i] == 1 and s[v][i] == 1:
        visit[i] = 0
        q.append(i) # dfs와 달리 바로 다음으로 넘어가지 않고 끝처리 마무리 후 

dfs(v)
print() # 한칸 뛰기
bfs(v) 
```

---

## Day 3 (2021-09-02)

### **백준 2178번**  

**문제**  
N×M크기의 배열로 표현되는 미로가 있다.  

1	0	1	1	1	1  
1	0	1	0	1	0  
1	0	1	0	1	1  
1	1	1	0	1	1  

미로에서 1은 이동할 수 있는 칸을 나타내고, 0은 이동할 수 없는 칸을 나타낸다. 이러한 미로가 주어졌을 때, (1, 1)에서 출발하여 (N, M)의 위치로 이동할 때 지나야 하는 최소의 칸 수를 구하는 프로그램을 작성하시오. 한 칸에서 다른 칸으로 이동할 때, 서로 인접한 칸으로만 이동할 수 있다.  

위의 예에서는 15칸을 지나야 (N, M)의 위치로 이동할 수 있다. 칸을 셀 때에는 시작 위치와 도착 위치도 포함한다.  

**입력**  
첫째 줄에 두 정수 N, M(2 ≤ N, M ≤ 100)이 주어진다. 다음 N개의 줄에는 M개의 정수로 미로가 주어진다. 각각의 수들은 붙어서 입력으로 주어진다.  

**출력**  
첫째 줄에 지나야 하는 최소의 칸 수를 출력한다. 항상 도착위치로 이동할 수 있는 경우만 입력으로 주어진다.  

**풀이**

```
from collections import deque # 임포트 deque 함(bfs는 거의 필수)

n,m = map(int,input().split())
s = [list(map(int, input())) for _ in range(n)] # 지도 그리기 문자열이 붙여서 나오므로 split는 생략!
v = [[0]*m for _ in range(n)] # 2차원 방문확인

dx = [-1, 1, 0, 0] # x축이동
dy = [0, 0, -1, 1] # y축이동

def bfs():
  q = deque()
  q.append((0,0))
  v[0][0] = 1
  while q:
    x,y = q.popleft()
    for i in range(4):
      nx = x + dx[i]
      ny = y + dy[i]
      if 0<=nx<n and 0<=ny<m and v[nx][ny] == 0 and s[nx][ny] == 1: # 범위안에서 방문x, 1로 쓰여진 지도값 찾기
        v[nx][ny] = 1
        s[nx][ny] = s[x][y] + 1
        q.append((nx, ny)) # dfs라면 여기서 함수값이 들어가지만 bfs는 그전 트리가 처리된 이후에 한다!
bfs()
print(s[n-1][m-1])
```

--

## Day 3 (2021-09-03)

### **백준 2606번**  

**문제**  
신종 바이러스인 웜 바이러스는 네트워크를 통해 전파된다. 한 컴퓨터가 웜 바이러스에 걸리면 그 컴퓨터와 네트워크 상에서 연결되어 있는 모든 컴퓨터는 웜 바이러스에 걸리게 된다.  

예를 들어 7대의 컴퓨터가 <그림 1>과 같이 네트워크 상에서 연결되어 있다고 하자. 1번 컴퓨터가 웜 바이러스에 걸리면 웜 바이러스는 2번과 5번 컴퓨터를 거쳐 3번과 6번 컴퓨터까지 전파되어 2, 3, 5, 6 네 대의 컴퓨터는 웜 바이러스에 걸리게 된다. 하지만 4번과 7번 컴퓨터는 1번 컴퓨터와 네트워크상에서 연결되어 있지 않기 때문에 영향을 받지 않는다.  



어느 날 1번 컴퓨터가 웜 바이러스에 걸렸다. 컴퓨터의 수와 네트워크 상에서 서로 연결되어 있는 정보가 주어질 때, 1번 컴퓨터를 통해 웜 바이러스에 걸리게 되는 컴퓨터의 수를 출력하는 프로그램을 작성하시오.  

**입력**  
첫째 줄에는 컴퓨터의 수가 주어진다. 컴퓨터의 수는 100 이하이고 각 컴퓨터에는 1번 부터 차례대로 번호가 매겨진다. 둘째 줄에는 네트워크 상에서 직접 연결되어 있는 컴퓨터 쌍의 수가 주어진다. 이어서 그 수만큼 한 줄에 한 쌍씩 네트워크 상에서 직접 연결되어 있는 컴퓨터의 번호 쌍이 주어진다.  

**출력**  
1번 컴퓨터가 웜 바이러스에 걸렸을 때, 1번 컴퓨터를 통해 웜 바이러스에 걸리게 되는 컴퓨터의 수를 첫째 줄에 출력한다.  

**풀이**

```
from collections import deque

n=int(input())
m=int(input())
v = [0 for _ in range(n+1)]
s = [[0]*(n+1) for _ in range(n+1)]
for i in range(m):
  x,y = map(int,input().split())
  s[x][y] = 1
  s[y][x] = 1

r = [] # dfs문제에서 해당 조건에 맞는 값을 리스트'r'에 추가한다.
def dfs(a):
  v[a] = 1
  for i in range(n+1):
        if s[a][i] == 1 and v[i] == 0:
            r.append(i)
            dfs(i)
  return len(r) # 길이 값을 리턴함

print(dfs(1))

```

--

## Day 4 (2021-09-04)

### **백준 2667번**  

**문제**  
<그림 1>과 같이 정사각형 모양의 지도가 있다. 1은 집이 있는 곳을, 0은 집이 없는 곳을 나타낸다. 철수는 이 지도를 가지고 연결된 집의 모임인 단지를 정의하고, 단지에 번호를 붙이려 한다. 여기서 연결되었다는 것은 어떤 집이 좌우, 혹은 아래위로 다른 집이 있는 경우를 말한다. 대각선상에 집이 있는 경우는 연결된 것이 아니다. <그림 2>는 <그림 1>을 단지별로 번호를 붙인 것이다. 지도를 입력하여 단지수를 출력하고, 각 단지에 속하는 집의 수를 오름차순으로 정렬하여 출력하는 프로그램을 작성하시오.  

**입력**  
첫 번째 줄에는 지도의 크기 N(정사각형이므로 가로와 세로의 크기는 같으며 5≤N≤25)이 입력되고, 그 다음 N줄에는 각각 N개의 자료(0혹은 1)가 입력된다.  

**출력**  
첫 번째 줄에는 총 단지수를 출력하시오. 그리고 각 단지내 집의 수를 오름차순으로 정렬하여 한 줄에 하나씩 출력하시오.  

**풀이**

```
from collections import deque

n = int(input())
s = [list(map(int,input())) for _ in range(n)] # 지도그리기
v = [[0]*n for _ in range(n)] # 방문기록
dx = [-1, 1, 0, 0] # x축 이동
dy = [0, 0, -1, 1] # y축 이동

def dfs(cnt, i, j): # cnt를 추가하는 이유 - 단지수를 재기 위해 매개변수를 추가함 / dfs로 추가하면 편함
  q = deque()
  q.append((i,j))
  v[i][j] = 1
  while q:
    x,y = q.popleft()
    for i in range(4):
      nx = x + dx[i]
      ny = y + dy[i]
      if 0<=nx<n and 0<=ny<n and s[nx][ny] and v[nx][ny] == 0:
        v[nx][ny] = 1
        cnt = dfs(cnt+1, nx, ny) # 단지수를 기록하기 위해
  return cnt # 최종적으로 단지수로만 출력하기 위함

dab = [] # 단지 수 기록하는 리스트
for i in range(n):
  for j in range(n):
    if s[i][j] == 1 and v[i][j] == 0:
      dab.append(bfs(1, i, j))

print(len(dab)) # 총 갯수
for i in sorted(dab): # 단지 수
  print(i)
  
```

-----

## Day 4 (2021-09-07)

### **백준 7569번**  

**문제**  
철수의 토마토 농장에서는 토마토를 보관하는 큰 창고를 가지고 있다. 토마토는 아래의 그림과 같이 격자모양 상자의 칸에 하나씩 넣은 다음, 상자들을 수직으로 쌓아 올려서 창고에 보관한다.  



창고에 보관되는 토마토들 중에는 잘 익은 것도 있지만, 아직 익지 않은 토마토들도 있을 수 있다. 보관 후 하루가 지나면, 익은 토마토들의 인접한 곳에 있는 익지 않은 토마토들은 익은 토마토의 영향을 받아 익게 된다. 하나의 토마토에 인접한 곳은 위, 아래, 왼쪽, 오른쪽, 앞, 뒤 여섯 방향에 있는 토마토를 의미한다. 대각선 방향에 있는 토마토들에게는 영향을 주지 못하며, 토마토가 혼자 저절로 익는 경우는 없다고 가정한다. 철수는 창고에 보관된 토마토들이 며칠이 지나면 다 익게 되는지 그 최소 일수를 알고 싶어 한다.  

토마토를 창고에 보관하는 격자모양의 상자들의 크기와 익은 토마토들과 익지 않은 토마토들의 정보가 주어졌을 때, 며칠이 지나면 토마토들이 모두 익는지, 그 최소 일수를 구하는 프로그램을 작성하라. 단, 상자의 일부 칸에는 토마토가 들어있지 않을 수도 있다.  

**입력**  
첫 줄에는 상자의 크기를 나타내는 두 정수 M,N과 쌓아올려지는 상자의 수를 나타내는 H가 주어진다. M은 상자의 가로 칸의 수, N은 상자의 세로 칸의 수를 나타낸다. 단, 2 ≤ M ≤ 100, 2 ≤ N ≤ 100, 1 ≤ H ≤ 100 이다. 둘째 줄부터는 가장 밑의 상자부터 가장 위의 상자까지에 저장된 토마토들의 정보가 주어진다. 즉, 둘째 줄부터 N개의 줄에는 하나의 상자에 담긴 토마토의 정보가 주어진다. 각 줄에는 상자 가로줄에 들어있는 토마토들의 상태가 M개의 정수로 주어진다. 정수 1은 익은 토마토, 정수 0 은 익지 않은 토마토, 정수 -1은 토마토가 들어있지 않은 칸을 나타낸다. 이러한 N개의 줄이 H번 반복하여 주어진다.  

토마토가 하나 이상 있는 경우만 입력으로 주어진다.  

**출력**  
여러분은 토마토가 모두 익을 때까지 최소 며칠이 걸리는지를 계산해서 출력해야 한다. 만약, 저장될 때부터 모든 토마토가 익어있는 상태이면 0을 출력해야 하고, 토마토가 모두 익지는 못하는 상황이면 -1을 출력해야 한다.  

**풀이**

```
from collections import deque

m,n,h = map(int, input().split())
s = [[list(map(int, input().split())) for _ in range(n)] for _ in range(h)] # 지도그리기
v = [[[0]*m for _ in range(n)] for _ in range(h)] # 방문기록
dx = [-1, 1, 0, 0, 0, 0] # x축
dy = [0, 0, -1, 1, 0, 0] # y축
dz = [0, 0, 0, 0, -1, 1] # z축
q = deque() # 각자 따른곳에서 동시에 출발하기에 미리 deque를 해놓는다.

def bfs():
  while q:
    x,y,z = q.popleft()
    for i in range(6):
      a = x + dx[i]
      b = y + dy[i]
      c = z + dz[i]
      if 0<=a<n and 0<=b<m and 0<=c<h and s[c][a][b] == 0 and v[c][a][b] == 0:
        v[c][a][b] = 1
        s[c][a][b] = s[z][x][y] + 1
        q.append((a, b, c))

for k in range(h):
  for i in range(n):
    for j in range(m):
      if s[k][i][j] == 1:
        q.append((i, j, k)) # 바로 bfs를 하지 않는 이유는 1이 하나에서 출발하는게 아닌 여러곳부터 출발하기에 q에 s가 1인 값을 다 넣어준다.

bfs()
isTrue = True
max_num = 0
for k in range(h):
  for i in range(n):
    for j in range(m):
      if s[k][i][j] == 0:
        isTrue = False # 0값이 있는지 체크하는 것
      max_num = max(max_num, s[k][i][j]) # max(map(max, s))를 하면 type error다

print(max_num-1 if isTrue else -1)
```

위 코드는 다만 문제되는 것이 있다면 초기 append값에 v가 1로 처리되지 않았다는 것이 걸린다. 하지만 진행에 문제가 없다.

------

## Day 5 (2021-09-08)

###**백준 1697번**  

**문제**  
수빈이는 동생과 숨바꼭질을 하고 있다. 수빈이는 현재 점 N(0 ≤ N ≤ 100,000)에 있고, 동생은 점 K(0 ≤ K ≤ 100,000)에 있다. 수빈이는 걷거나 순간이동을 할 수 있다. 만약, 수빈이의 위치가 X일 때 걷는다면 1초 후에 X-1 또는 X+1로 이동하게 된다. 순간이동을 하는 경우에는 1초 후에 2 * X의 위치로 이동하게 된다.  

수빈이와 동생의 위치가 주어졌을 때, 수빈이가 동생을 찾을 수 있는 가장 빠른 시간이 몇 초 후인지 구하는 프로그램을 작성하시오.  

**입력**  
첫 번째 줄에 수빈이가 있는 위치 N과 동생이 있는 위치 K가 주어진다. N과 K는 정수이다.  

**출력**  
수빈이가 동생을 찾는 가장 빠른 시간을 출력한다.  

**풀이**

```
from collections import deque

n,k = map(int,input().split())
v = [0 for _ in range(100001)] # 0 ~ 100,000이 범위

def bfs(): # 최단시간은 bfs가 효율적이다.
  q = deque()
  q.append(n)
  while q:
    x = q.popleft()
    if x == k: # n이 k 가 되면 출력하는 조건으로 한다.
      print(v[x])
      break
    for i in (x+1, x-1, x*2): # 수빈이가 이동하는 방식
      if 0<=i<100001 and v[i] == 0: # 지도를 그릴필요도 없으며 방문기록에 count까지 같이하여 효율적으로 
        v[i] = v[x] + 1
        q.append(i)
        
bfs()

```

## Day 6 (2021-09-09)

### **백준 5014번**

**문제**  
강호는 코딩 교육을 하는 스타트업 스타트링크에 지원했다. 오늘은 강호의 면접날이다. 하지만, 늦잠을 잔 강호는 스타트링크가 있는 건물에 늦게 도착하고 말았다.  

스타트링크는 총 F층으로 이루어진 고층 건물에 사무실이 있고, 스타트링크가 있는 곳의 위치는 G층이다. 강호가 지금 있는 곳은 S층이고, 이제 엘리베이터를 타고 G층으로 이동하려고 한다.  

보통 엘리베이터에는 어떤 층으로 이동할 수 있는 버튼이 있지만, 강호가 탄 엘리베이터는 버튼이 2개밖에 없다. U버튼은 위로 U층을 가는 버튼, D버튼은 아래로 D층을 가는 버튼이다. (만약, U층 위, 또는 D층 아래에 해당하는 층이 없을 때는, 엘리베이터는 움직이지 않는다)  

강호가 G층에 도착하려면, 버튼을 적어도 몇 번 눌러야 하는지 구하는 프로그램을 작성하시오. 만약, 엘리베이터를 이용해서 G층에 갈 수 없다면, "use the stairs"를 출력한다.  

**입력**  
첫째 줄에 F, S, G, U, D가 주어진다. (1 ≤ S, G ≤ F ≤ 1000000, 0 ≤ U, D ≤ 1000000) 건물은 1층부터 시작하고, 가장 높은 층은 F층이다.  

**출력**  
첫째 줄에 강호가 S층에서 G층으로 가기 위해 눌러야 하는 버튼의 수의 최솟값을 출력한다. 만약, 엘리베이터로 이동할 수 없을 때는 "use the stairs"를 출력한다.  

**풀이**

```
from collections import deque

f,s,g,u,d = map(int,input().split())
a = [-1 for _ in range(f)] # 굳이 v(방문기록)로 해결하지 않은 이유는 최소입력값과 방문기록이 겹칠 수 있는 상황이기 때문이다. 백준1697번은 방문기록과 겹칠 확률이 매우 적다 아니 없다.
a[s-1] = 0 # 초기 입력값은 리스트에 반영되기 위해서는 1을 뺀다.

def bfs():
  q = deque()
  q.append(s-1)
  v = [0 for _ in range(f)]
  v[s-1] = 1
  while q:
    x = q.popleft()
    for i in [u, -d]: # 이동 범위를 리스트로 넣어놓는다.
      nx = x + i
      if 0<=nx<f and v[nx] == 0:
        a[nx] = a[x] + 1
        v[nx] = 1
        q.append(nx) # bfs 기본공식

bfs()
print(a[g-1] if a[g-1] != -1 else 'use the stairs') # 조건값을 넣어준다.
```
-----

## Day 7 (2021-09-10)

### **백준 2468번**

**문제**  
재난방재청에서는 많은 비가 내리는 장마철에 대비해서 다음과 같은 일을 계획하고 있다. 먼저 어떤 지역의 높이 정보를 파악한다. 그 다음에 그 지역에 많은 비가 내렸을 때 물에 잠기지 않는 안전한 영역이 최대로 몇 개가 만들어 지는 지를 조사하려고 한다. 이때, 문제를 간단하게 하기 위하여, 장마철에 내리는 비의 양에 따라 일정한 높이 이하의 모든 지점은 물에 잠긴다고 가정한다.  

어떤 지역의 높이 정보는 행과 열의 크기가 각각 N인 2차원 배열 형태로 주어지며 배열의 각 원소는 해당 지점의 높이를 표시하는 자연수이다. 예를 들어, 다음은 N=5인 지역의 높이 정보이다.  

6	8	2	6	2  
3	2	3	4	6  
6	7	3	3	2  
7	2	5	3	6  
8	9	5	2	7  
이제 위와 같은 지역에 많은 비가 내려서 높이가 4 이하인 모든 지점이 물에 잠겼다고 하자. 이 경우에 물에 잠기는 지점을 회색으로 표시하면 다음과 같다.   

6	8	2	6	2  
3	2	3	4	6  
6	7	3	3	2  
7	2	5	3	6  
8	9	5	2	7  
물에 잠기지 않는 안전한 영역이라 함은 물에 잠기지 않는 지점들이 위, 아래, 오른쪽 혹은 왼쪽으로 인접해 있으며 그 크기가 최대인 영역을 말한다. 위의 경우에서 물에 잠기지 않는 안전한 영역은 5개가 된다(꼭짓점으로만 붙어 있는 두 지점은 인접하지 않는다고 취급한다).   

또한 위와 같은 지역에서 높이가 6이하인 지점을 모두 잠기게 만드는 많은 비가 내리면 물에 잠기지 않는 안전한 영역은 아래 그림에서와 같이 네 개가 됨을 확인할 수 있다.   
 
6	8	2	6	2  
3	2	3	4	6  
6	7	3	3	2  
7	2	5	3	6  
8	9	5	2	7  
이와 같이 장마철에 내리는 비의 양에 따라서 물에 잠기지 않는 안전한 영역의 개수는 다르게 된다. 위의 예와 같은 지역에서 내리는 비의 양에 따른 모든 경우를 다 조사해 보면 물에 잠기지 않는 안전한 영역의 개수 중에서 최대인 경우는 5임을 알 수 있다.   

어떤 지역의 높이 정보가 주어졌을 때, 장마철에 물에 잠기지 않는 안전한 영역의 최대 개수를 계산하는 프로그램을 작성하시오.   

**입력**  
첫째 줄에는 어떤 지역을 나타내는 2차원 배열의 행과 열의 개수를 나타내는 수 N이 입력된다. N은 2 이상 100 이하의 정수이다. 둘째 줄부터 N개의 각 줄에는 2차원 배열의 첫 번째 행부터 N번째 행까지 순서대로 한 행씩 높이 정보가 입력된다. 각 줄에는 각 행의 첫 번째 열부터 N번째 열까지 N개의 높이 정보를 나타내는 자연수가 빈 칸을 사이에 두고 입력된다. 높이는 1이상 100 이하의 정수이다.  

**출력**  
첫째 줄에 장마철에 물에 잠기지 않는 안전한 영역의 최대 개수를 출력한다.  

**풀이**  

```
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**6) # 데이터 계산한도 증가

dx = [-1, 1, 0, 0] # X축 이동
dy = [0, 0, -1, 1] # Y축 이동

def dfs(x, y, h):
  for i in range(4):
    nx = x + dx[i]
    ny = y + dy[i]
    if 0<=nx<N and 0<=ny<N:
      if arr[nx][ny] > h and visit[nx][ny] == 0:
        visit[nx][ny] = 1
        dfs(nx, ny, h)

N = int(input())
arr = [list(map(int,input().split())) for _ in range(N)]
ans = 0

for k in range(max(map(max, arr))): # 지도데이터 값중 가장 높은 값을 기준으로 물높이를 정함
  visit = [[0]*N for _ in range(N)] # 방문기록은 물높이를 올릴때마다 최신화
  cnt = 0 # 침수되지 않은곳 갯수
  for i in range(N):
    for j in range(N):
      if arr[i][j] > k and visit[i][j] == 0:
        visit[i][j] = 1
        cnt += 1
        dfs(i, j, k) # 최단시간이 아니라 단순히 갯수를 세기 위함
  ans = max(ans, cnt) # 물높이를 올릴때마다 최대값 최신화

print(ans)

```

-----

## Day 8 (2021-09-13)

### **백준 2573번**

**문제**  
지구 온난화로 인하여 북극의 빙산이 녹고 있다. 빙산을 그림 1과 같이 2차원 배열에 표시한다고 하자. 빙산의 각 부분별 높이 정보는 배열의 각 칸에 양의 정수로 저장된다. 빙산 이외의 바다에 해당되는 칸에는 0이 저장된다. 그림 1에서 빈칸은 모두 0으로 채워져 있다고 생각한다.  

 	 	 	 	 	 	   
```  
 	2	4	5	3	 	   
 	3	 	2	5	2	   
 	7	6	2	4	 
``` 	 	 
그림 1. 행의 개수가 5이고 열의 개수가 7인 2차원 배열에 저장된 빙산의 높이 정보  

빙산의 높이는 바닷물에 많이 접해있는 부분에서 더 빨리 줄어들기 때문에, 배열에서 빙산의 각 부분에 해당되는 칸에 있는 높이는 일년마다 그 칸에 동서남북 네 방향으로 붙어있는 0이 저장된 칸의 개수만큼 줄어든다. 단, 각 칸에 저장된 높이는 0보다 더 줄어들지 않는다. 바닷물은 호수처럼 빙산에 둘러싸여 있을 수도 있다. 따라서 그림 1의 빙산은 일년후에 그림 2와 같이 변형된다.  

그림 3은 그림 1의 빙산이 2년 후에 변한 모습을 보여준다. 2차원 배열에서 동서남북 방향으로 붙어있는 칸들은 서로 연결되어 있다고 말한다. 따라서 그림 2의 빙산은 한 덩어리이지만, 그림 3의 빙산은 세 덩어리로 분리되어 있다.  

```
 		2	4	1	 	     
 	1	 	1	5	 	   
 	5	4	1	2	 	   
``` 	 
그림 2  

```	 	 	 	 	 	 
 	 	 	3	 	 	   
 	 	 	 	4	 	   
 	3	2	 	 	 	   
```
그림 3

한 덩어리의 빙산이 주어질 때, 이 빙산이 두 덩어리 이상으로 분리되는 최초의 시간(년)을 구하는 프로그램을 작성하시오. 그림 1의 빙산에 대해서는 2가 답이다. 만일 전부 다 녹을 때까지 두 덩어리 이상으로 분리되지 않으면 프로그램은 0을 출력한다.  

**입력**  
첫 줄에는 이차원 배열의 행의 개수와 열의 개수를 나타내는 두 정수 N과 M이 한 개의 빈칸을 사이에 두고 주어진다. N과 M은 3 이상 300 이하이다. 그 다음 N개의 줄에는 각 줄마다 배열의 각 행을 나타내는 M개의 정수가 한 개의 빈 칸을 사이에 두고 주어진다. 각 칸에 들어가는 값은 0 이상 10 이하이다. 배열에서 빙산이 차지하는 칸의 개수, 즉, 1 이상의 정수가 들어가는 칸의 개수는 10,000 개 이하이다. 배열의 첫 번째 행과 열, 마지막 행과 열에는 항상 0으로 채워진다.  

**출력**  
첫 줄에 빙산이 분리되는 최초의 시간(년)을 출력한다. 만일 빙산이 다 녹을 때까지 분리되지 않으면 0을 출력한다.  

```
from collections import deque

dx = [-1, 1, 0, 0] (x축으로 이동)
dy = [0, 0, -1, 1] (y축으로 이동)

def bfs(a, b): # bfs를 한 이유는 녹은값과 빙하가 이어지는 것을 동시에 생각해야 하기 때문이다.
    q = deque()
    q.append((a, b))
    v[a][b] = 1
    while q:
        x,y = q.popleft()
        for i in range(4):
            nx = x + dx[i] # x축으로 이동한다고 해서 x자체가 ㅊ좌표가 아니다 여기서 x는 우리가 흔히아는 y좌표다
            ny = y + dy[i] # y는 x좌표다
            if 0<=nx<n and 0<=ny<m: # 왜냐하면 2차원 리스트는 앞에서부터 열,행으로 데이터를 저장함
                if s[nx][ny] == 0:
                    if s[x][y]:
                        s[x][y] -= 1
                elif s[nx][ny] > 0 and v[nx][ny] == 0:
                    q.append((nx, ny))                  # bfs방식
                    v[nx][ny] = 1
                
n,m = map(int, input().split())                         # 입력데이터
s = [list(map(int, input().split())) for _ in range(n)] # 지도데이터
year = 0                                                # 최종값

while True:
    v = [[0] * m for _ in range(n)]                     # 방문기록 데이터는 연도가 지날때마다 최신화 한다.
    cnt = 0                                             # 빙하 수도 연도가 지날 최신화 한다.
    for i in range(n):
        for j in range(m):
            if s[i][j] > 0 and v[i][j] == 0:
                cnt += 1
                bfs(i, j)
    
    if cnt == 0:          # 애초에 이어져 있지않은 데이터를 0으로 출력한다.
        year = 0
        break
    if cnt > 1:           # 빙하가 2개로 쪼개지면 연도를 출력한다.
        break
    year += 1             # 이상이 없으면 다음 해로 넘긴다.

print(year)
```

## Day 9 (2021-09-14)

### **백준 9205번**

**문제**  
송도에 사는 상근이와 친구들은 송도에서 열리는 펜타포트 락 페스티벌에 가려고 한다. 올해는 맥주를 마시면서 걸어가기로 했다. 출발은 상근이네 집에서 하고, 맥주 한 박스를 들고 출발한다. 맥주 한 박스에는 맥주가 20개 들어있다. 목이 마르면 안되기 때문에 50미터에 한 병씩 마시려고 한다. 즉, 50미터를 가려면 그 직전에 맥주 한 병을 마셔야 한다.  

상근이의 집에서 페스티벌이 열리는 곳은 매우 먼 거리이다. 따라서, 맥주를 더 구매해야 할 수도 있다. 미리 인터넷으로 조사를 해보니 다행히도 맥주를 파는 편의점이 있다. 편의점에 들렸을 때, 빈 병은 버리고 새 맥주 병을 살 수 있다. 하지만, 박스에 들어있는 맥주는 20병을 넘을 수 없다. 편의점을 나선 직후에도 50미터를 가기 전에 맥주 한 병을 마셔야 한다.  

편의점, 상근이네 집, 펜타포트 락 페스티벌의 좌표가 주어진다. 상근이와 친구들이 행복하게 페스티벌에 도착할 수 있는지 구하는 프로그램을 작성하시오.  

**입력**  
첫째 줄에 테스트 케이스의 개수 t가 주어진다. (t ≤ 50)  

각 테스트 케이스의 첫째 줄에는 맥주를 파는 편의점의 개수 n이 주어진다. (0 ≤ n ≤ 100).  

다음 n+2개 줄에는 상근이네 집, 편의점, 펜타포트 락 페스티벌 좌표가 주어진다. 각 좌표는 두 정수 x와 y로 이루어져 있다. (두 값 모두 미터, -32768 ≤ x, y ≤ 32767)  

송도는 직사각형 모양으로 생긴 도시이다. 두 좌표 사이의 거리는 x 좌표의 차이 + y 좌표의 차이 이다. (맨해튼 거리)  

**출력**  
각 테스트 케이스에 대해서 상근이와 친구들이 행복하게 페스티벌에 갈 수 있으면 "happy", 중간에 맥주가 바닥나서 더 이동할 수 없으면 "sad"를 출력한다.   

```
for _ in range(int(input())):                        # test 갯수값 입력
    n = int(input())                                 # n 값 입력
    s = [list(map(int,input())) for _ in range(n+2)] # 가지(branch)데이터
    v = [0 for _ in range(n+2)]                      # 방문기록 데이터
    d = [0]                                          # 초기값 입력
    p = False
    while d:
        x = d.pop()
        if x==n+1: p=True; break                     # 마지막 값 도달 목적으로 함
        for a in range(n+2):
            if abs(s[x][0]-s[a][0]) + abs(s[x][1]-s[a][1]) <= 1000:
            d.append(a)                              # bfs 방식으로 함
            v[a] = 1                                 # 방문데이터 값 변경
    print('happy' if p else 'sad')                   # 출력
```

## Day 9 (2021-09-14)

### **백준 9205번**

**문제**  
스타트링크에서 판매하는 어린이용 장난감 중에서 가장 인기가 많은 제품은 구슬 탈출이다. 구슬 탈출은 직사각형 보드에 빨간 구슬과 파란 구슬을 하나씩 넣은 다음, 빨간 구슬을 구멍을 통해 빼내는 게임이다.  

보드의 세로 크기는 N, 가로 크기는 M이고, 편의상 1×1크기의 칸으로 나누어져 있다. 가장 바깥 행과 열은 모두 막혀져 있고, 보드에는 구멍이 하나 있다. 빨간 구슬과 파란 구슬의 크기는 보드에서 1×1크기의 칸을 가득 채우는 사이즈이고, 각각 하나씩 들어가 있다. 게임의 목표는 빨간 구슬을 구멍을 통해서 빼내는 것이다. 이때, 파란 구슬이 구멍에 들어가면 안 된다.  

이때, 구슬을 손으로 건드릴 수는 없고, 중력을 이용해서 이리 저리 굴려야 한다. 왼쪽으로 기울이기, 오른쪽으로 기울이기, 위쪽으로 기울이기, 아래쪽으로 기울이기와 같은 네 가지 동작이 가능하다.  

각각의 동작에서 공은 동시에 움직인다. 빨간 구슬이 구멍에 빠지면 성공이지만, 파란 구슬이 구멍에 빠지면 실패이다. 빨간 구슬과 파란 구슬이 동시에 구멍에 빠져도 실패이다. 빨간 구슬과 파란 구슬은 동시에 같은 칸에 있을 수 없다. 또, 빨간 구슬과 파란 구슬의 크기는 한 칸을 모두 차지한다. 기울이는 동작을 그만하는 것은 더 이상 구슬이 움직이지 않을 때 까지이다.  

보드의 상태가 주어졌을 때, 최소 몇 번 만에 빨간 구슬을 구멍을 통해 빼낼 수 있는지 구하는 프로그램을 작성하시오.   

**입력**  
첫 번째 줄에는 보드의 세로, 가로 크기를 의미하는 두 정수 N, M (3 ≤ N, M ≤ 10)이 주어진다. 다음 N개의 줄에 보드의 모양을 나타내는 길이 M의 문자열이 주어진다. 이 문자열은 '.', '#', 'O', 'R', 'B' 로 이루어져 있다. '.'은 빈 칸을 의미하고, '#'은 공이 이동할 수 없는 장애물 또는 벽을 의미하며, 'O'는 구멍의 위치를 의미한다. 'R'은 빨간 구슬의 위치, 'B'는 파란 구슬의 위치이다.  

입력되는 모든 보드의 가장자리에는 모두 '#'이 있다. 구멍의 개수는 한 개 이며, 빨간 구슬과 파란 구슬은 항상 1개가 주어진다.  

**출력**  
최소 몇 번 만에 빨간 구슬을 구멍을 통해 빼낼 수 있는지 출력한다. 만약, 10번 이하로 움직여서 빨간 구슬을 구멍을 통해 빼낼 수 없으면 -1을 출력한다.  


**풀이**  
```
from collections import deque

n,m = map(int, input().split())                                             # 가로 세로값 입력
s = []                                                                      # 지도데이터
v = [[[[False] * m for _ in range(n)] for _ in range(m)] for _ in range(n)] # 방문데이터

dx = [-1, 1, 0, 0]      # X축이동
dy = [0, 0, -1, 1]      # Y축이동

def move(i, j, dx, dy): 
    c = 0                                          # 이동횟수
    while s[i+dx][j+dy] != '#' and s[i][j] != 'O': # 위 조건이 상과없다면 끝까지 이동하도록
        i += dx
        j += dy
        c += 1                                     # 단 이동횟수를 세서 비교해야함
    return i, j, c

def bfs():
    while q:
        ri, rj, bi, bj, d = q.popleft()
        if d > 10:                                        # 10회 이상 움직일 경우 '-1' 출력
            break
        for i in range(4):
            nri, nrj, rc = move(ri, rj, dx[i], dy[i])
            nbi, nbj, bc = move(bi, bj, dx[i], dy[i])
            if s[nbi][nbj] != 'O':                        # 파란구슬이 들어가지 않는 조건
                if s[nri][nrj] == 'O':                    # 빨간구슬만 들어가는 조건
                    print(d)
                    return
                if nri == nbi and nrj == nbj:             # 파란구슬과 빨간구슬이 겹칠경우를 대비하기 위함
                    if rc > bc:                           # 만약 겹쳤을 경우 더 많이 이동해온 구슬에서 왔던 칸으로 다시 이동
                        nri -= dx[i]
                        nrj -= dy[i]
                    else:
                        nbi -= dx[i]
                        nbj -= dy[i]
                if not v[nri][nrj][nbi][nbj]:             # 아무조건에 걸리지 않을 경우 방문기록값을 최신화하여 이동이 겹치지 않도록 함
                    v[nri][nrj][nbi][nbj] = True
                    q.append([nri, nrj, nbi, nbj, d+1])   # d 값은 총 움직인 횟수임
    print(-1)
    
for i in range(n):
    a = list(input())                                     # 입력값을 받아 지도데이터에 넣음
    s.append(a)
    for j in range(m):
        if a[j] == 'R':                                   # 만약 입력값이 B, R일 경우를 바로 최신화 시킴
            ri, rj = i, j
        if a[j] == 'B':
            bi, bj = i, j
            
q = deque()
q.append([ri, rj, bi, bj, 1])                             # 초기값 입력
v[ri][rj][bi][bj] = True                                  # 초기값 방문기록 입력
bfs()           
```

##  Day 10 (2021-09-18)

### 백준 12100번

```
import sys, copy
input = sys.stdin.readline # 입력 속도 up

def move(dir):                                # 북쪽으로 몰았을 때
    if dir == 0:
        for j in range(n):
            idx = 0                           # 변동되는 index
            for i in range(1, n):
                if a[i][j]:
                    temp = a[i][j]
                    a[i][j] = 0
                    if a[idx][j] == 0:
                        a[idx][j] = temp      # 해당값이 0이라면 a[i][j]값으로 바꿈
                    elif a[idx][j] == temp:
                        a[idx][j] = temp * 2  # 해당값이 저장값과 같다면 2배로
                        idx += 1
                    else:
                        idx += 1
                        a[idx][j] = temp      # 해당값이 a[i][j]값과 다르다면 그 a[idx][j]값을 바꿈
    elif dir == 1:                            # 남쪽으로 몰았을 때
        for j in range(n):
            idx = n-1
            for i in range(n-2, -1, -1):
                if a[i][j]:
                    temp = a[i][j]
                    a[i][j] = 0
                    if a[idx][j] == 0:
                        a[idx][j] = temp
                    elif a[idx][j] == temp:
                        a[idx][j] = temp * 2
                        idx -= 1
                    else:
                        idx -= 1
                        a[idx][j] = temp
    elif dir == 2:                            # 동쪽으로 몰았을 때
        for i in range(n):
            idx = 0
            for j in range(1, n):
                if a[i][j]:
                    temp = a[i][j]
                    a[i][j] = 0
                    if a[i][idx] == 0:
                        a[i][idx] = temp
                    elif a[i][idx] == temp:
                        a[i][idx] = temp * 2
                        idx += 1
                    else:
                        idx += 1
                        a[i][idx] = temp
                        
    else:                                      # 서쪽으로 몰았을 
        for i in range(n):
            idx = n-1
            for j in range(n-2, -1, -1):
                if a[i][j]:
                    temp = a[i][j]
                    a[i][j] = 0
                    if a[i][idx] == 0:
                        a[i][idx] = temp
                    elif a[i][idx] == temp:
                        a[i][idx] = temp * 2
                        idx -= 1
                    else:
                        idx -= 1
                        a[i][idx] = temp
    
def dfs(cnt):
    global a, ans                              # 전역변수
    if cnt == 5:                               # 기회는 5번까지
        for i in range(n):
            for j in range(n):
                ans = max(ans, a[i][j])        # 정산
        return
    
    temp_a = copy.deepcopy(a)                  # 기회가 1개씩 증가할 때마다 임의의 a값 생성
    for i in range(4):
        move(i)
        dfs(cnt + 1)
        a = copy.deepcopy(temp_a)              # move이후 값에 따른 임의의 변동 a값 생성

n = int(input())
a = [list(map(int, input().split())) for _ in range(n)]  # 지도데이터
ans = 0                                                  # 출력값(최대자연수)
dfs(0)                                                   # 기회 0부터 시작
print(ans)
```
-----

## Day 11(2021-09-24)

### 백준 3190번

```
from collections import deque


def change(d, c):
    # 상(0) 우(1) 하(2) 좌(3)
    # 동쪽 회전: 상(0) -> 우(1) -> 하(2) -> 좌(3) -> 상(0) : +1 방향
    # 왼쪽 회전: 상(0) -> 좌(3) -> 하(2) -> 우(1) -> 상(0) : -1 방향
    if c == "L":
        d = (d - 1) % 4
    else:
        d = (d + 1) % 4
    return d


# 상 우 하 좌
dy = [-1, 0, 1, 0]
dx = [0, 1, 0, -1]


def start():
    direction = 1  # 초기 방향
    time = 1  # 시간
    y, x = 0, 0  # 초기 뱀 위치
    visited = deque([[y, x]])  # 방문 위치 '[[]]'인 이유는 아래 popleft에 있어서 y,x 를 동시에 입력이 들어가야하는데 '[]'만 붙이면 y만 나오게 된다.
    arr[y][x] = 2
    while True:
        y, x = y + dy[direction], x + dx[direction]
        if 0 <= y < N and 0 <= x < N and arr[y][x] != 2:
            if not arr[y][x] == 1:  # 사과가 없는 경우
                temp_y, temp_x = visited.popleft()
                arr[temp_y][temp_x] = 0  # 꼬리 제거
            arr[y][x] = 2
            visited.append([y, x])
            if time in times.keys():
                direction = change(direction, times[time])
            time += 1
        else:  # 본인 몸에 부딪히거나, 벽에 부딪힌 경우
            return time


if __name__ == "__main__": # 이것을 붙인 이유는 파이썬은 실행할때 위에서부터 실행하는데 def 부분은 따로 출력으로 호출하지 않으면 실행이 되지 않게하는 것이다.
                           # 즉 결국 프로그래밍 속도를 빠르게 하기 위함이다.  

    # input
    N = int(input())
    K = int(input())
    arr = [[0] * N for _ in range(N)]
    for _ in range(K):
        a, b = map(int, input().split())
        arr[a - 1][b - 1] = 1  # 사과 저장
    L = int(input())
    times = {}
    for i in range(L):
        X, C = input().split()
        times[int(X)] = C
    print(start())
```

-----


## Day 12(2021-09-28)

### 백준 13458번

```
def count():
    ans = 0
    for i in range(n):
        if arr[i] > 0:                # 총감독관 인원 수
            arr[i] -= b
            ans += 1
        if arr[i] > 0:                # 부감독관 인원수
            ans += int(arr[i]/c)      # 몫만큼 제외
            
            if arr[i] % c != 0:       # 나머지값 존재시 + 1
                ans += 1
    
    return ans                        # 답 return
            
n = int(input())                      # 강의실 수
arr = list(map(int,input().split()))  # 강의실 별 학생수
b,c = map(int,input().split())        # 총감독관, 부감독관 인원수
print(count())
```
-----
## Day 13 (2021-09-29)

### 백준 14499번

```
import sys
input = sys.stdin.readline

def move(direction):
    if direction == 1:                                                                  # 주사위 모형도 1(바닥), 2(바닥상부쪽), 5(바닥하부쪽), 4(바닥좌측), 3(바닥우측), 6(윗면)
        dice[1], dice[3], dice[4], dice[6] = dice[3], dice[6], dice[1], dice[4]         # 우측회전
    elif direction == 2:
        dice[1], dice[3], dice[4], dice[6] = dice[4], dice[1], dice[6], dice[3]         # 좌측회전
    elif direction == 3:
        dice[1], dice[2], dice[5], dice[6] = dice[2], dice[6], dice[1], dice[5]         # 전측회전
    elif direction == 4:
        dice[1], dice[2], dice[5], dice[6] = dice[5], dice[1], dice[6], dice[2]         # 후측회전

def dire(direction):
    if direction == 1: return 0,1               # 동쪽이동
    elif direction == 2: return 0,-1            # 서쪽이동
    elif direction == 3: return -1, 0           # 북쪽이동
    elif direction == 4: return 1, 0            # 남쪽이동

n,m,x,y,k = map(int,input().split())
s = []                                          # 지도데이터
for _ in range(n):                              # 지도데이터 최신화
    s.append(list(map(int,input().split())))
com = list(map(int,input().split()))            # 방향데이터
dice = [0,0,0,0,0,0,0]                          # 주사위지도

for i in com:                             # 방향데이터로 시작
    a,b = dire(i)                         # 이동하기 위한 값
    if 0<x+a<n and 0<y+b<m:
        x = x+a
        y = y+b
        move(i) 
        if s[x][y] != 0:                  # 0이 아닌 값은 주사위 바닥값을 해당지도데이터로 최신화 후 지도데이터값은 0으로 바꿈
            dice[1] = s[x][y]
            s[x][y] = 0
        else:                             # 지도데이터가 0이라면 주사위 바닥값 복사
            s[x][y] = dice[1]
        print(dice[6])                    # 
```

-----

## Day 14 (2021-10-02)

### 백준 14500번

```
import sys; input = sys.stdin.readline

def dfs(x, y, idx, total):                # 블럭을 만들어 지도데이터 값의 총합을 구하는 함수
    global ans                            # 함수 안에서 global변수로 저장 / 함수 안에서 변하는 수이지만 초기화 되지 않고 이전 값 그대로 가져감
    if ans >= total + max_val * (3-idx):  # 최대값을 정하여 그 이상으로 값이 나갈시에 걸러내어 시간을 빨리 나오게 함
        return
    if idx == 3:                          # 4번째 블럭 완성시 출력 
        ans = max(ans, total)
        return
    else:
        for i in range(4):
            nx = x + dx[i]
            ny = y + dy[i]
            if 0 <=nx<n and 0<=ny<m and v[nx][ny] == 0:
                if idx == 1:                                      # 'ㅗ'자 만들기위함
                    v[nx][ny] = 1
                    dfs(x, y, idx+1, total + arr[nx][ny])         
                    v[nx][ny] = 0
                v[nx][ny] = 1                                     # 'ㅁ', 'ㅣ', ㄱ', 'N' 블럭 만듦
                dfs(nx, ny, idx+1, total + arr[nx][ny])
                v[nx][ny] = 0
    
n,m = map(int, input().split())                             # 열, 행 입력
arr = [list(map(int,input().split())) for _ in range(n)]    # 지도데이터 입력
v = [[0] * m for _ in range(n)]                             # 방문데이터 (중복방지)
dx = [1, -1, 0, 0]                                          # x축
dy = [0, 0, 1, -1]                                          # y축
ans = 0                                                     # 지도데이터 4개의 합의 최대값
max_val = max(map(max,arr))                                 # 지도데이터 단독 최대값

for x in range(n):
    for y in range(m):
        v[x][y] = 1                 # 1번 블럭 방문데이터 1로 바꾸며 중복방지 
        dfs(x, y, 0, arr[x][y])     # 블록함수
        v[x][y] = 0                 # 다음 수의 방문데이터를 위한 초기화
        
print(ans)
```
-----

## Day 15 (2021-10-13)

### 백준 14501번

```
n = int(input())
t = []
p = []
dp = []
for _ in range(n):
    a,b = map(int,input().split())
    t.append(a)
    p.append(b)
    dp.append(b)
dp.append(0)
for i in range(n-1, -1, -1):
    if t[i] + i > n:
        dp[i] = dp[i + 1]
    else:
        dp[i] = max(dp[i + 1], p[i] + dp(t[i] + i))
print(dp[0])
```
