# 10-11 week : 3D Rendered image in 2D composition
## 1115 class (w10) - 1122 class (w11)

* 가상의 상태의 카메라로 RGB 데이터를 얻게되는 과정(x,y,z 데이터로 이루어진 다양한 utility AOV 뽑을 수 있음)

* render image = xyplane (jpg, mov, dpx ...) -> rgb data만
* 추가적 확장자 3D scan : lidar , photogrammetry -> Point Cloud

* Ray tracing : 화면 안에 있는 광선을 추적해 보다 사실적인 그림자, 빛의 표현을 함, GPU

* 대부분의 3D 작업 이후 comp 과정을 겪어야 함 (smoke, particle, Grade 작업 등)

* format 
  - 2D : JPG, PNG, DPX, MOV, MP4
  - 3D : EXR (레이어로 나눠진다)

* AOV Render Elements
 - AOV(Arbitary Output Variables)
 - Shading Information : Diffuse Color, Specular, Metalness ...
 - Light Information
 - Utilities : Z Depth, World Position
 - Cryptomatte : mask 따기 편하게 (스포이트 찍듯이)

* render 로 100%의 이미지를 뽑아내지 않는 이유 : 비싸서 (Raytracing, Shading Material, Geometries)

* nuke에서 renderimage 이용하기
 - World Normal이나 World Position 등을 이용해서 Relight 하기
 - Point Cloud 로 포지션 하기
 - 3D 오브젝트를 누크로 불러와서 씬 만들기


--
## Research : EXR format, AOV

### EXR format

* What is EXR format? 
 - INM에서 개발한 HDR 이미지 포맷. 채널당 16비트이상의 컬러, 비손실 압축기법
 - 다양한 픽셀 크기의 다중 채널을 지원, 카메라 이미지를 여러 시점으로 인코딩 가능
 - 알파채널, 카메라 앵글에 대한 정보, Z 깊이, 이미지 레이어링 등을 담아낼 수 있는 포맷임
 - HDR만큼 폭 넓게 호환되지는 않음.(포토샵에서는 읽을 수 있음)
 - 
 - 출처 : https://ko.wikipedia.org/wiki/OpenEXR, https://www.online-convert.com/file-format/exr

* EXR 포맷 장점
 - 색 표현 범위가 넓고, 휘도 범위가 뛰어나서 이미지를 고품질로 저장할 수 있음. 정밀도
 - 각 픽셀의 서로 다른 깊이에 대한 값을 저장 할 수 있음
 - Multipart : 별개의 이미지를 하나의 파일로 인코딩 할 수 있음, 개별 부분들을 뽑아낼 수 있다
 - OpenEXR 소스를 사용하면 같은 프로세스 공간에서 여러 버전의 라이브러리 이용할 때, 사용자가 구성 가능한 C++ 네임 스페이스가 보호기능을 제공한다. = 즉 보안성이 좋고, 신뢰성이 있으며 사용 용이 하다는 말
 - 출처 : https://www.openexr.com/

### AOV

* What is AOVs
 - AOVs = Arbitrary output variables : 렌더 계산 중에 셰이더 혹은 렌더상의 데이터를 출력해서 합성을 더 용이하게 도와줌
 - AOV 많을수록 메모리와 디스크 공간 차지 -> 자신에게 필요한 채널만을 렌더링
 - Beauty : 모든 AOVs들이 합쳐져서 나온 이미지 결과(안에 있을 수 있는 AOV는 정말 다양하다)

* Standrad Surface AOVs
 - 물리적 기반의 쉐이더 에서 나올 수 있는 AOVs
 - Base, Specular, Transmissio, Subsurface, Coat, Sheen, Emission
 - 더 세분화된 AOV 들도 있음(밑에 예시)
 - Coat : coat albedo, coat direct, coat indirect

* Direct and Indirect AOVs
 - 결합되면 기본적으로 diffuse AOV와 동일하긴 하지만, 조명 구성 요소로 분리된다.
 - Direct 는 광원에서 오는 직사광선
 - Indircet는 직사광선이 닿을 때 지오메트리에서 광선을 간접적으로 반사한 것
 - Ray depth parameter은 간접 광선 바운스가 발생하는 횟수를 제어한다

