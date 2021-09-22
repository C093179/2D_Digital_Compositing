# 2D
2D - x,y 함수  / 3D - x,y,z (depth)

* 표현방식 = 2D, 3D 

* 최종작품을 완성하는 매체 = **only 2D**(아직 기술의 발달이 덜 됨)

     --> 2D는 단순한 이미지의 합성 차원이 아닌 결과물을 표현하기 위한 가장 기본적인 개념(3D 분야와 밀접한 관련)
     
     맵핑, 합성등을 할 때에도 2D의 방식이 필요하기도 함
          
          
          
# Disital
disit = 숫자  / al = ~화 된  --> 숫자화 된

0 = black   /  1 = white    --> 컴퓨터는 숫자를 색상으로 치환하여 보여줌

 -> 컴퓨터는 픽셀(숫자, 디지털 정보)로 사진을 보기 때문에 고양이 사진을 바로 고양이로 인지 못함
 
 
 

# Compositing

* composition : 하나의 이미지, 사진을 정렬하는 모든 행위
* compositing : 2개 이상의 이미지를 겹쳐 만들어냄 (post-production에서 주로 작업)

  ![image](https://user-images.githubusercontent.com/90564649/134373533-070a83e7-4b17-409a-8b07-b7a513f76a19.png)
  
  자료출처 : https://subscription.packtpub.com/book/hardware_and_creative/9781782161127/1/ch01lvl1sec09/understanding-cg-compositing

### 합성의 장점
렌더링 된 CG footage에 추가적인 디테일 작업이 가능 (더 사실적으로 보이거나.. 더 이쁜 작업물 만들기)

### 컴포지팅 종류
* ROTOSCOPING : 이미지를 2가지 요소로 분리해냄(제거, 마스킹 등 특정 개체를 수동으로 잘라내는 것도 포함), 프레임별로 footage를 추적할수도 있음
* TRACKING(2D/3D) : 2D는 x.y 안에 특징적인 부분을 잡아서 트래커가 따라가도록 함 

   3D는 카메라의 움직임을 재현(3D 데이터를 기록할 수 있는 카메라는 없기 때문)
* A OVER B : 가장 기본, A 위에 B 삽입
* MATTING : 풍경나타낼 때 사용
* GREEN SCREEN : 크로마키 할 때 사용, 배경 쉽게 뺄 수 있도록
* FRONT OR REAR PROJECTION : 다양한 장면이나 배경을 레이어링하는 것()
* CGI : 컴퓨터를 사용하여 만든 이미지(특수효과, fx 등)를 레이어링
* MULTIPLE EXPOSURE : 여러 이미지 겹쳐서

출처 : https://beverlyboy.com/filmmaking/what-are-the-different-types-of-compositing-techniques-in-film/
 
 
 
 
# Color
* 색조, 채도, 명도
  - 색조(Hue) : 색상환 내에서의 위치
  - 명도(Value) : 밝고 어두움
  - 채도(Saturation) : 색의 강렬한 정도 (채도가 높을수록 선명함)
* 색 = 빛
  - 색은 다양한 파장의 에너지로 이루어져 있고, 그 에너지가 안구 속 신경 신호로 변환되어 뇌에서 처리 -> 뇌에서만 존재하는 '색'이라는 개념
* RGB 와 CMYK
  - RGB : 빛의 가산혼합, 모니터나 스크린 상영등에서 이용 (Red, Green, Blue)
  - CMYK : 감산혼합, 인쇄매체 (Cyan, Magenta, Yellow, blacK)
  - RGB와 CMYK는 섞인 부분이 서로 반대이다
* 나중에 읽어보고 정리해볼 자료 : https://chrisbrejon.com/cg-cinematography/chapter-2-color-theory/

### 디지털 속의 색 (이진법으로 색 표현)
* Bit depth : 1bit(0,1) / 2bit(00,01,10,11) / 3bit(000,111...=2의 3승) / 8bit(2의 8승)
* bit값이 클수록 다이나믹레인지가 넓어짐
* 대표적 8 bit 포맷 : JPG(그래서 이미지가 압축되는 것..)
* 10~16 bit : RAW 포맷은 확장자가 다 다름, CCD 단자가 표현할 수 있는 최대한의 픽셀값을 저장할 수 있음(같은 해상도일때 JPG보다 훨 좋음)
* 저가형 모니터의 경우 6bit 표현, bit 값 클수록 모니터 비싸짐, 포토샵의 경우 8 bit(2D에 맞춰진 툴)
* bit depth는 채널 하나를 말한다. (RGB는 채널 하나가 총 3개 있는 것)


# Alpha
