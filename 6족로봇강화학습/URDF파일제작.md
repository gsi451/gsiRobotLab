# URDF 파일제작
- 6족 로봇의 URDF 파일은 XML 형식의 로봇 정의 파일로, 로봇의 각 관절, 링크, 모터 등을 정의할 수 있어. 여기서 기본적인 6족 로봇(3개의 관절을 가진 6개의 다리) URDF 파일을 간단히 작성하는 방법을 알려줄게.

# 1. 기본 구조 이해하기
- 링크: 로봇의 각 부위 (몸통, 다리, 발 등)
- 조인트: 각 링크를 연결하는 관절
- URDF 파일 기본 구조:

```
<robot name="six_legged_robot">
    <!-- Base Link (몸통) -->
    <link name="base_link">
        <visual>
            <geometry>
                <box size="0.5 0.5 0.1"/>
            </geometry>
            <material name="blue"/>
        </visual>
    </link>

    <!-- 각 다리 정의 -->
    <!-- 예제: 첫 번째 다리 -->
    <link name="leg1_link1"/>
    <joint name="leg1_joint1" type="revolute">
        <parent link="base_link"/>
        <child link="leg1_link1"/>
        <axis xyz="0 0 1"/>
        <limit lower="-1.57" upper="1.57"/>
    </joint>
</robot>
```

# 2. 6족 로봇 전체 URDF 예제
```
<?xml version="1.0"?>
<robot name="six_legged_robot">
    
    <!-- 로봇 몸체 -->
    <link name="base_link">
        <visual>
            <geometry>
                <box size="0.5 0.5 0.1"/>
            </geometry>
            <material name="blue"/>
        </visual>
    </link>

    <!-- 6개의 다리 (각각 3개의 관절) -->
    <!-- 다리 1 -->
    <link name="leg1_link1"/>
    <joint name="leg1_joint1" type="revolute">
        <parent link="base_link"/>
        <child link="leg1_link1"/>
        <origin xyz="0.2 0.3 0"/>
        <axis xyz="0 0 1"/>
        <limit lower="-1.57" upper="1.57"/>
    </joint>

    <link name="leg1_link2"/>
    <joint name="leg1_joint2" type="revolute">
        <parent link="leg1_link1"/>
        <child link="leg1_link2"/>
        <origin xyz="0 0 -0.1"/>
        <axis xyz="0 1 0"/>
        <limit lower="-1.57" upper="1.57"/>
    </joint>

    <link name="leg1_link3"/>
    <joint name="leg1_joint3" type="revolute">
        <parent link="leg1_link2"/>
        <child link="leg1_link3"/>
        <origin xyz="0 0 -0.1"/>
        <axis xyz="0 1 0"/>
        <limit lower="-1.57" upper="1.57"/>
    </joint>

    <!-- 여기에 나머지 5개의 다리도 동일하게 정의 -->
    <!-- 다리 2 -->
    <link name="leg2_link1"/>
    <joint name="leg2_joint1" type="revolute">
        <parent link="base_link"/>
        <child link="leg2_link1"/>
        <origin xyz="-0.2 0.3 0"/>
        <axis xyz="0 0 1"/>
        <limit lower="-1.57" upper="1.57"/>
    </joint>
    
    <link name="leg2_link2"/>
    <joint name="leg2_joint2" type="revolute">
        <parent link="leg2_link1"/>
        <child link="leg2_link2"/>
        <origin xyz="0 0 -0.1"/>
        <axis xyz="0 1 0"/>
        <limit lower="-1.57" upper="1.57"/>
    </joint>

    <link name="leg2_link3"/>
    <joint name="leg2_joint3" type="revolute">
        <parent link="leg2_link2"/>
        <child link="leg2_link3"/>
        <origin xyz="0 0 -0.1"/>
        <axis xyz="0 1 0"/>
        <limit lower="-1.57" upper="1.57"/>
    </joint>

    <!-- 나머지 4개 다리도 동일하게 추가 -->

</robot>
```

# 3. URDF 파일 설명
- <robot> 태그: 로봇의 이름을 지정.
- <link> 태그: 로봇의 각 부위를 정의 (몸통, 다리, 발).
- <joint> 태그: 각 링크를 연결하고, 관절의 움직임을 설정 (회전축, 회전 범위).
- <origin> 태그: 각 관절의 위치를 지정.
- <axis> 태그: 관절이 회전할 축을 지정.
- <limit> 태그: 관절의 회전 범위를 설정.


# 4. 이 파일을 저장하는 방법
- 파일명: six_legged_robot.urdf
- 파일 위치: 프로젝트 폴더 (/six_legged_robot_project/)


# 5. PyBullet에서 로봇 불러오기
- 이 URDF 파일을 PyBullet에서 직접 로드할 수 있어:

```
import pybullet as p
import pybullet_data
import time

p.connect(p.GUI)
p.setAdditionalSearchPath(pybullet_data.getDataPath())
p.loadURDF("six_legged_robot.urdf", [0, 0, 0.5])

while True:
    p.stepSimulation()
    time.sleep(1.0 / 240.0)
```


# 6. 다음에 할 일
- 6개의 다리를 완성하고 관절 위치를 미세 조정
- 각 다리의 기본 위치 설정 (초기 자세)
- 로봇의 무게와 관절 모터 추가
- 강화학습 환경 구축 (로봇 이동 보상 설정)

