---
title: 크롬 게임 개발기
date: 2024-11-22 15:43:07 +0900
categories: [daveloping]
tags: [develop, python]
pin: true
render_with_liquid: false
---

이번 포스팅에서는 2024년 2학기에 진행한 `크롬 게임 만들기` 에 대한 개발기를 써보려고 합니다.

---

# 📱 **게임 개발기: Pygame으로 만든 'Charlie Jump Dino'**

## 1. **소개**
### **게임 소개**

이 게임은 크롬 브라우저에서 기본으로 제공되는 공룡 점프 게임을 바탕으로 한 2D 게임입니다. 화면에 등장하는 장애물들을 피하는 것이 주요 목표로, 장애물을 피하고 점수를 쌓는 과정에서 점점 속도가 증가합니다. 플레이어는 더 높은 점수를 기록하는 것을 목표로 게임을 진행하게 됩니다. 

이 게임은 ```Pygame``` 라이브러리를 사용하여 개발되었으며, 단순하지만 직관적인 인터페이스와 도전적인 게임 플레이로 누구나 쉽게 접근할 수 있습니다. 게임은 기본적으로 ```키보드 입력```을 통해 조작되며, `점프`, `슬라이딩` 등의 기능을 지원하여 다양한 방식으로 장애물을 피할 수 있습니다.

### **게임의 특징**
- **점프**: 공룡이 장애물을 피하기 위해 점프할 수 있으며 이를 통해 장애물을 피하거나 더 높은 곳으로 올라갈 수 있습니다.
- **슬라이딩**: 특정 장애물(예: 새)을 피하기 위해 공룡이 땅에 엎드려서 슬라이딩할 수 있는 기능이 있습니다.
- **장애물**: 게임 중에는 여러 종류의 장애물이 등장합니다.
- **점수 시스템**: 게임을 진행하면서 획득한 점수는 화면 상단에 실시간으로 표시됩니다. 점수는 시간이 지날 때마다 증가하며, 최종 점수는 게임 종료 후 확인할 수 있습니다.
- **게임 종료 및 하이스코어**: 게임이 종료되면 사용자 이름을 입력하여 하이스코어를 기록할 수 있습니다. 이를 통해 자신이 기록한 최고 점수를 다른 사람들과 비교할 수 있습니다.
- **간단한 UI**: 게임의 시작 화면, 게임 진행 중 점수 표시, 게임 종료 후 하이스코어 입력 기능, 낮/밤 테마 등 기본적인 사용자 인터페이스(UI)가 잘 구성되어 있어, 직관적으로 게임을 플레이할 수 있습니다.

---
  
## 2. **개발 환경 및 도구**

### 🔧 **Pygame**
- **Pygame**은 Python 기반의 2D 게임 개발 라이브러리입니다. 직관적인 API와 다양한 기능(그래픽, 사운드, 이벤트 처리)을 제공하여 빠르게 게임을 만들 수 있습니다. 이 게임에서는 화면 렌더링, 충돌 처리, 사운드 효과 등을 Pygame으로 구현했습니다.

### 🖥 **개발 언어 및 도구**
- **Python**: 간결한 문법과 빠른 개발 속도 덕분에 게임 개발에 적합한 언어입니다.
- **PyCharm**: 코드 자동 완성, 디버깅, 버전 관리 등 다양한 기능을 제공하는 IDE로 개발을 효율적으로 지원합니다.
- **Git**: 코드 변경 사항을 관리하고, 협업 시 충돌을 방지하기 위해 Git을 사용했습니다.


# 3. **게임 설계 및 기능 구현**
저는 `하이스코어 구현`, `점프 로직`, `스킨 이미지 불러오기`, `README 등의 md` 를 담당했습니다.

## 1. **하이스코어 구현**  
### **기능 요약**  
게임이 끝난 후 플레이어가 얻은 점수를 저장하고, 상위 5개의 하이스코어를 유지하도록 구현했습니다.  

### **주요 작업**  
1. **하이스코어 저장**  
   - 플레이어의 점수를 리스트로 관리하여, 최종 점수가 상위 5위에 들 경우 리스트를 갱신하도록 설계했습니다.  
   - `config.py`에 하이스코어를 저장하고, 하이스코어 초기화 오류를 해결하기 위해 하이스코어 관리용 별도의 함수와 파일 구조를 추가했습니다.  

