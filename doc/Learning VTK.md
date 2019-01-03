# Learning VTK 

## 목표 

**매시와 애니메이션:** 

- FBX 임포트 
- 매시와 애님 렌더링 
- 머티리얼 지정 
- 머티리얼 애님

이미 기능이 충분할 수 있다. 연습 문제로 푼다. 



**공간처리:** 

- Voxel 화 
- 충돌 처리 
- 이동 처리 

먼저 Sparse Voxel Tree를 VTK에 구현한다. 메모리 사용이 많으면 줄일 수 있는 방법들을 찾는다. 표면을 사용해서 해결한다. 표면 중 하나가 네비게이션 매시이다. 




## Installation 

cmake-gui 

- BUILD_SHARED_LIBS 
  - DLL 빌드
- pdb 포함하여 디버깅 가능하도록 함 

gui로 잘 안 돼서 cmake ..으로 실행. 솔루션 파일들 만들어짐. 

VTK_DIR을 Build 폴더로 하면 VTKConfig.cmake 파일을 참조하여 튜토리얼 솔루션을 생성할 수 있다. 



## Getting Started

간단한 예제로 사용법 익히기. 매시 렌더링, 쉐이더 지원 확인. 

vtkObject, vtkObjectBase, ::New(), Delete() 
vtkSmartPointer<T>  obj = vtkSmartPointer<vtkExampleClass>::New(); 


vtkProp
vtkActor, vtkVolume, vtkActor2D

vtkProperty 
vtkRenderer 
vtkCamera
vtkLight 
vtkRenderWindow
vtkTransform 
vtkLookupTable, vtkColorTransferFunction, vtkPieceWiseFunction 

```c++
vtkCylinderSource *cylinder = vtkCylinderSource::New();
vtkPolyDataMapper *cylinderMapper = vtkPolyDataMapper::New();
cylinderMapper->SetInputConnection(cylinder->GetOutputPort());
vtkActor *cylinderActor = vtkActor::New();
cylinderActor->SetMapper(cylinderMapper);
vtkRenderer *ren1 = vtkRenderer::New();
ren1->AddActor(cylinderActor);
vtkRenderWindow *renWin = vtkRenderWindow::New();
renWin->AddRenderer(ren1);
vtkRenderWindowInteractor *iren = vtkRenderWindowInteractor::New();
iren->SetRenderWindow(renWin);
renWin->Render();
iren->Start();
```

위의 흐름이 실린더를 화면에 그리는 코드이다.  카메라 지정은 일단 없다. 



Data Source -> Filter -> Mapper -> Actor 

위의 흐름으로 데이터가 입력된다. Filter가 렌더링과 달리 데이터 처리에 많이 사용된다. 



##  튜토리얼



### 디버깅 정보 생성 

- VTK의 pdb가 생성되지 않음 
  - cmake의 pdb 생성 옵션 찾기 
  - 디버깅이 자유로워야 내부를 파악하기 쉽다 



RelWithDebInfo 로 VTK 빌드. 패스를 이쪽으로 잡고 튜토리얼도 같은 구성으로 빌드.  



## 매시와 애니메이션

자체 매시 그리기 확인. Voxel(Volume) 렌더링 확인. 관련 알고리즘 확인. 

