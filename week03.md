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
  


 ### ACES : Academy Color Encoding System 
  ACES APO : 가장 넓은 color space (Academy에서 작업단계 중 색을 더 잘 표현하기 위해 일단 크게 만들음, 상영단계까지는 아직 노력중) 
  
 * 영화 산업의 변화 flim -> disital, 촬영부터 제작까지 넓은 색을 표현해야하는데 손실이 발생하고 있어서 새로운 색 표준이 생긴 것
 
 * 모든사람이 언제 어디서든 똑같은 이미지를 볼 수 있도록 하기 위해 표준 제안(글로벌 트렌드, 한국도 점차 들어오는 추세)
  
  -> 다른 color space 차이가 많이 나기 시작
  
  
### color-management-fundamentals-aces-workflows-in-nuke 영상정리
        https://www.youtube.com/watch?v=Hlj5ep-85ys   ★다시 볼 것, 넘 어렵당

* Color Management : 색 공간 변환을 통제 -> 모든 부서에서 공통된 색으로 작업할 수 있도록 일관성 있게 제공하는 프레임워크 
(모두가 같은 이미지를 사용해서 의도가 변하지 않도록)
  - 옛날에는 동일한 rgb 방식을 사용(필름~상영), 현재는 다양한 color space가 생김(디지털의 여러 버젼들)
  - color space 마다 빛을 표현하는 방식이 달라서 다양할 수 밖에 없음
  - 디지털 색공간은 원기둥에 극좌표를 구성하고, 가장 잘 알려진게 가산혼합 방식인 RGB모델
  - 색상의 시각화 - 1차원 : 가시 스펙트럼 (line)/ 컬러휠(각도, chromaticity 색도-색조와 채도만 )
  
  (x,y공간) : Tuple , x - 인간이 감지하는 색상 도표의 한 지점, y - 휘도(luminance -Y) 
  -> 모든 파장 혼합해서 평균인간시각과 휘도 사용해서 모든 가시 색상을 지정할 수 있음 = Spectral Locus = CIE xy Chromaticity Diagram = 인간이 감지하는 색 영역 = 참고용 color space
 
  
       - 순도(Purity) : 순수한 단일 파장이 지배적이라는 것을 의미, 마젠타 방향으로는 없음
         
         각 Primaries(원색)의 위치는 순도로 결정, 이걸로도 색역 지정가능하고 화이트 포인드 지정가능 = sRGB
         
       - 스펙트럼 궤적 내의 색은 모두 인간이 볼 수 있는 색, 외부는 볼 수 없는 색(수학적으론 존재)
       - Gamut : 정의된 부분이 의미하는 전체 범위 = color (임의로 설정한 절대값을 사용) / 1차원에서는 선으로, 2차원에서는 삼각형으로 표현됨
       - Color Gamut : 특정장치가 포착하거나 재생하는 색상의 양을 색도도에서 색역으로 나타냄, 4번째 점인 화이트 포인트 반드시 필요(안정감 위해)
       
         기준 색역(relative gamut)은 인간의 시각에 비해 상대적인 것이 되는 것임
         
         -> 색상의 강도를 나타내고(chromaticity), 색상이 재현하는 범위의 한계를 설정함 / 입력과 출력 장치의 색을 지정해줌
         
       - 화이트 포인트 : 최저 채도점/ 흑체 궤적 포물선에서 움직이며 색온도를 보존하고 마젠타 성분에 영향을 주고, 인간은 색온도 변화에 익숙함(화이트 밸런스 덕분에)
         
         CIE Standard Illuminant D65(표준광, daylight, sRGB의 화이트포인트)

* 그동안 nuke color management가 sRGB 색공간을 자동적으로 알아서 잘 해주다가 상황이 변함 
 - Transfer Function : 구체적인 전달함수 생성해서 영상 기록 성능과 카메라 전체 색상재현능력이 최적화해서 정확한 색공간 위치에 배치
 - 삼원색 f(x) = Characteristic Curve
 - HDR 디스플레이는 SDR보다 더 밝은 백색을 구현할 수 있어서 더 특별한 전달함수가 필요 (HLG, PQ/ HDR 광도에 따라 신호값을 배분)
 
 Look Up Table(LUT) : 대응점으로 구성된 데이터의 단순한 배열(사전 느낌, 입력 색상값을 출력 색상값으로 변환=리매핑 작업)
 
 LUT에서 output=input이면 linear임 / 0,0 output<input 1,1 데이터양이 충분하지 않으면 로그함수(곡선)
 
 전달 함수 = 특정 유형의 LUT
 
 * 디지털 시네마 -> 디지털 카메라 발명 이후로는 모든 작업이 디지털로 변하기 시작 -> 티비도 변화(HDTV 표준 : Rec.709 탄생) -> HDR 생김
 * LUT 사용해서 sRGB 보다 좋은 컬러스페이스를 사용하는 매체에 전달하도록 해야함
 * Display Referred : 디스플레이에 따른 색상변형 / LUT 이용해서 한 색상을 변환해서 다른 색공간으로 바꿔야 함(각 디스플레이마다, 일관성 해침)
 * Scene Referred : 색상변형을 할 때 영상을 참조-> 모든 디스플레이에 적용 가능 / 모든 영상 색변환을 마스터 colorspace로 바꿔서 거기서 영상작업 후 단순한 진행가능
 * 처음에 Master color space 고르기(주입할 파일의 모든 작업 범위를 포함하는 넓은 영역) -> 모든 영상이 통합 color space로(아티스트가 뭐든 적용할 수 있도록)
 -> VFX -> DI(디지털 중간 작업, 최종 색보정) -> 최종 디스플레이에 맞춰서 알맞은 color space보내야함 (원본도 저장하기) :  색공간 변환은 총 2번만
 
 * RGB Color Space 핵심 요소들
  - 색공간의 색역(Gamut)을 지정하는 삼원색(3 Primaries)
  - 색역 중심 정하고 색도(Chromatic)가 가장 낮은 지점인 White Point(최저 채도, 백색)
  - 선형 3색 자극값과 비선형 전자 신호값 사이를 매핑하도록 지정하고, colorspace 내에서 색 표본 데이터를 분배하는 Transfer Function
 
 

### 헷갈리는 한글 - 영어 용어 정리
* 색의 3속성 : 색상(Hue), 명도(Value Brightness Lightness), 채도(Chroma Saturation Colorfulness)
* 빛의 밝기 
  - 광속(Luminous Flux) : 광원에서 방출되는 빛의 총량 - lm(루멘)
  - 조도(Illumination) : 빛 밝기의 정도, 대상면에 입사하는 빛의 양, 단위 면적에 도달하는 빛의 양 단위 - lux(럭스)
  - 광도(Luminous intensity) : 광원에서 특정한 방향으로 나오는 빛의 세기 - cd(칸델라)
  - 휘도(Luminance) : 눈부심의 정도, 단위면적에서 반사되는 빛의 양 - nt, lm(니트, 루멘)
      
         출처 :  https://kkhipp.tistory.com/32
        
        
 
# Alpha-Premult/Straight

* Premultiplication : RGBA 채널들이 미리 곱해져 있다는 것을 뜻함

|R  G  B  x  A | 컬러값|
|---|---|
|1  1  1  x  0 | 0(검은색 or 투명)|
|1  1  1  x  1 | 1(흰색, 불투명)|

* Photoshop : Alpha - Straight 방식 (원래 알파채널 X, 투명도 자동계산), Blending Mode, 지우개 사용-파괴적 제작방식 / 알파채널 사용 - 비파괴적
* Nuke : Alpha - Premult 방식 (알파 수동방식), Merge