2. **게임 오버 시 하이스코어 표시**  
   - 게임 오버 화면에서 상위 5개의 점수를 볼 수 있도록 구현했습니다.  

<br>

### **하이스코어 관련 이슈**  
- **목표:**  
  - 점수를 남기고 이를 하이스코어로 저장.  
  - 상위 5위 안에 들면 하이스코어 갱신.  
  - 갱신된 하이스코어를 게임 오버 화면에 표시.  


### **코드 구현**  

#### **하이스코어를 저장하는 함수**  
```python
# 하이스코어를 config 파일에 저장하는 함수
def save_high_scores():
    with open("config.py", "w") as config_file:
        config_file.write("high_scores = " + str(high_scores))
```

#### **하이스코어 갱신 함수**  
```python
# 현재 점수가 하이스코어에 해당하면 리스트 갱신
def update_high_scores(score):
    global high_scores
    high_scores.append(score)
    high_scores = sorted(high_scores, reverse=True)[:5]  # 상위 5개의 점수만 유지
    save_high_scores()
```

#### **점수 업데이트 로직**  
```python
# 현재 점수가 상위 5위 안에 들면 하이스코어 업데이트
update_high_scores(points)
```

#### **게임 오버 화면에서 하이스코어 표시**  
```python
# 하이스코어 표시
high_score_text = font.render("High Scores:", True, (0, 0, 0))
SCREEN.blit(high_score_text, (SCREEN_WIDTH // 2 - 80, SCREEN_HEIGHT // 2 + 100))
for i, hs in enumerate(high_scores):
    hs_text = font.render(f"{i + 1}. {hs}", True, (0, 0, 0))
    SCREEN.blit(hs_text, (SCREEN_WIDTH // 2 - 80, SCREEN_HEIGHT // 2 + 130 + i * 30))
```

#### **`highscore.py` 초기화 예시**  
```python
# highscore.py
high_scores = [429, 331, 321, 311, 279]
```

<br>

### **진행하며 발생한 오류**  

**초기화 과정에서 발생한 버그**  
- **문제:**  
  - 초기에는 `config.py`에 하이스코어를 저장했지만, 하이스코어 초기화 과정에서 `config` 파일 자체가 완전히 초기화되는 문제가 발생했습니다.  
  - 이로 인해 다른 상수값이 사라지고, 게임을 재시작하면 하이스코어 데이터가 손실되는 상황이 발생했습니다.  

- **시도한 해결 방법:**  
  1. **JSON 파일 사용**  
     - 하이스코어를 `json` 파일로 저장하는 방식을 시도했습니다.  
     - 그러나 `json`은 딕셔너리 형태로 데이터를 다룰 수 없기에 기존 코드에서 이를 불러오는 방식과 충돌하여 추가적인 오류가 발생했습니다.  
  2. **별도 Python 파일로 분리**  
     - 기존 방식으로 돌아가 `highscore.py`라는 별도 파일을 생성.  
     - 상위 5개 점수만 저장하던 방식에서 모든 점수를 딕셔너리 형태로 저장하는 방식으로 변경했습니다.  

#### **수정된 딕셔너리 구조 예시**  
```python
# highscore.py
hhigh_scores = {
    'aaa': 223,
    'kang': 4794,
    'sim': 162,
    'vm': 857,
    'adadw': 165,
    'adw': 167,
    'sad': 165
}
```
---
## 2. 점프 로직
- 이단 점프 기능은구현 과정에서 여러 번의 도전과 실패를 겪으며 최종적으로 제외하기로 결정된 기능입니다. 여기서는 이단 점프 구현 과정에서 발생한 문제와 시도했던 해결책들을 기록해보겠습니다. 

### 이단 점프 로직 1차 구현기
#### 이단 점프 관련 이슈
1. **이단 점프 가능 여부**: 공룡이 첫 번째 점프 이후 공중에서 한 번 더 점프 가능해야 함.
2. **점프 속도와 높이 조절**: 두 번째 점프의 속도와 높이를 첫 번째 점프와 별도로 설정.
3. **화면 경계 제한**: 공룡이 화면 밖으로 넘어가지 않도록 제어.


