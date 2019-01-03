# Cinder

게임 엔진으로 사용하기에 적합하지는 않지만 프로토타잎과 여러 실험을 해보기에 유연하고 괜찮다. 애니메이션이 없어 관련 기능이 동작하도록 한다. 



- 다운로드 
- boost 경로 지정 
- 빌드 



샘플들 

- boost 경로 지정 
- cinder include 경로 지정 
- 구성별 lib 경로 지정 



## Geometry

- shader
- TriMesh 



## TriMesh

```c++
TriMesh::Format fmt; 
TriMesh  mesh(source, fmt);
gl::Batch::create(); 
```

Geomio::Source의 하위 클래스들 

- Teapot
- Sphere
- ... 

```c++
TriMesh mesh(); 

TriMesh::loadFromSource(); 

TriMeshGeomTarget target(this); 
source.loadInto( &target, attribs ); 

// Attribute는 POSITION, NORMAL, TANGENT, BITANGENT, COLOR, TEX_COORD_0 ~ TEX_COORD_3 
// 위 정도면 대부분의 TriMesh는 그릴 수 있다. 

Teapot::calculate(); 

Teapot::computeBasisFunctions(); 
// 표면의 미분 계수 등 계산 

Teapot::buildPatch(); 
// 버텍스와 노멀 등 계산 

// TriMesh로 다시 복사됨 
// 이런 inderction이 필요했던 것 같은데 최적화 가능하다. 
   
```



## App::draw()

```c++
mPrimitive->draw(); 
// mPrimitive는 gl::BatchRef 

gl::Batch::create( mesh, mPhongShader ); 
// mPhongShader는 gl::GlslProgRef 
```

```glsl
uniform mat4	ciModelViewProjection;
uniform mat4	ciModelView;
uniform mat3	ciNormalMatrix;
```

위 행렬과 ciPosition 등이 전달되는 흐름이 어디서 결정되는 지 확인이 필요하다. 

`Context::setDefaultShaderVars()`

여기서 UNIFORM_MODEL_MATRIX 등 기본 uniform 변수를 지정한다. 

VboMesh::drawImpl()에서 glDrawEleemnts() 또는 glDrawArrays()로 그린다. 



## 노멀 맵 예제

멋지다.  

- 로딩 
- 렌더링 
- 쉐이더 



### 로딩

텍스처는 별도 텍스처로 로딩한다. 

- loadAsset 
- loadImage
- gl::Texture::create() 



VboMesh는 TriMesh로 로딩한 후 생성한다. 

- TriMesh(file); 
- VboMesh::create(TriMesh)



### 렌더링

VboMesh에 대해 gl::draw()로 그림 

update()에서 쉐이더 uniform 변수들 지정.  

```c++
mShaderNormalMapping->uniform( "uLights[0].diffuse", mLightLantern.diffuse );
mShaderNormalMapping->uniform( "uLights[0].specular", mLightLantern.specular );
mShaderNormalMapping->uniform( "uLights[1].diffuse", mLightAmbient.diffuse );
mShaderNormalMapping->uniform( "uLights[1].specular", mLightAmbient.specular );
```

uLights가 배열로 정해진 문자열을 사용한다. 

나머지 파라미터는 매트릭스로 전달된다.  (확인!)



## 애셋

https://github.com/gaborpapp/Cinder-Assimp

몇 가지 사용 예를 포함한 라이브러리이다. 완전하지는 않다. 

Cinder에 Assimp를 포함해서 빌드하고 사용한다. 



## 애님

https://github.com/num3ric/Cinder-Skinning

- 괜찮긴 하나 오래 되었다. 



https://github.com/tek-nishi/SkeletalAnimation

- 기능이 적다 



둘 다 완성되지는 않았으므로 참고 자료로만 활용한다. 



# 스킨 애님  

기본 개념들: 

- joint 
- skinning
  - weight 
- bind pose



구현: 

- curve 
  - channel
  - event 
- skeleton 
  - bone / joint
  - socket
- skin mesh 
  - weight 
- shader 





