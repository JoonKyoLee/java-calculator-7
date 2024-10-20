# java-calculator-precourse

### 🥳 사용자의 실행 흐름

1. 사용자는 구분자와 양수로 구성된 문자열을 입력한다.
    - 커스텀 구분자를 이용하지 않고 입력을 하는 경우("1,2:3")
    - 커스텀 구분자를 이용하여 입력을 하는 경우("//;\n1;2;3")
2. 입력한 문자열 중에 구분자를 잘못 입력하거나 양수가 아닌 수를 입력한다면 오류로 인한 종를 확인할 수 있다.
3. 입력한 문자열에 오류가 없다면 모든 수의 합을 결과창에서 확인할 수 있다.

### 🖥️ 프로그램 진행 흐름

1. 계산기를 시작하고 사용자에게 문자열을 입력받는다.
    ``` 
    "덧셈할 문자열을 입력해 주세요."
    ```
2. 입력받은 문자열을 확인하고 해당 문자열에 잘못된 부분이 있는지 확인한다.
    - 기본 구분자는 쉼표(,) 또는 콜론(:)이다.
    - 사용자는 //과 \n 사이에 원하는 구분자를 지정할 수 있다.
    - 양수만 입력할 수 있다.

3. 구분자를 기준으로 각 숫자를 분리하고 각 숫자의 합을 계산한다.
4. 계산한 수를 결과창에 출력해서 사용자가 볼 수 있도록 한다.

### 🚨 계산기 실행 중 예외처리 사항 정리

```
사용자가 입력한 문자열에서 구분자와 수에 대한 예외처리를 진행해야 한다. 
사용자가 잘못된 값을 입력할 경우 IllegalArgumentException을 발생시키고 애플리케이션은 종료되어야 한다.
```

- 구분자 예외처리
    - 커스텀 구분자가 없는 경우
    - [x] 쉼표(,) 또는 콜론(:) 외에 다른 문자를 입력한 경우(1,2:3;4)
    - [x] 구분자를 연속해서 입력한 경우(1,,2:3)
    - 커스텀 구분자가 있는 경우
    - [x] 커스텀 구분자와 쉼표(,) 또는 콜론(:) 외에 다른 문자를 입력한 경우(//;\n1;2,3-4)
    - [x] 구분자를 연속해서 입력한 경우(//!\n1!2!!9,,2) 또는 구분자를 가장 앞에 입력한 경우(//-\n-1-2-3)
        - [x] 커스텀 구분자가 -일때는 연속해서 입력하면 음수로 처리할 수도 있지만 구분자를 연속해서 입력한 경우로 예외처리(//-\n)
- 숫자 예외처리
    - [x] 숫자가 아닌 문자가 포함된 경우(1,2,ㄱ:5) -> 구분자 예외처리 시에 다른 문자 입력한 경우에서 같이 처리됨
    - [x] 입력한 문자열에 0이 있는 경우(1,2,0:3)
    - [x] 입력한 문자열에 음수가 있는 경우(-1,2,3)

### 💻 구현 목록 정리

#### Domain

- [InputData]
    - 기본 구분자(쉼표, 콜론), 커스텀 구분자와 숫자를 저장한다.
    - [x] 생성자에서 사용자에게 입력받은 문자열과 기본 구분자를 처리한다.
    - [x] 기본 구분자와 커스텀 구분자를 이용해 문자열을 분리한다.
    - [x] 분리한 문자열을 바탕으로 숫자를 저장한다.
        - [ ] 숫자를 저장할 때 배열의 길이가 1이고 비어있는 문자열을 저장하고 있을 때는 0으로 변환해서 저장한다.(""를 입력했을 경우)
- [SeparatorValidator]
    - 구분자에 대한 유효성을 검증한다.
    - [x] 커스텀 구분자가 존재하는지 확인한다.
    - [x] 커스텀 구분자가 있는 경우에는 커스텀 구분자를 추출한다.(//;\n에서 //와 \n을 분리해 ;를 확인)
    - [x] 기본 구분자와 커스텀 구분자에 대해 예외처리를 진행한다.
        - [x] 기분 구분자와 커스텀 구분자 외에 다른 문자를 입력한 경우
        - [x] 구분자를 연속해서 입력한 경우
- [NumberValidator]
    - 숫자에 대한 유효성을 검증한다.
    - [x] 숫자가 아닌 문자가 포함된 경우(1,2,ㄱ:5) -> 구분자 유효성 검증시 같이 처리됨
    - [x] 입력한 문자열에 0이 있는 경우(0,1,2)
    - [x] 입력한 문자열에 음수가 있는 경우(-1,2,3)

#### View

- [InputView]
    - 사용자에게 문자열을 입력받는다.
    - [x] 사용자에게 입력 안내 문구를 보여주고 문자열을 입력받는다.

- [OutputView]
    - 사용자에게 결과를 출력한다.
    - [ ] 사용자에게 결과 문구와 덧셈한 수를 출력한다.

#### Controller

- [Calculator]
    - 프로그램의 전체적인 흐름을 관리한다.

### 흐름에 맞춰 구현하기

1. 사용자에게 문자열을 입력받는다.(InputView)
   ```
   덧셈할 문자열을 입력해 주세요.
   ``` 
2. 사용자가 입력한 문자열에서 구분자 예외처리를 진행한다.
3. 구분자 예외처리 진행 후, 구분자를 이용해 문자열을 분리하고 숫자에 대한 예외처리를 진행한다.
4. 덧셈을 진행한다.
5. 사용자에게 덧셈 결과를 출력한다.
   ```
   결과 : 6
   ``` 