#### **2. 코드 구현**
**이단 점프 로직** 
```python
def double_jump(self):  # 이단 점프 로직
        self.image = self.jump_img
        self.dino_rect.y -= self.DOUBLE_JUMP_VEL * 3  # 이단 점프를 위한 속도
        self.double_jump_active = False  # 이단 점프 사용 후 해제
```
**이단 점프 호출**
```python
if userInput[pygame.K_UP]:
    if not self.dino_jump:  # 첫 번째 점프
        self.sound_manager.play_jump()  # 점프 효과음
        self.dino_duck = False
        self.dino_run = False
        self.dino_jump = True
        self.jump_vel = self.JUMP_VEL  # 점프 속도 초기화
        self.double_jump_active = True  # 이단 점프 가능 상태로 설정
    elif self.double_jump_active:  # 점프 중일 때 다시 점프 (이단 점프)
        self.sound_manager.play_jump()  # 점프 효과음
        self.double_jump()
```
#### **3. 문제 해결**
**문제**: 공룡이 점프 중 화면 경계를 초과하거나 점프가 부자연스러워 보이는 경우가 발생.  
**해결책**: 공룡이 화면 높이보다 높이 올라갈 수 없도록 막음
```python
# 공룡이 화면을 넘어가지 않도록 제한
    if self.dino_rect.y < 0:
        self.dino_rect.y = 0  # 위로 나가지 않도록 제한
```
<br>

### 이단 점프 로직 2차 구현기
#### 이단 점프 관련 이슈
1.  한 번만 점프해도 **이단 점프 로직이 호출**되는 문제
2. 이단 점프 시 **살짝 내려왔다가 다시 올라가도록** 되어있는 문제 : 현재는 한 번 점프 후 다시 올라갈 때 살짝 내려감.
3. `up` 버튼을 **계속 누를 시**에 이단 점프가 아닌 일반 점프가 호출되도록 수정


#### 코드 구현
- 여기부터는 실제로 성공한 코드가 아닙니다. 하지만 어떤 오류가 있었는 지 아는 것도 중요하다 생각해 써놓았습니다.

#### 1차 시도
##### 아이디어: 이단 점프의 횟수 제한
##### 코드
```python
if userInput[pygame.K_UP]:
        if not self.dino_jump:
            self.sound_manager.play_jump() 
            self.dino_duck = False
            self.dino_run = False
            self.dino_jump = True
            self.jump_vel = self.JUMP_VEL 
            self.double_jump_count = 1  # 첫 번째 점프 후 두 번째 점프 가능
        elif self.dino_jump and self.double_jump_count > 0:  # 점프 중일 때 두 번째 점프
            self.sound_manager.play_jump()  
            self.double_jump() 
            self.double_jump_count -= 1  # 남은 이단 점프 횟수 감소
```

##### 버그 이유 (추측)
- 코드의 점프 로직을 살펴보면 결국 이단 점프라는 것은 일반 점프 후 다시 일반 점프를 하는 방식입니다. 
- 이단 점프라는 동작이 끝난 후에는 바로 달리기 동작으로 넘어가는 것이 아니라 ```이단 점프 종료 > 일반 점프 종료 > 달리기```의 방식으로 돌아가는 것입니다. 이럴 경우 이단 점프가 끝난 후 다시 jump 상태가 되면서 결국 ```self.double_jump_count = 1```이 되어 버려 이단 점프의 횟수 제한이 되지 않게 됩니다.

###### 추가 내용
그렇다면 이단 점프 후 무조건 달리기로 돌아오도록 로직을 바꾸면 어떻게 될 지 궁금했습니다.

```python
def double_jump(self):  # 이단 점프 로직
        self.image = self.jump_img
        self.dino_rect.y -= self.DOUBLE_JUMP_VEL * 3
        self.double_jump_active = False 
        self.dino_jump = False  # 이단 점프 후 점프 상태 종료
        self.dino_run = True  # 달리기 상태로 전환
```
이런 형태로 코드를 입력하니 예상 못한 오류가 발생했습니다.. 
- 공룡이 8.0 만큼의 기본 점프도 못하고 ```곧바로 땅으로 착지```해버렸기 때문입니다.. 
- 추측: 아마 해당 코드는 이단 점프 중 ``Y 축`` 이동이 ```DOUBLE_JUMP_VEL```에 의해서 한 번만 실행되었기 때문에 발생한 것 같았습니다. 이로 인해 공룡이 충분히 상승하지 못하고 바로 땅으로 착지하는 비정상적인 동작이 발생한 것으로 보이기 때문입니다. 

#### 2차 시도
##### 아이디어: 키보드가 눌려져 있는 변수 설정
##### 코드
```python
def __init__(self):
        self.is_key_pressed = False  # 키 입력 상태를 추적하는 변수
```
이런 식으로 키 입력 상태를 추적하는 변수를 설정한 후 로직을 바꿔봤습니다.

