## 목차
1. VSCode 다운로드 및 사용하기
2. Python 컴퓨터에 설치하기
3. 모든 ETF 상품들의 Adj Close 가격을 확인할 수 있는 프로그램 소스코드 
4. 원하는 ETF 상품과 기간을 입력하여 Adj Close 가격을 불러올 수 있는 프로그램 소스코드

## VSCode 다운로드 및 사용하기
1. https://code.visualstudio.com/download 접속합니다.
2. Windows - Windows 7, 8, 10 버튼 누르면 자동으로 진행됩니다.

## Python 컴퓨터에 설치하기 - 이미 Python 3 이상 버전이 있는 경우 패스
1. https://www.python.org/downloads/windows 접속합시다.
2. ctrl + f 를 누른 후, 3.6.5 를 검색하면 밑에 **Download Windows x86-64 executable installer** 라는 버튼이 있는데, 이를 클릭하면 자동으로 다운로드가 됩니다.
    - (굳이 Python 3.6.5 버전을 다운로드 받는 이유는 가장 에러가 없는 버전 중 하나이기때문입니다. 제가 이 버전을 사용하여 만들었기 때문에 가장 매끄럽게 실행될 것입니다.)

3. 아래 상태표시줄에서 검색 버튼을 클릭하여 *cmd* 를 입력하면 '명령 프롬프트'가검색됩니다. 이를 클릭합시다.
4. 아래의 순서대로 프롬프트에 입력하여 각각의 module을 설치합니다.
**pip install datetime**
**pip install pandas**
**pip install pandas_datareader**
**pip install yfinance**

## 모든 ETF의 Adj Close 가격을 확인할 수 있는 프로그램(전일과 비교하는 프로그램)
1. 바탕화면에 *Python_codes* 라는 폴더를 만듭니다.
2. 카카오톡을 통해 별첨한 *TickersY.txt* 라는 파일을 이 폴더에 넣어줍시다.
3. VSCode를 실행하여 *ctrl+n*, *ctrl+shift+s* 를 차례로 눌러 **adjclose.py**라는 파일을 위 폴더에 생성하여 아래의 코드를 '복붙' 하여 저장합니다.
```Python
import os
import datetime
from pandas_datareader import data as pdr
from pandas.testing import assert_frame_equal
import csv
import yfinance as yf
yf.pdr_override()
now = datetime.datetime.now()

f1 = open("TickersY.txt",'r')
ticker = f1.read().split("\n")
f1.close()
data = pdr.get_data_yahoo(ticker, period="2d", interval="1d")
data = data['Adj Close']

data.to_csv("Adj_Close"+"_"+str(now.year)+"_"+str(now.month)+"_"+str(now.day)+"_"+str(now.hour)+"_"+str(now.minute)+"_"+str(now.second)+".csv")
```
4. 명령 프롬프트를 실행하여 아래와 같이 입력합니다. 
**cd desktop/Python_codes**
**python adjclose.py**
5. 이를 입력하면 해당 폴더에 *Adj_Close_(날짜 및 시간).csv* 라는 파일이 생성되며, 이를 열면 각 ETF마다의 최근 이틀간 Adj Close 가격이 기록되어 있을것입니다.

## 알아보고 싶은 ETF들 및 원하는 기간을 입력하여 Adj Close 가격을 알아보는 프로그램
1. *Python_codes* 폴더 안에 역시 위와 같은 방법으로 *search_adjclose.py* 라는 파일을 생성합니다.
2. 아래의 코드를 한번 더 복붙하여 저장합니다.

```Python
import os
import datetime
from pandas_datareader import data as pdr
from pandas.testing import assert_frame_equal
import csv
import yfinance as yf
yf.pdr_override()
now = datetime.datetime.now()

print("Enter tickers in uppercase letters (A B C D ...)")
tickers = input().split()

print("Starting date(YYYY-MM-DD)")
stdate = input()

print ("End date(YYYY-MM-DD)")
enddate = input()

data = pdr.get_data_yahoo(tickers, start=stdate, end=enddate)
data = data['Adj Close']

data.to_csv("Tickers_Adj_Close"+"_"+str(now.year)+"_"+str(now.month)+"_"+str(now.day)+"_"+str(now.hour)+"_"+str(now.minute)+"_"+str(now.second)+".csv")
```

3. 명령 프롬프트를 실행하여 아래와 같이 입력합니다.
*cd desktop/Python_codes*
  - 이미 해당 폴더로 directory를 설정한 상태라면 여긴 스킵해도 됩니다.
4. *python search_adjclose.py* 를 입력합니다.
5. 첫 번째로, 원하는 상품의 명칭을 개수에 상관 없이, 쉼표 없이 입력합니다(ex. EWZ ARGT EMLC LQD).
6. 원하는 기간의 시작 날짜를 입력합니다(ex. 2019-02-24)
7. 원하는 기간의 마지막 날짜를 입력합니다(ex. 2020-02-24)
8. 해당 폴더에 *Tickers_Adj_Close_(날짜).csv* 파일이 생성될 것입니다. 이 파일을 열면 원하는 ETF 상품의 해당 기간 동안의 Adj Close 가격이 알파벳 순서로 기록될 것입니다.

###### 해당 사용설명서대로 진행되지 않을 경우, 주저하지 마시고 연락 주시길 바랍니다. 유용하게 사용하여 주시고, 나름 지적 자산이니 외부에 유출하진 말아주세요 ㅋㅋㅋㅋㅋ 화이팅합시다ㅎㅎ