---
layout: post
title: "그냥 끄적여보자!"
subtitle: "다시 끄적여 보자!"
categories: writing
tags: writing
---

### 그냥 끄적임
 마지막으로 글을 쓴게 3월 31일 벌써 한 달이 넘어 버렸다. 뭔가 쉽게 쓸 수 있는 시스템을 구축 하면 글을 계속 해서 쓸 줄 알았는데... 생각보다 잘 안된 것 같다.
그래도 약 45일.. 기억이 완전히 사라지진 않는 시간인 것 같다. 그래도 ATOM, Linux, git명령어 등이 어렴풋이 남았다. 이대로 시간이 45일이 더 지나가 버리면 기억에 남는게 거의 없을지도 모른다. 지금이라도 다시 ATOM 편집기 안에서 끄적이는 것이 다행이라 생각이 든다.
그냥 끄적이는 것이니깐 나에게 중요한 것이 무엇인지 다시 한 번 적어보자. 난 이 것을 좋아한다.

### 나의 미래?
나의 미래에서 가장 중요한 것은 무엇일까? 우선 밥벌이겠다.
밥벌이를 하려면 지금 하고있는 일을 계속해야 할 것인데... 지금 하는일은 나름 잘 하고 있지만 과연 대체 불가능한 일인가? 생각 해보면 그렇지 않다. 누구나 우리팀에 와서 성실하게 지내다 보면 기본은 할 수 있을것으로 보인다. 그 이상의 능력이 있어도 좀 편해지고 장애가 덜 나고 장애복구 잘하고.. 뭐 그런 일들이지.. 역시 그 이상의 일은 아니라 본다. 그래도 우리팀 일에 대해 나름 정리를 해보면 다음과 같다.

### 우리팀 일
> _"무선이동통신의 core 역할을 하는 서버장비를 운영하는 업무"_

무선이동통신의 일부분을 담당하고 있다. 팀 크기로 보면 일 부분이지만 가장 core가 되는 부분이다 보니 그 중요성은 꽤 높은 편이다. 간단하게 설명을 하면 선이 연결되지 않은 A,B라는 단말이 무선통신을 하기 위해서는 중간에 거치는 부분이 많다. 단말끼리 직접 통신을 하는게 아니다.(물론 WIFI 테더링, 블루투스 등 근거로 통신도 있지만) 단말은 가장 가까이 있는 중계기 안테나와 통신을 한다. 정확히 이 부분만 무선 통신이다. 그 이후로는 모두 선으로 연결이 되어있다. 3G니 4G니 하는 것은 이 구간의 얘기다. 이 짧은 구간에서 얼마나 빨리 처리 하느냐가 중요하다. 그 이후로는 중계기 -> 기지국 -> 교환기 -> 우리팀장비들과 차례 대로 연동하게 되고 그 뒷 구간은 역순이다. 앞서 말했듯이 단말과 단말이 직접 통신 하는것이 아니고 중간에 거쳐서 통신한다. 거의 대부분의 서비스는 우리팀 장비를 거친다고 보면 된다.**그래서 전체 서비스에대해 러프하게라도 이해하고 있어야 한다.**
우리팀의 장비는 전부 서버장비이다. 물론 서버간 통신을 위해 중간에 스위치/라우터 장비들이 많이 있다. HP/ORACLE 등의 H/W에 제조업체에서 만든 서버위에 Linux/Unix등의 OS 올라가고 역할에 맞는 Application을 개발하고 올려놓는다. 가입자 등의 저장 할 내용이 있을 경우 DB 도 올린다. 이와 같은 서버와 스위치를 장애 안나게 잘 운용하는 것이 우리의 역할이다. 새로운 서비스가 나오거나 하면 해당 내용을 Application에 적용하게 되는데(개발팀 역할), 우리는 이렇게 적용된 PKG이 문제없이 잘 동작하는지 관리한다. **S/W적으로 문제있는지여부** 를 아는 방법은 다양하게 있다. 첫번째는 VOC라 불리는 민원이다. 특이한 민원이 들어오거나 특정CASE민원이 늘어나거나 하면 시스템 문제를 의심해 볼 수 있다. VOC를 보고 시스템의 문제점을 확인하려면 서비스에대한 이해도와 **다량의 VOC에서 문제점을 뽑아 낼 수 있는 데이터 분석능력이 필요하다** 대게는 엑셀을 잘 다룰 줄 알면된다. **H/W 문제** 또한 중요하다. **다양한 서버와 OS의 특성에 대해 이해하고 명령어등을 파악하고 있어야 한다.** 그렇다고 뭐 대단한건 아니고 ADMIN 1 수준과 경험적인 지식만 있으면 된다. 마지막으로 **N/W문제** 에 대한 이해가 필요하다. **서버간통신을 위한 명령어 집선되어있는 스위치에서의 명령어 라우터 등의 명령어에 대해 알고 있어야한다.** 사실 이것도 매우 기본적이다.
이 외에 위에서 시키는 다양한 보고서 작성능력과 이력관리 능력 타팀과 협업할 수 있는 약간의 친화력 등이 필요하다고 할 수 있다.