```python
# 키 입력에 따라 상태 변경
        if userInput[pygame.K_UP] and not self.is_key_pressed:  # UP 키를 눌렀을 때
            self.is_key_pressed = True  # 키 입력을 처리했음을 표시
            if not self.dino_jump:  # 첫 번째 점프
                self.dino_duck = False
                self.dino_run = False
                self.dino_jump = True
                self.jump_vel = self.JUMP_VEL  
                self.double_jump_active = True  
            elif self.double_jump_active:  # 점프 중일 때 다시 점프 (이단 점프)
                self.double_jump()
        if not userInput[pygame.K_UP]:  # UP 키를 떼었을 때
            self.is_key_pressed = False  # 키 입력을 다시 받을 수 있도록 함
```
의도한 코드는 주석에도 적혀있듯이 **키보드가 눌렸을 때**만 동작이 작동하게끔 만들고 싶었습니다.

##### 버그 이유(추측)
- Pygame은 **키보드 입력이 지속적으로 감지**되기 때문에, 키가 눌려져 있는 상태도 입력이라고 봅니다. 따라서 근본적으로 Pygame에서 키보드가 눌려져 있는 현상을 방지하기에는 어려움이 있어 보였습니다.
- 또한 해당 코드를 살펴보면 이단 점프라는 것이 결국 일반 점프 상태에서 다시 일반 점프를 눌러야하기 때문에 애초에 점프의 이중 입력을 막을 수도 없었습니다.


#### 3차 시도
##### 아이디어: 점프 시간에 리밋을 걸어보기
##### 코드 
```python
if self.dino_jump:
    if current_time - self.jump_start_time >= self.JUMP_DURATION:  # 점프 시간이 지나면
        self.dino_jump = False  # 점프 종료
        self.dino_run = True  # 달리기로 돌아옴
        self.jump_vel = self.JUMP_VEL 
        self.double_jump_active = False  
    else:
        self.dino_rect.y -= self.jump_vel * 3 
        self.jump_vel -= 0.6 
if current_time - self.jump_start_time >= self.JUMP_DURATION:  # 이단 점프 시간 제한
    self.dino_jump = False
    self.double_jump_active = False  # 이단 점프 비활성화
    self.dino_run = True  # 달리기로 돌아감

```
이번에는 점프 시간 자체에 리밋을 걸어보도록 했습니다.

```python
JUMP_DURATION = 1.0  # 점프 가능 시간 (초)
jump_start_time = 0  # 점프 시작 시간
```
시간은 각각 1.0초와 0으로 줬으며, 이후 1초부터 5초까지 다양하게 실험해봤습니다.

##### 버그 이유(추측)
- 이단 점프가 끝나고 다시 내려올 때, 이단 점프 타이머가 초기화되면서 **일반 점프와 이단 점프가 구분되지 않는 문제**가 발생하는 듯 보였습니다.
- 점프가 끝난 후 다시 올라가게 되거나, 이단 점프 후 상태가 제대로 처리되지 않고 일반 점프처럼 다시 처리될 수 있었고, 결국 **이단 점프가 지속**되는 문제는 해결되지 않았습니다.


#### 4차 시도
##### 아이디어: pressed_keys 딕셔너리로 이단 점프 상태 분리
점프와 슬라이딩 등의 동작을 더욱 세분화하기 위해 키 입력 상태를 별도의 `pressed_keys` 딕셔너리로 관리하며, 각 동작의 상태 전환 로직을 개선해보았습니다.  

##### 코드  
```python
pressed_keys = {
    pygame.K_UP: False,
    pygame.K_DOWN: False
}

def update(self, events, sound_manager):  # 공룡 상태 업데이트
    global pressed_keys

    for event in events:
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_UP:
                pressed_keys[pygame.K_UP] = True
            elif event.key == pygame.K_DOWN:
                pressed_keys[pygame.K_DOWN] = True
        elif event.type == pygame.KEYUP:
            if event.key == pygame.K_UP:
                pressed_keys[pygame.K_UP] = False
            elif event.key == pygame.K_DOWN:
                pressed_keys[pygame.K_DOWN] = False

    if pressed_keys[pygame.K_UP]:
        if not self.dino_jump:  # 첫 번째 점프
            self.sound_manager.play_jump()
            self.dino_duck = False
            self.dino_run = False
            self.dino_jump = True
            self.jump_vel = self.JUMP_VEL  # 점프 속도 초기화
            self.double_jump_active = True  # 이단 점프 가능 상태로 설정
        elif self.double_jump_active:  # 점프 중일 때 다시 점프 (이단 점프)
            self.sound_manager.play_jump()
            self.double_jump()
```  

