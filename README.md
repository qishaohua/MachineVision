# ORBSLAM2_MYNT工程架构 
## my_rgbd.cc
1. 图漾相机初始化……ln74  int init_flag = ty_RGBD_init();  
2. SLAM系统初始化……ln83  ORB_SLAM2::System SLAM(argv[1],argv[2],ORB_SLAM2::System::RGBD,true);  
3. 获取相机参数……ln96-119  
4. 帧获取……ln129-138  int err = TYFetchFrame(hDevice, &frame, -1);  
5. 帧处理……ln137 frameHandler(&frame, &cb_data);  
6. SLAM处理图像……ln152 SLAM.TrackRGBD(color_img, depth_img, slam_sys_time);  
7. 关闭slam系统……ln170 SLAM.Shutdown();  
8. 保存相机轨迹……ln188 SLAM.SaveTrajectoryTUM("my_ty_rgbd_CameraTrajectory.txt");  

