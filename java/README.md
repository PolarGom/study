#### 톰캣에서 사용중인 Heap 사이즈 확인
- ps -au|grep tomcat 명령어로 톰캣의 정보를 확인한다.
![ps 이미지](./images/ps-tomcat.png)
- jmap -heap [PID] 를 입력하여 JAVA의 힙 메모리 설정을 확인한다.
  (예. tomcat의 PID가 23631 경우 명령어는 jmap -heap 23631)
- MaxHeapSize 를 확인한다. 현재 최대로 설정되어 있는 힙사이즈는 250MB 이다.
![ps 이미지](./images/jmap.png)

#### JAVA Heap 사이즈 초기 설정 규칙

```
  Heap sizes 규칙
  Initial heap size of 1/64 of physical memory up to 1Gbyte
  Maximum heap size of 1/4 of physical memory up to 1Gbyte
  ```

&nbsp;&nbsp;나의 가상머신은 1GB 이므로 최소 Heap 사이즈는 16MB 이고 최대 Heap 사이즈는 250MB 이다.