##### 버그 발생
- 이렇게 코드를 고치니 메뉴에서 게임 스타트로 넘어가면 얼마 지나지 않아 게임이 꺼지는 오류가 발생했습니다.

### 생각해 본 다른 해결방법
- 일반 점프와 이단 점프를 **서로 다른 키로 분리**하면 이단 점프의 문제를 해결할 수 있을 것 같다고 생각했습니다. 
- 방향키는 일반 점프에 사용하고, 스페이스바를 이단 점프 전용으로 설정하여 두 점프의 동작을 독립적으로 관리하는 방식입니다. 이렇게 하면 점프 중에 이단 점프가 불필요하게 실행되는 문제를 방지하고, 더 직관적인 게임 플레이가 가능해질 것입니다.

#### 이단 점프 삭제 이유
- 하지만 **시간이 부족**했고, 짧은 시간 내에 로직과 게임 운영 방식을 완전히 바꾸는 것은 어려운 상황이었습니다.
- 또한, 기존 점프 로직에서 이단 점프 기능을 추가하는 것은 더 복잡한 문제를 일으킬 수 있었습니다.
- 그래서 결국 이단 점프는 삭제하고, 현재 **구현된 점프 로직을 유지**하기로 결정했습니다. 이 방식이 **게임의 안정성**을 보장하며, 향후 다른 개선을 통해 점프 시스템을 보다 효율적으로 조정할 수 있을 것으로 판단했습니다.

---

## 3. 스킨 이미지 불러오기 (main 구현 전)
### **스킨 이미지 기능 요약**  

1. **스킨 추가**:  
   - 운동 종목(농구, 승마, 탁구)을 테마로 한 공룡 스킨 3종 생성했습니다.  
<br>
2. **배경 투명화**:  
   - 각 스킨 이미지를 배경 투명화 처리해 게임에 자연스럽게 적용했습니다.  

---  

## 4. README 등의 md 파일
### Jump Dino의 문서화

#### 신경 쓴 부분
단순한 기록을 넘어, 팀원과 사용자 `모두가 쉽게 이해할 수 있는 문서`를 작성하는 것을 목표로 했습니다. 프로젝트의 `README`부터 세부 회의록, 실행 가이드까지 모든 파일이 팀의 협업을 강화하고, 사용자에게 친절한 안내서 역할을 하도록 신경 썼습니다.

### **주요 작업**  
1. **README.md**  
   - **프로젝트의 첫인상**인 만큼 깔끔한 구성과 직관적인 설명에 중점을 두었습니다. 
   - 디렉토리 구조, 브랜치 전략, 실행 방법 등을 체계적으로 작성해 개발 환경에 익숙하지 않은 사람도 쉽게 접근할 수 있도록 했습니다.  
   - ```PyCharm```과 ```Visual Studio``` 등 다양한 환경에서의 실행 방법을 상세히 설명해, 사용자 편의성을 극대화 할 수 있도록 신경을 써 작성했습니다.
<br>
2. **회의록 작성**  
   - 프로젝트 전반의 회의 내용을 꼼꼼히 기록해 팀원들이 이전 논의사항을 빠르게 확인할 수 있도록 했습니다.
   -  기능 구현 중 발생한 ```충돌(conflict)``` 해결 과정이나, 주요 결정을 문서화해 프로젝트 히스토리를 정리했습니다.
---

# 느낀점
개발을 하면서 예상치 못한 문제들이 많았고, 이를 해결하는 과정에서 많은 것을 배웠습니다. 특히, 점프 기능처럼 간단해 보이는 것도 여러 조건과 상태가 얽혀 있어 신중한 설계가 필요함을 깨달았습니다. 또한 팀원들끼리 시간을 관리하는 것의 중요성도 느꼈고, 여러 시도를 통해 더 나은 결과를 도출할 수 있다는 것을 알게 되기도 했습니다. 팀원들과 짧지 않은 기간 서로 고생하며 게임 개발을 진행했는데, 뿌듯하면서도 마지막에 기능을 제대로 수행하지 못한 것 같아 아쉽기도 했습니다.