위 능력은 누구나 쉽게 갖출 수 있다고 본다... 그리고 기본능력이상 잘 발달되지도 않는다.. 재미도 없고.. 뭔가 열정을 불태우며 집중 할 수 있는 일을 하고 싶다.

### 그래서 관심을 갖고 있는 일
> _"누구나 쉽게 할 수 없으면서 나의 열정을 태울 수 있는 일들"_

**코딩쪽을 파고싶은 생각이 있다.** 하지만 기능적인 부분은 많이 늦었을 수 있다. 이미 이쪽으로 몸 담은 친구들은 거의 10년 가까이 현장에서 일을 했을것이다. 툭 거들면 탁 하고 어느정도 수준의 프로그래밍 결과물이 나올 것이다. 이것을 따라잡기 위해서 코딩 공부를 열심히 한다?? 시간도 오래걸릴 뿐더러 취미로 한다면 더욱 그러할 것이다. 물론 단순 코딩업무와 달리 생각하면서 하는 코딩이라면 변별력이 있을 수 도 있다. 그렇다 하더라도 **지금 올인하기에는 시간 낭비가 많을 것** 이다.

**AWS등을 통한 인프라 구축에 대한 공부를 하고싶다.** 아무래도 지금 하는 일이 인프라 장비들을 운용하는 것이다 보니 좀더 접근하기 수월하고 재미도 있을 것 같다. 특히 AWS 와 같은 클라우드 서비스는 많이 성장하고 있고 앞으로도 이러한 형태로 대체될 가능성이 높기 때문에 본업이 아니더라도 잘 알 고 있을 필요가 있다. 이쪽 전문가에 대한 수요도 많이 있으리라 생각이 든다. 하지만 기존의 이쪽 종사자들도 누구나 쉽게 접근이 가능하기 때문에 이 서비스를 단순히 다룰 수 있는 정도의 지식이라면 과연 남들과 차별성이 있느냐에 대해서는 의구심이든다. 그렇다 하더라도 **기본적인 지식은 반드시 갖출 가치가 있다고 판단된다.**

**빅데이터를 다루는 능력도 필요하다** 가장 단순하게 생각하면 내가 지금 하고있는 VOC분석도 나름 데이터를 다루는 일이다. 엑셀로 하고 있지만 큰 데이터에서 의미 있는 부분을 찾아내는 것. 데이터가 클 수록 그 의미는 더 중요해 질 것이다. 이미 빅데이터에 대한 언급은 많이 되고 있고 빅데이터 분석가 등 직업으로써도 많이 알려진 편이다. 무언가로부터 의미를 찾는 다는 것이 상당히 흥미가 가는 부분이다. R프로그래밍으로 알려진 데이터를 다루는 도구가 있다. 하둡?도 유명한 도구 인 것 같고.. **이 능력도 무슨일을 하든 갖고있으면 삶에 크게 도움이 될 것 같다.** R프로그래밍이 요즘 핫하니깐 한번 배워보자!

