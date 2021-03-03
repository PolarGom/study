#### Pipeline
  - 파이프라인을 구성하여 젠킨스의 워크플로우를 구성할 수 있다.
  - Pipeline을 구성하는 문법에는 scripted 와 Declarative 문법 2가지가 존재한다.
    - [scripted 공식 문서](https://www.jenkins.io/doc/book/pipeline/syntax/#scripted-pipeline)
    - [Declarative 공식 문서](https://www.jenkins.io/doc/book/pipeline/syntax/#declarative-pipeline)
  - 기본적인 Pipeline script 예시
  
  ```
    pipeline {
      agent any

      stages {
          stage('Build Ready') {
            steps {
                echo 'Build Ready'
            }
          }
          stage('Test') {
              steps {
                  echo 'Testing...'
              }
          }
          stage('Build') {
              steps {
                  echo 'Building...'
              }
          }
          stage('Complete') {
              steps {
                  echo 'Complete!'
              }
          }
      }
    }
  ```

  - Pipelin 빌드 후 젠킨스 화면
  ![ps 이미지](./images/1.png)