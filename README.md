<h1 align="center">
  Banksalad
</h1>

### Any assumptions - 추정한 부분

- 성공 시나리오는 제외하고 실패 시나리오만 생각한다.
- 약관 동의 및 확인 테스트는 제외한다.
- PC 성능 문제는 제외한다.

### 툴과 언어 선택에 대한 이유

- Windows : Windows OS 사용자가 상대적으로 Mac OS 사용자보다 많음
- Python : 사용이 쉬움, 빠른 개발속도, 높은 확장성 및 이식성
- Selenium : 다양한 프로그래밍 언어를 지원 , 실제 사용자 동작 시뮬레이션 가능
- Vscode : 다양한 프로그래밍 언어를 지원 , 무료
- Chrome : 가장 점유율이 높은 브라우저


### 실행 환경 및 설정에 대한 안내

1. Vscode 설치 후 [링크](https://docs.python.org/ko/3/library/venv.html)를 참조하여 Python Venv 가상 환경 생성
```bash
python -m venv selenium
```

2. Python Venv 가상 환경 접속
```bash
cd selenium\Scripts 진입 후 activate 또는 .\activate 입력
```

3. 셀레니움 설치
```bash
pip install selenium
```

[링크](https://chromedriver.chromium.org/)를 참조하여 크롬 브라우저 드라이버 셋팅

4. chromedriver.exe 파일을 selenium 폴더 안으로 복사

5. {파일명}.py 파일을 생성하여 코드 작성

### 테스트 실행 방법에 대한 안내

1. 가상환경 접속
```bash
cd Selenium\Scripts 진입 후 activate 또는 .\activate 입력
```

2. 이전 폴더로 돌아와서 python {파일명}.py 명령어 입력
```bash
ex.) (selenium) C:\dane\selenium>python bank.py
```

### 다음 작업 또는 개선할 부분들에 대한 설명

샘플코드를 제외 한 네가지 실패 케이스에 대한 코드 추가 작성 필요
- 이름 오류
- 휴대폰번호 오류
- 통신사 오류
- 인증코드 오류

### 샘플 코드 [동영상](https://drive.google.com/file/d/1oVj4Bl4pgXiPc84RVcXIQZ8DdOMnD-0z/view?usp=sharing)

```bash
import time
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

def wait_until_id(id_str):
    WebDriverWait(browser, 30).until(EC.presence_of_element_located((By.ID, id_str)))
def wait_until_cssselector(cssselector_str):
    WebDriverWait(browser, 30).until(EC.presence_of_element_located((By.CSS_SELECTOR, cssselector_str)))

options = webdriver.ChromeOptions()
options.add_experimental_option("excludeSwitches", ["enable-logging"])
browser = webdriver.Chrome(options=options)

# 주민등록번호 오류에 대한 코드
try:
    Case2 = "주민등록번호 오류"
    browser.maximize_window() # 브라우저 최대화
    browser.get('https://www.banksalad.com/prequalification/loans/credit/authentication') # URL 이동
    wait_until_id('name') # 특정 ID 노출까지 대기
    browser.find_element(By.ID, 'name').send_keys('강동규') # 이름 입력
    browser.find_element(By.ID, 'rrn').send_keys('9302121') # 주민등록번호 앞자리 + 뒷자리 1 입력
    for i in range(6): # 주민번호 뒷자리 임의의 숫자로 6회 반복
            browser.find_element(By.CSS_SELECTOR, '.\_stealien_keybutton_36').click()
    browser.find_element(By.ID, 'phoneNumber').send_keys('01099993074') # 휴대폰번호 입력
    browser.find_element(By.CSS_SELECTOR, '.css-1u9v6ej:nth-child(3)').click() # 통신사 선택
    browser.find_element(By.CSS_SELECTOR, '.bg-green-100').click() # 다음 버튼 입력
    wait_until_cssselector('.css-1pigmyb:nth-child(5) > .bg-green-100') # 특정 css 노출까지 대기
    browser.find_element(By.CSS_SELECTOR, '.css-1pigmyb:nth-child(5) > .bg-green-100').click() # 동의하고 진행
    time.sleep(3)
    browser.close()
    print(str(Case2) + '테스트 완료')
except:
    print(str(Case2) + '시나리오 문제 발생')
```