**보안쪽일도 멋있어 보인다.** 사실 이부분은 내가 알고 있기보다는 멋있다고 보는 부분이다. 순전히 어릴때 로망 이랄까? 영화에서 해커들이 노트북을열고 막혀있는 보안을 뚫고 들어가는 모습? 컴퓨터쟁이들이라면 누구라도 멋져보였을 것이다. 물론 실상은 그렇지 않겠지만 ㅎㅎ. 스노든이라는 사람을 알고 NSA 등 보안에 대해 많은 관심을 갖게 되었다. 그러다 **Block Chain** 이라는 매력적인 기술도 알게 되었고, 또한 이것을 통해 전국민 투표시스템을 만드는 등의 재밌는 생각도 해보았다. 과연 이쪽 분야에 대해 어떻게 공부를 해야할 지 감이 오진 않지만 그래도 **Block Chain 기술만큼은 깊게 공부해보고 싶다**

기술적인 부분들은 위와 같고... 과연 난 그럼 무슨일을 해야할까? 위 기술들은 한분야만 파려고 해도 끝이없다. 이미 계속해서 새로운 기술들이 무궁무진하게 쏟아지고 있고 깊게 파기에 전문성이 뛰어난 것도 아니니깐.. 그렇다고 전부 대충알고 일하는 우리회사 임원들처럼 되고 싶지는 않다. 아무리 잘 알수는 없더라도 깊이가 있어야 그만큼 서비스를 하나 만들더라도 완성도가 높아진다고 생각한다. 특히 우리나라에서는 전문성이 너무너무 떨어지고 다들 되는데로 일하기 때문에 정말 멋진 일을 하기 위해서는 나부터라도 일정 수준이상의 전문성을 갖춰야 한다.! 다시.. 그럼 난 무슨일을 할까?

### 그래서 해보고 싶은 일

그래 한번 **End to End 서비스** 를 한번 만들어보자. 어떤 것을 하든 기본이 있고 그 위에 쌓아갈 수 있는 법이다. 주제를 정하고 AWS로 구축하고 그위에 Python으로 코딩한 프로그램을 올리고 아두이노 같은 단말을 이용해서 뭔가를 만드는 것이다. 그 와중에 R프로그램이을 사용하기도 하고 보안도 신경쓰면서 말이지 그냥 다 공부할 수 있는 뭔가를 한번 만들어 보자. 그리고 그것을 해내게 되면 그 경험을 바탕으로 **창업** 을 해도 되고 아니면 **사업부서** 이동해서 안정적으로 사업을 해도 좋다. 또 하다보면 그 중에 매력적인 서비스는 좀더 깊게 공부하고 그 쪽으로 빠질 수 도 있을것이다. **중요한 것은 나의 능력을 키우는 것이다. 남에것 배끼지 말고 내스스로 능력을 키우고 쌓아가 보자** 그리고 그 과정을 여기다 적어보면서 지식을 쌓는 과정을 진행해 보자.

기왕이면 팀을 꾸려서 해보면 좋을 것 같다. 같이하면 재밌잖아. 내가 생각하지 못하는 부분을 누군가는 생각할 수 도 있고.. 그리고 아마 대부분 나처럼 현실의 삶에서 지겨워 하고 있을지도 모른다. 뭔가 태우고싶을거야!
일단 가까이에 우리팀 사람들을 데리고 뭔가 해보면 재밌겠다. 프로그램쪽일은 대학교친구들한테 물어봐도 좋을것 같다. 크로닉스애들한테도 재미있는 아이디어 요구도 해보고 말이지!
