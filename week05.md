# 05
## Chroma key
Matte를 더 쉽게 추출하기 위해

https://www.youtube.com/watch?v=H8aoUXjSfsI   -  그린스크린 합성의 역사

* 디지털 시대에서의 그린스크린 : 적은 빛, 전자기기는 밝게 인식, 외부촬영 용이, 초록 의상 적음

  Bayer Pattern -> Green의 수 가장 많음 : Green으로 뺐을 때 가장 정확히 빠질 수 있게됨

* 주의사항 : 카메라 잡히는 영역에 그림자 잡히지 않도록(피사체가 플랫해지더라도)




## Keyers : 특정 채널을 이용해 alpha 를 만드는 것

각 keyer 마다 key를 추출하는 알고리즘이 달라서 각각의 shot에 맞는 keyer를 사용해야함

원본의 색 변형을 조금 일으켜도 상관X -> 결국 alpha 채널만 잘 추출해내면 됨

알파 추출 후 Copy node로 원본에다가 알파채널을 넣어줄 것 

계속 알파채널 확인해주면서 제대로 추출되고 있는지 확인할 것


### Keying (Nuke)
* Utility : 굳이 크로마 촬영 안하더라도 누크 채널들을 이용해 key를 뽑아냄

  -Keyer(alpha) : 각 채널들(루미넌스, RGB 채널 등)을 이용 , a(시작)bc(조종값)d(블랙값) 핸들을 이용하여 값 적용
  
  -Hue Keyer(alpha) : 색상 이용, 그래프 움직이면서 색상 정하기
  
  -Difference(alpha) : input A와 B의 픽셀차이를 이용, 차이가 적은 것은 알파0, 두 오브젝트 차이 비교시에도 유용하게 사용

* (B/G) Screen : 크로마 촬영 후 key를 뽑아냄

  -Keylight : 스크린 컬러 이용, ctrl 누른 상태에서 체크박스 누른 후 스포이드로 색 뽑아내기, premult 기본적용
  
  screen matte 옵션 이용 (clip white/ black 이용 -> 피사체의 디테일이 사라질 수 있음)
  
  screen gain 옵션 - 이미지의 gain을 조종하는 거나 거의 마찬가지
  
  -Ultimatte : fg 위 이미지 연결, bg 아래이미지 연결(없어도 상관x)
  
  -Primatte : fg 위 이미지 연결, bg 아래이미지 연결(없어도 상관x)
  
  operation 옵션 - 스포이드로 색 선택해서 clean BG noise 등으로 노이즈 지울 수 있음, 화살표는 다음 단계로, lingting 조명 할건지
  
  -IBKGizmoV3
  
  -ChromaKeyer
  
  
* Soft Key (외곽디테일 따는데 이용), Hard Key(내부 matte 남아있는 부분 이용) + Keymix(key 부분을 나눠서) -> 대부분 key를 따낼 수 있음

 - keyer 하나로 끝낼 생각 하지 말고, 다양한 keyer을 이용하여 제대로된 알파 추출해낼 것


* Dispill : key를 추출했을 때 크로마가 묻은 색을 억제하는 방법

