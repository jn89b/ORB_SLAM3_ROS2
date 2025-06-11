# Testing ORB SLAM3

On one terminal:
```
ros2 run ros2_orb_slam3 mono_node_cpp   --ros-args   -p node_name_arg:=my_mono_node   -p voc_file_arg:=/develop_ws/src/ros2_orb_slam3/orb_slam3/Vocabulary/ORBvoc.txt.bin   -p settings_file_path_arg:=/develop_ws/src/ros2_orb_slam3/orb_slam3/config/Monocular/
```

On the other terminal:
```
ros2 run ros2_orb_slam3 mono_driver_node.py --ros-args -p settings_name:=EuRoC -p image_seq:=sample_euroc_MH05
```

```
ros2 run ros2_orb_slam3 mono_cam_cpp \
  --ros-args \
  -p node_name_arg:=mono_cam \
  -p voc_file_arg:="/develop_ws/src/ros2_orb_slam3/orb_slam3/Vocabulary/ORBvoc.txt.bin" \
  -p settings_file_path_arg:="/develop_ws/src/ros2_orb_slam3/orb_slam3/config/Monocular/"
```