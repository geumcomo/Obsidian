---
---

```
PS C:\My_Study\My_Study\Docker_Study\work\doc_boot1> ./gradlew clean build -x test
```

## 왜 gradlew이냐

gradlew은 로컬 시스템에 Gradle이 없을 때  
프로젝트를 빌드하는 데 사용되며,  
gradle은 로컬 시스템에 이미 설치된 Gradle을 실행할 때 사용됩니다.  
따라서 프로젝트를 공유하거나 다른 컴퓨터에서 빌드할 때  
일관된 결과를 얻으려면 **gradlew을 사용**하는 것이 좋습니다.
