---
layout: post
title: 소프트웨어 공학 기본-1
subtitle: 
gh-repo: 
gh-badge: [star, fork, follow]
tags: 소프트웨어 공학
categories : SW_engineering
comments: true
---

> 모듈화 (Modularity)

- 정의
  \- 소프트웨어를 각 기능별로 분할, 설계 및 구현기법
- 특징
  \- 소프트웨어의 복잡도 감소 & 성능 향상
  \- 효과적인 유지보수 (추상화, 캡슐화)
  \- 모듈의 독립성
  \- 적정한 모듈의 수 유지가 관건

![sw_engineering](..\assets\post_img\sw_engineering.png)

모듈의 수와 비용+노력의 상관관계 그래프

------

> 결합도 (Coupling)

- 정의
  \- **어떤 모듈이 다른 모듈에 의존하는 정도**를 측정하는 척도
  \- **독립적인 모듈**이 되기 위해서는 **결합도가 약해야한다.**

![Image for post](https://miro.medium.com/max/60/0*VzcKuLZHvJzFd_6Q?q=20)

![sw_engineering2](..\assets\post_img\sw_engineering2.jpg)

```
결합도순서(약함 -> 강함)
1. 자료 : 단순자료만 매개변수로 전달 & 참조
2. 스탬프 : 자료구조(배열,리스트,레코드,객체 등)를 매개변수로 전달 & 참조
3. 제어 : 논리적 흐름을 제어하기 위한 제어 플래그(flag)나 정보를 전달
4. 외부 : 외부 환경(특수H/W, 통신 프로토콜, OS, 컴파일러등)과 연관된 경우
5. 공통 : 두 모듈이 같은 글로벌 데이터(전역변수)를 공유하는 경우
6. 내용 : 한 모듈이 다른 모듈의 내부 기능 및 자료를 직접 참조하는 경우
```

------

> 응집도 (Cohesion)

- 정의
  \- **하나의 모듈이 하나의 기능을 수행하는 요소들간의 연관성 척도**
  \- **독립적인 모듈**이 되기 위해서는 **응집도가 강해야한다.**

![Image for post](https://miro.medium.com/max/60/0*pXuItGd0WkYZsUji?q=20)

![sw_engineering3](..\assets\post_img\sw_engineering3.jpg)

```
응집도순서(강함 -> 약함)
1. 기능적 : 모듈 내 모든 요소들이 단일 기능을 수행
2. 순차적 : 모듈 내의 한 요소의 출력 자료가 다음 요소의 입력 자료로 사용
3. 교환적 : 모듈 내의 요소들이 동일한 입출력 자료로 서로 다른 기능을 수행
4. 절차적 : 모듈 수행 요소들이 반드시 특정 순서대로 수행
5. 시작적(일시적) : 특정 시간에 실행되는 기능들을 모아 작성된 모듈
6. 논리적 : 논리적으로 유사한 기능을 수행 하지만 서로의 관계는 밀접하지 않음
7. 우연적 : 모듈 내 요소들이 뚜렷한 관계가 없이 존재
```

------

> 결론적으로, 소프트웨어 공학에서는 결합도를 낮추고, 응집도를 높이는 것이 좋은 SW를 만들 수 있다고 말한다.(단, 모듈화에 너무 치중하는 것은 개발자의 노력과 비용을 많이 들게 하므로 적절하게)