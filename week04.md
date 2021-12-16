# 1004 - class 

## About Roto

* Roto node : 커브로 (shape 딸 때 등)
* Roto paint node : 커브, 페인트 모드 (배경 클리닝, 상처지우기 등)

* 로토스코핑 할 때 주의할 점 
  - 레퍼런스로 쓸 프레임 찾기(모션블러가 적은 부분, 전체적인 shape이 잘 드러나는 부분)
  - 많은 양의 점을 찍지 말 것
  - 배경과의 경계에서 배경 색이 묻지 않도록


* 로토 노드 사용
  - 센터는 ctrl 키 누르면 움직일 수 있다.
  - 전체 선택은 Ctrl + A
  - Q : overlay 해서 커브 숨김 수 있음
  - premult node 연결해서 알파 확인
  - smooth = z
  - 페더 값(블러느낌) = E
  - 마디마디, 부분부분 나눠서 로토 따줄 것

* 로토페인트 노드 사용
  - clone 작업 : ctrl
  - shift : 브러쉬 사이즈 조절
  - 투명도 1일 떄 레퍼런스가 그대로 들어온다
  

--
## Merge Operation

출처 : https://learn.foundry.com/nuke/13.0/content/comp_environment/merging/merge_operations.html

  - A = Input A / a = alpha of A (여기서 A가 반드시 premult - 데이터의 일부를 알려줘야 하기 때문)
  - B = Input B / b = alpha of B


* over : A+B(1-a)
  - (1-a) = A 알파의 invert
  - (1-a)에 B를 곱해주면 B 자리에 A의 자리가 생김 -> 그 자리에 A가 쏙 들어감
  - (1-a)의 자리와 A의 알파 자리가 어긋나면 이미지가 틀어지게 된다.
     -> 각각에게 transform node를 clone 해주거나 translate 값을 옮겨주어야 함
  
* stencil : B(1-a)
  - A의 자리까지만 만들어주는, over에 사용하는 B
  - plus A+B 라는 수식과 합쳐주면 over A+B(1-a)의 수식이 된다.

* atop : Ab+B(1-a)
  - A와 B가 겹쳐진 채로, B의 모양으로만 보인다.

* average : (A+B)/2
  - A와 B의 평균 : 원본 이미지보다 어둡게 보인다.

* color-burn : darken B, towards A
  - B가 A의 휘도에 따라 어두워진다.

* color-dodge : brighten B, towards A
  - B가 A의 휘도에 따라 밝아진다.

* conjoint-over : A+B(1-a/b), A if a>b
  - A와 B가 겹쳐진 부분에서 A가 B를 완전히 덮는다.
  - Normal over은 약간 투명한 경계선을 만든다.

* copy : A
  - A만 보인다.
  - B의 일부가 보이도록 믹스하거나 마스크 컨트롤 할 때 유용함.

* difference : abs(A-B)
  - 겹쳐진 부분의 픽셀이 얼마나 다른지.
  - Merge -> Merge -> Absminus 와 같음
  - 유사한 두 이미지 비교할 때 유용함.

* disjoint-over : A+B(1-a)/b, A+B if a+b<1
  - 겹쳐진 상황이지만, 겹쳐지지 않았다고 가정함.
  - Normal over은 약간 투명한 경계선 형성함.
  - a에 b가 이미 보류되어 있는 경우 유용함(각 개체에 다른 개체가 렌더링 되지 않도록)
  - over 연산 사용하면 어두운 선이 생성됨(배경 이미지 유지 됨)

* divide : 	A/B, 0 if A<0 and B<0
  - 두 개의 음수 값이 양수가 되지 않도록 한다.
  - 곱하기 취소하는데 사용가능
  - 겹쳐지지 않은 B부분이 사라짐.

* exclusion : A+B-2AB
  - A와 B가 곱해진 부분이 (점차적으로) 빠진다.

* from : B-A
  - B에서 A가 빠진다.

* geometric : 2AB/(A+B)
  - A와 B가 겹쳐져서 곱해진 부분 만이 보인다.
  - 두 이미지를 평균화 하는 방법 중 하나

* hard-light : multiply if A<0.5, screen if A>0.5
  - B가 A모양에서 밝고 선명하게 겹쳐진 부분이 보인다.

* hypot : sqrt (A*A+B*B)
  - B와 A가 겹쳐진 부분이 화면보다 밝음.
  - 1이상의 값으로 작동하는 hypot
  - 스크린 대안으로 반사를 추가하는데 유용함

* in : Ab
  - 겹쳐진 부분에서 A만 보임
  - Merge -> Merge -> in 의 수식과 같음
  - 매트를 결합하는데에도 유용함

* mask : Ba
  - 겹쳐진 부분에서 B만 보임

* matte : Aa+B(1-a)
  - Premultied over
  - Merge -> Merge -> Matte 의 수식과 같음

* max : max (A,B)
  - 겹쳐진 부분의 최대값을 취한다.
  - Merge -> Merge -> Max 와 같음
  - 매트 결합하기 좋은 방법 중 하나(밝은 머리카락의 부분을 가져오는데 유용함)

* min : min (A,B)
  - 겹쳐진 부분의 최소값을 취한다.
  - Merge -> Merge -> Min 과 같음.

* minus : A-B
  - B에서 A를 뺀다.

* multiply : AB, A if A<0 and B<0
  - 두 개의 음수 값이 양수가 되지 않도록 해야함
  - 두 이미지에서 겹쳐진 부분이 멀티플라이 되어 남음
  - 흰색 배경 위에 짙은 회색 연기와 같이 A가 더 어두울 때 B의 이미지와 합성하여 사용
  - F_Regrain으로 다시 배합된 이미지에 곡판을 추가하는 데도 유용함

* out : A(1-b)
  - A에서 겹쳐진 부분이 빠져서 보인다.
  - Merge -> Merge -> Out 과 같음
  - 매트 결합 시 유용함

* overlay : multiply if B<0.5, screen if B>0.5
  - A는 B를 밝게 하는데, B의 형태로 겹쳐진 부분과 함께 보인다.

* plus : A+B
  - Merge -> Merge -> Plus 와 같음
  - plus 알고리즘 사용하면 픽셀 값이 1.0보다 높을 수 있다.
  - 레이저 빔 합성에 유용, 매트 결합에는 사용X

* screen : A or B ≤1? A+B-AB: max(A,B)
  - plus 와 유사함, A또는 B가 1보다 작거나 같으면 최대 예를 사용하고 플러스와 유사함
  - Merge -> Merge -> Screen 와 같음
  - 매트 결합하고 레이저 빔 추가할 때 유용

* soft-light : B(2A+(B(1-AB))) if AB<1, 2AB otherwise
  - B가 켜짐. 부드러운 빛 느낌

* under : A(1-b)+B
  - over 연산의 역순
  - B의 매트에 따라 B를 A 위에 레이어링

* xor : A(1-b)+B(1-a)
  - 겹쳐지는 부분이 빠지고, A와 B 모두 보이긴 함.

