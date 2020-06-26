## 입력 Form은 어떻게 받아오게 하지?

> [REST 아키텍처를 훌륭하게 적용하기 위한 몇 가지 디자인 팁](https://spoqa.github.io/2012/02/27/rest-introduction.html)에서 발췌

새로운 아이템을 작성하거나 기존의 아이템을 수정할 때 작성/수정 Form은 어떻게 제공할까.

정답은 <strong>Form 자체도 정보로 취급해야 한다</strong>는 것입니다. 서버로부터 `새로운 아이템을 작성하기 위한 Form을 GET한다`고 생각하시면 됩니다.

Rails 에선 기본적인 CRUD를 제공할 때 아래와 같은 REST 인터페이스를 구성해줍니다.
