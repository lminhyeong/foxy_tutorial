# action 생성하기
1. 소개
2. 실습

## 1. 소개
* 기존에 배운 topics과 services와 같이 action 을 정의하고 빌드하는 방법 배워보기
## 2. 실습
* action_tutorials_interfaces package 생성하기
```bash
mkdir -p ros2_ws/src
cd ros2_ws/src
ros2 pkg create action_tutorials_interfaces
```

### 2-1 action 정의하기
* .action 파일 정의
```
# Request
---
# Result
---
# Feedback
```
  * request : client가 server로 새로운 goal을 전송 
  * result : server가 client에게 goal이 완료되면 전송
  * feedback : server가 client에게 주기적으로 업데이트된 내용을 전송
----
* Fibonacci 수열을 계산하는 새로운 action을 정의해보자.

* action 디렉토리 생성하는 명령
```bash
cd action_tutorials_interfaces
mkdir action
```

* Fibonacci.action 생성
```
int32 order
---
int32[] sequence
---
int32[] partial_sequence
```
  * request : order
  * result : sequence
  * feedback : partial_sequence

### 2-2 action 빌드하기
* CMakeLists.txt 파일 수정 (ament_package() 이전에 아래 코드 추가)
```cmake
find_package(rosidl_default_generators REQUIRED)

rosidl_generate_interfaces(${PROJECT_NAME}
  "action/Fibonacci.action"
)
```

* package.xml 파일 수정(의존성 추가)
```xml
<buildtool_depend>rosidl_default_generators</buildtool_depend>

<depend>action_msgs</depend>

<member_of_group>rosidl_interface_packages</member_of_group>
```

* package 빌드하기
```bash
cd ~/ros2_ws
colcon build
```

* action type
  * [package_name]/action/[action_name]
* action 확인
```bash
. install/setup.bash
ros2 interface show action_tutorials_interfaces/action/Fibonacci
```
