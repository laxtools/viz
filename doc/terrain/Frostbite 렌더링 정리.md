# Frostbite 렌더링 정리 

Battlefield : Bad Company에 사용된 기법들을 정리한다. 이전의 방법은 컬러맵과 디테일 마스크를 사용하는 방법이다. 



## Terrain Texturing and Shading 



### 그래프 기반의 서피스 쉐이더 

아래와 같이 쉐이더 파라미터를 연결하여 쉐이더를 생성하는 머티리얼 지정 기능이 있다. 

![fbtr_figure5](img\fbtr_figure5.png)



### Procedural parameters

- Height 
- Slope 
- Normal 

위 세가지를 쉐이더 인스턴스에 제공하는 것으로 시작. 

normal cross filter란 기법: 

```
Given a function f : ℝ2 → ℝ that describes a surface in ℝ3, a unit normal at (x,y) is given by

v = (−∂f/∂x, −∂f/∂y, 1) and n = v/|v|.

It can be proven that the best approximation to ∂f/∂x by two samples is archived by:

∂f/∂x(x,y) = (f(x+ε,y) − f(x−ε,y))/(2ε)

To get a better approximation you need to use at least four points, thus adding a third point (i.e. (x,y)) doesn't improve the result.

Your hightmap is a sampling of some function f on a regular grid. Taking ε=1 you get:

2v = (f(x−1,y) − f(x+1,y), f(x,y−1) − f(x,y+1), 2)
```

간결하면서 정확하게 설명한다. 실제 위 코드를 사용한다. 

## Masking



### 절차적인 마스크 

쉐이더에서 기울기 등을 보고 쉐이더를 선택한다. 아이디어는 그렇다. 



### Static Sparse Mask Textures

Sparse quad-tree texture를 사용하는 32x32 픽셀 타일을 통해 쉐이더 지정. 



### Destruction Mask 

파괴 효과 등을 표현하기 위한 재질용 마스크. 

![fbtr_figure12](D:\laxtools\viz\doc\terrain\img\fbtr_figure12.png)

메모리 최적화를 위해 render to texture를 사용하여 필요한 부분에 대한 정보만 전달한다. 

## Terrain Shader Compositing 

터레인 재질 쉐이더를 하나로 합쳐서 실행한다. 전의 결과가 다음 쉐이더로 전달된다. 필요없으면 쉐이더를 건너뛰기도 한다. 

이를 procedural shader splatting이라고 부른다. 



## Terrain Rendering 

쿼드 트리를 사용한다. 하나의 쿼드 트리 노드는 33x33 버텍스 그리드를 갖는다. 항상 고정된 크기를 갖는다. 



### Geometry LOD 

T junction 제거를 위한 방법에 대한 정리. 



## Undergrowth (덤불)

절차적 방법을 사용한다. 뮤는 쿼터뷰 게임이라 적합한 방법이 아니다. 바닥면이 게임의 인상에 매우 큰 영향을 미친다. 

### Method 

여기도 결국 텍스처에 정보를 담는다. 페인팅할 때 정보를 주고 텍스처를 참고해서 그린다. 



## 정리 

배틀필드의 개발사인 DICE는 Frostbite 엔진으로 뛰어난 기술력을 항상 보여주고 있다. 이와 같이 하나의 코드 베이스에서 파생되고 지속적으로 발전하는 시스템을 갖추고 있어야 한다. 어떤 때는 처음부터 구현할 수도 있겠지만 항상 계속 개선하는 기반이 있어야 한다. 

나는 언리얼과 WISE를 그 기반으로 한다. 



































