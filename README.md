# ORBSLAM2_MYNT工程架构 
## my_rgbd.cc
### main函数……ln64
1. 图漾相机初始化  int init_flag = ty_RGBD_init();  
2. SLAM系统初始化  ORB_SLAM2::System SLAM(argv[1],argv[2],ORB_SLAM2::System::RGBD,true);  
3. 获取相机参数  
4. 帧获取  int err = TYFetchFrame(hDevice, &frame, -1);  
5. 帧处理  frameHandler(&frame, &cb_data);  
6. SLAM处理图像  SLAM.TrackRGBD(color_img, depth_img, slam_sys_time);  
7. 关闭slam系统  SLAM.Shutdown();  
8. 保存相机轨迹 SLAM.SaveTrajectoryTUM("my_ty_rgbd_CameraTrajectory.txt");  

## System.cc 
### 构造函数System……ln76   在主函数的2处被调用
1. 创建字典  mpVocabulary = new ORBVocabulary();  
2. 创建关键帧数据库  mpKeyFrameDatabase = new KeyFrameDatabase(*mpVocabulary);  
3. 创建地图对象  mpMap = new Map();  
4. 创建地图显示 帧显示
   mpMapDrawer = new MapDrawer(mpMap, strSettingsFile);//地图显示  
   mpFrameDrawer = new FrameDrawer(mpMap, mpMapDrawer, strSettingsFile);//关键帧显示  
5. 创建目标检测对象 mpRunDetect = make_shared<RunDetect>();  
6. 跟踪线程初始化  mpTracker = new Tracking(this, mpVocabulary, mpFrameDrawer, mpMapDrawer,mpMap, mpKeyFrameDatabase,          
                                          strSettingsFile, mSensor,mpRunDetect);  
7. 局部建图线程  mpLocalMapper = new LocalMapping(mpMap, mSensor==MONOCULAR); 
8. 闭环检测线程  mpLoopCloser = new LoopClosing(mpMap, mpKeyFrameDatabase, mpVocabulary, mSensor!=MONOCULAR);  
9. 可视化线程  mpViewer = new Viewer(this, mpFrameDrawer,mpMapDrawer,mpTracker,strSettingsFile);  
10. 跟踪、局部建图、闭环检测线程之间传递指针……ln180-187  
 
