# Building Cinder



## Warnings

- BOOST_CONFIG_SUPPRESS_OUTDATED_MESSAGE; 
- _SILENCE_TR2_SYS_NAMESPACE_DEPRECATION_WARNING;

다른 변환 경고가 많이 나온다. 무시하면 좋지 않은 오류들인데 좀 봐야겠다. 



## VS 2017 빌드 

vcxproj  에서 불필요한 구성들 제거하고 빌드 진행. 경고가 아주 많이 나오긴 하는데 전에도 VS 2017로 빌드 한적이 있다. (플래폼 도구 v141) 

https://docs.microsoft.com/ko-kr/cpp/porting/binary-compat-2015-2017?view=vs-2017



- 경로 설정 시 x64 인지 잘 확인 
- $(CINDER_HOME) 환경 구성해서 정리 
  - 있으면 훨씬 편함 
- __iob_func 정의 안 된 오류 
  - 이건 이전 버전 라이브러리 빌드라 그렇다 함 
  - 함수를 정의해서 해결 
  - 스택오버플로에서 찾음 













