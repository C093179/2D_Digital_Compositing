# 1101 class - maya 속 3D 환경

* 2D 노드는 사각형 / 3D 노드는 둥근 모양
* scene node - 각각 하나의 3D 파일을 말함 / scene에 card node(2D레이어), 3D 모델링 등 연결 가능(ReadGeo node)
* camera node - 3D 카메라
* Scanline Rander node - 카메라 렌더 프로그램(2D화), translate 로 공간감 위치 조정 가능, obj/scn : scene과 연결. cam : camera와 연결

--
## CameraTracker node - Plate 기반으로 3D 카메라의 움직임 추출, 시퀀스나 스틸 모드 연결

* setting tab -> number of feature 조정, preview feature체크
* mask 로 카메라 움직임에 방해하는 것들 가려주기
* Camera 설정 : Camera motion, Distoration, Focall Length - 모르면 unknown 체크, film back size
* Analysis -> track 누르기 / solve는 tracking marker의 쓸모 판단
* AutoTracks tab -> tracking marker 지워주기 (Mas Track Error로 조정)
* Camere node Create -> 카메라 추출
* Camera scene Create -> feature 들을 기반으로 씬 구성
* Scan line Rander node에 plate를 bg 로 연결해서 트래킹이 제대로 되었는지 확인 가능

--
## Disital Matte Painting
* psd 노드 파일 불러오기
* 각각의 레이어에 card node 붙이기 - card의 z 값 조절해서 depth 조절
* editd geo node - 좀 더 공간감 줄 수 있음
