# Gamma
매체에 나타나는 색 스펙트럼의 밝기 곡선, 의도적인 왜곡값

* 사용하는 이유 : 디바이스가 일대일로 색표현 X, 우리의 눈도 일대일로 밝기를 감지 못함

  -> 어둡게 표현하는 매체에 밝은 이미지로 저장하면 적당한 색으로 보인다 = SRGB 이미지(원래 이미지는 더 밝게 저장되어 있음)
  
  ![image](https://user-images.githubusercontent.com/90564649/135703889-be4aabcb-ea0b-4f50-9256-5219c2aaaff0.png)
  
  
  
|Where|Gamma 값|
|---|---|
|Real World| 1|
|hdr, exr(32bit)| 1|
|jpg, png(sRGB)| 0.45|
|Monitor| 2.2|
  
  
* 3D 프로그램에서의 적용 : HDR을 제외한 텍스쳐 이미지들이나 컬러피커가 스크린에서 바르게 보일수 있는 이유는 감마 값이 미리 적용된 것.
   렌더 엔진들은 linear에서 작용 -> 텍스쳐 이미지들의 gamma 수치를 없애지 않고 linear로 작용하는 엔진에서 렌더하면 감마가 섞임 
   -> 최종에서 감마 두배됨(예상이미지와 달라짐)
   
    해결방법 : 텍스쳐 이미지, 컬러피커에서 감마 수치 뺀 후, linear로 렌더, 최종 결과물에 감마 다시 대입

  
  
 # Linear Workflow
  컬러스페이스를 선형화 했다는 뜻 (3D에서 씬 안의 모든 렌더를 같은 color space 보장!)
  
  linear : 눈으로 보는 빛의 양 = 광원들의 빛의 양을 합친 값 -> 실제 세상에서는 input, output이 일정, 컴퓨터는 아님
  
       [출처] https://frauniemand.tistory.com/8
  
  * 중요한 이유 : 모니터 자체는 감마를 올리는 곡선을 만들 수 없다.(linear 공간까지 표현하면 비싸짐)
             
      -> 즉, 일반적인 파일들은 왜곡되게 저장될 수 밖에 없으며, 작업 과정 중 색이 섞이는 문제가 발생할 수 있음
              
      대부분의 프로그램에서는 자동으로 선형 변환이 되고 있는 상황이긴 함. (감마가 깨지면 망함 - 꼭 감마를 linear로)
      
      마지막에 이미지를 출력할 때, 올바른 color space 지정하기
  


 # ACES : Academy Color Encoding System 
  ACES APO : 가장 넓은 color space (Academy에서 작업단계 중 색을 더 잘 표현하기 위해 일단 크게 만들음, 상영단계까지는 아직 노력중) 
  
 영화 산업의 변화 flim -> disital, 촬영부터 제작까지 널은 색을 표현해야하는데 많은 손실이 발생하고 있어서 새로운 색 표준이 생긴 것
 
 모든사람이 언제 어디서든 똑같은 이미지를 볼 수 있도록 하기 위해 표준 제안(글로벌 트렌드, 한국도 점차 들어오는 추세)
  
  -> 다른 color space 차이가 많이 나기 시작
  
  
### color-management-fundamentals-aces-workflows-in-nuke 영상정리
  

  
# Alpha-Premult/Straight

* Premultiplication : RGBA 채널들이 미리 곱해져 있다는 것을 뜻함

|R  G  B  x  A | 컬러값|
|---|---|
|1  1  1  x  0 | 0(검은색 or 투명)|
|1  1  1  x  1 | 1(흰색, 불투명)|

* Photoshop : Alpha - Straight 방식 (원래 알파채널 X, 투명도 자동계산), Blending Mode, 지우개 사용-파괴적 제작방식 / 알파채널 사용 - 비파괴적
* Nuke : Alpha - Premult 방식 (알파 수동방식), Merge