* 합성에서 AOV가 쓰이는 방법 예시
 - Diffuse, Specular, Emission : 색상보정, 노출 조정 등
 - Direct, Indirect : 색상보정, 노이즈 제거 등
 - albedo : Beauty 에서는 필요없지만, RAW 조명을 얻기 위해 디퓨즈 나누거나, 텍스쳐 디테일 유지하면서 조명의 노이즈 제거 하는데 사용
 
 * utility AOVs : 특정 개체를 선택하고 조작 가능
  - ID Mask : 개체 선택
  - motionVector  (Velocity): 모션 블러 값을 수정
  - Z : 디포커스 또는 가짜 안개/안개에 사용되는 깊이 채널
  - Р  &  N : 물체의 간단한 재조명용
  - albedo : 노이즈 제거 후 안전한 정보를 위해 사용가능
  - cryptomatte : 자동 재료 ID 마스크, 개체 마스크, 자산 마스크(합성 도구에 설치해야 함)

 * Arnold AOVs
  - A: 알파
  - AA_inv_density: 샘플링으로 샘플 밀도를 시각화, 히트맵 필터 같이 사용
  - ID: 개체에 대한 사용자 옵션 문자열 필드를 통해 특정 ID 번호를 추가할 수도 있음
  - N: 셰이딩 포인트의 부드러운 법선(표준 공간)
  - P: 셰이딩 포인트의 위치(월드 공간에서)
  - Pref:   음영 포인트의 참조 위치
  - RGBA:  전체 렌더링된 이미지를 포함하는 Beauty AOV
  - Z: 카메라에서 본 음영 지점의 깊이
  - albedo: 반사율, 조명이나 그림자가 없는 표면 또는 볼륨 색상
  - background: 카메라에 보이는 배경 및 스카이돔 조명에서 방출
  - coat : 코트 반사
  - coat_albedo: 조명이나 그림자가 없는 코트 색상
  - coat_direct: 코팅 직접 조명
  - coat_indirect: 코팅 간접 조명
  - cputime: 이 레이어에는 픽셀의 샘플을 평가하기 위한 CPU 시간이 포함
  - diffuse : 확산 반사
  - diffuse_albedo:  조명이나 그림자 없이 색상을 확산
  - diffuse_direct: 직접 조명을 확산
  - diffuse_indirect: 간접광을 확산
  - direct : D의 모든 표면과 볼륨에서 irect 조명
  - Emission : 카메라에서 직접 볼 수 있는 조명 및 방출 물체
  - indirect : 모든 표면과 볼륨의 간접 조명
  - motion vector : 주어진 시간 간격 동안 음영 포인트의 화면 공간에서 모션을 나타내는 2D 벡터, RGB 형식으로 출력하는 경우 벡터는 R, G 채널에 포함, 카메라의 순간 셔터를 설정해야 함 -> 렌더링에선 모션 블러를 원하지 않지만 모션 벡터 AOV에서 모션 속도 정보를 원하기 때문/ 모션 블러-> 순간 셔터 에서 찾기
  - oppacity :  전체 3채널 불투명도가 있는 RGB AOV(단일 채널 알파와 반대)
  - raycount:  픽셀의 샘플에 대해 추적된 총 광선 수
  - shadow_matte :  장면의 그림자로, 가려지지 않은 직접 조명에 대한 가려진 직접 조명의 비율로 계산
  - sheen : 광택
  - sheen_albedo : 조명이나 그림자가 없는 광택 색상
  - sheen_direct : 광택 직접 조명
  - sheen_indirect : 광택 간접 조명
  - specular : 정반사
  - specular_albedo : 조명이나 그림자가 없는 반사 색상
  - specular_direct : 직접 조명을 확산
  - specular_indirect : 간접 조명을 확산
  - sss: 표면 아래 산란 및 확산 투과
  - sss_albedo : 조명이나 그림자가 없는 SSS 및 확산 투과 색상
  - sss_direct : SSS 및 확산 투과 직접 조명
  - sss_indirect : SSS 및 확산 투과 간접 조명
  - transmission : 반사 투과(굴절)
  - transmission_albedo : 조명이나 그림자가 없는 반사광 투과 색상
  - transmission_direct : 반사광 투과 직접 조명
  - transmission_indirect : 간접 조명의 반사광 투과
  - volume : 부피 산란
  - volume_z:  첫 번째 볼륨 기여에 대한 Z 깊이는 평면 AOV로 출력
  - volume_albedo : 조명이나 그림자가 없는 볼륨 색상
  - volume_direct : 볼륨 스캐터 다이렉트 라이팅
  - volume_indirect : 볼륨 산란 간접 조명
  - volume_oppacity :  볼륨에 대해서만 전체 3채널 불투명도가 있는 RGB AOV



--
## Research : Ratracing, Scanline render difference
