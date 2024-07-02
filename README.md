# [전자액자] image_carousel

위젯 생명 주기, PageView, Timer, SystemChrome, StatefulWidget

<br>

## 1️⃣ 위젯 생명 주기

| StatelessWidget  | StatefulWidget                   |
| :--------------- | :------------------------------- |
| 상태가 없는 위젯 | 위젯 내부에서 build()함수 재실행 |

<br>

## 2️⃣ PageView

```
@override
Widget build(BuildContext context) {
    SystemChrome.setSystemUIOverlayStyle(SystemUiOverlayStyle.light);
    return Scaffold(
      body: PageView(
        controller: pageController,
        children: [1, 2, 3, 4, 5]
            .map(
              (number) => Image.asset(
                "asset/img/image_$number.jpeg",
                fit: BoxFit.cover,
              ),
            )
            .toList(),
      ),
    );
  }
```

#### 💡 코드 자세히 보기

| ✨.map((number) => ~ )            | .toList()                                                                                                                                                                                                                              |
| :-------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| map() 함수로 image 1 ~ 5까지 추가 | 이터러블 객체를 리스트로 변환하는 함수로,<br> map은 이터러블 객체를 반환하므로 이를 리스트로 변환하기 위해 사용<br> (이터러블 객체란 List, Set, Map과 같이 여러 개의 요소를 가지며<br> 이 요소들을 하나씩 꺼내 쓸 수 있는 타입의 객체) |

<br>

## 3️⃣ 상태바 색상 변경 (SystemChrome 함수)

```
SystemChrome.setSystemUIOverlayStyle(SystemUiOverlayStyle.light);
```

| setEnabledSystemUIMode()                               | setPreferredOrientations()                       | setSystemUIChangeCallback()      | setSystemUIOverlayStyle() |
| :----------------------------------------------------- | :----------------------------------------------- | :------------------------------- | :------------------------ |
| 앱의 풀스크린 모드 지정 <br> (폰 상단바 안보이게 가능) | 앱 실행 방향 설정 <br> (가로, 좌우반전, 세로 등) | 시스템 UI 변경되면 콜백함수 실행 | 시스템 UI 색상 변경       |

<br>

## 4️⃣ Timer

| Timer()     | Timer.periodic()          |
| :---------- | :------------------------ |
| 기본 생성자 | 주기적으로 콜백 함수 실행 |

```
 PageController pageController = PageController();
  @override
  void initState() {
    Timer.periodic(
      const Duration(seconds: 1),
      (timer) {
        int? nextPage = pageController.page?.toInt();
        if (nextPage == null) {
          return;
        }
        if (nextPage == 4) {
          nextPage = 0;
        } else {
          nextPage++;
        }
        pageController.animateToPage(
          nextPage,
          duration: const Duration(seconds: 1),
          curve: Curves.ease,
        );
      },
    );
    super.initState();
  }
```

#### 💡 코드 자세히 보기

1. **부모 initState() 실행** : 모든 initState() 함수는 부모의 InitState() 함수를 실행해야 함

```
 void initState() {
  super.initState();
 }
```

2. **Timer.periodic() 등록**

```
Timer.periodic(
      const Duration(seconds: 1),
      (timer) {}
)
```

3. **PageController 생성**

```
PageController pageController = PageController();
```

```
    return Scaffold(
      body: PageView(
        controller: pageController,
```

4. **현재 페이지를 가져오고, 조건별 예외 및 분기 처리**

```
 int? nextPage = pageController.page?.toInt();
        if (nextPage == null) {
          return;
        }
        if (nextPage == 4) {
          nextPage = 0;
        } else {
          nextPage++;
        }
```

5. **페이지 변경**

```
        pageController.animateToPage(
          nextPage,
          duration: const Duration(seconds: 1),
          curve: Curves.ease,
        );
```

   <br>

## 5️⃣ 그 외

#### 🖼 asset에 추가한 이미지 사용하기

1. asset 폴더 생성 후 img 폴더 생성
2. img 폴더 안에 이미지 끌어다 넣기
3. pubspec.yaml에 아래 코드 추가하여 에셋 등록하기

```
assets :
   - asset/img/
```
