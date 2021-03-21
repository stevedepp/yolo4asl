**MSDS 462-55   
Final Project Demo Video = YOLO for ASL  
Steve Depp**


**Final project DV - YOLO for ASL**

**Motivation:**
	instant recognition of American Sign Language symbols by an edge device 
	trained in the cloud of course
	—> a suite of products for visually and hearing impaired 

**Ingredients for training in the cloud:**
	browser for COLAB
	Google Drive
	annotated data = 48 hours
	modified YOLO configuration 
		200 samples per 27 classes = 5,400 samples
		1000 iterations per sample = 54,000 iterations
		= 60 hours iterating over 106 layer model
	—> 95% mAP and 90% IOU

**Ingredients for testing at the edge:**
	Nvidia Jetson Nano et al
	camera
	SD card loaded with Jetpack 4.2.1 
	remote desktop to Nano or monitor/keyboard/mouse hooked up


2   
**YOLO for ASL**

**References:**
1.	Bochkovskiy, A., Wang, C. Y., & Liao, H. Y. M. (2020). YOLOv4: Optimal Speed and Accuracy of Object Detection. arXiv preprint arXiv:2004.10934.
2.	Lin, J., Gan, C., & Han, S. (2019). TSM: Temporal Shift Module for Efficient Video Understanding. 2019 IEEE/CVF International Conference on Computer Vision (ICCV), 7082-7092.
3.	Redmon, J., Divvala, S., Girshick, R., & Farhadi, A. (2016). You only look once: Unified, real-time object detection. In Proceedings of the IEEE conference on computer vision and pattern recognition (pp. 779-788).
4.	Visée, R. J., Likitlersuang, J., & Zariffa, J. (2020). An effective and efficient method for detecting hands in egocentric videos for rehabilitation applications. IEEE Transactions on Neural Systems and Rehabilitation Engineering, 28(3), 748-755.

**Helpful links:**   
- Sergio Canu: YOLO V3 – Install and run Yolo on Nvidia Jetson Nano (with GPU)    
  - https://pysource.com/2019/08/29/yolo-v3-install-and-run-yolo-on-nvidia-jetson-nano-with-gpu/  
- Jetson Hacks: Jetson Nano + Raspberry Pi Camera + Jetson Nano – Use More Power!  
  - https://www.jetsonhacks.com/2019/04/02/jetson-nano-raspberry-pi-camera/  
  - https://www.jetsonhacks.com/2019/04/10/jetson-nano-use-more-power/
- JP Redmon  
  - https://github.com/pjreddie/darknet  
- AlexeyAB   
  - https://github.com/AlexeyAB/darknet



3   
**YOLO for ASL**  

**Intermediate steps:  hardware = $ 201.18** 

brain 
- $94.99   
- NVIDIA Jetson Nano Developer Kit (945-13450-0000-100)
- https://www.amazon.com/gp/product/B084DSDDLT/ref=ppx_yo_dt_b_asin_title_o05_s02?ie=UTF8&psc=1   

power
- $14.95
- Adafruit 5V 4A (4000mA) switching power supply   
- https://www.adafruit.com/product/1466

communication  
- $24.39   
- Intel Dual Band Wireless-Ac 8265 w/Bluetooth 8265.NGWMG  
- https://amzn.to/2UcHszJ

vision   
- $27   
- nVidia Jetson Camera  
- https://store.donkeycar.com/products/nvidia-jetson-camera-for-donkey

antenna  
- $4.86  
- Molex Antenna Wi-Fi 3.3dBi Gain 2483.5MHz/5850MHz Film   
- https://www.arrow.com/en/products/2042811100/molex

memory  
- $34.99   
- SanDisk Extreme Plus microSDXC UHS-I Card with Adapter, 128GB, SDSQXBZ-128G-ANCMA  
- https://www.amazon.com/gp/product/B07HMJV355/ref=ppx_yo_dt_b_asin_title_o01_s00?ie=UTF8&psc=1



4   
**YOLO for ASL**

**OS**
- Jetson 4.2.1 flash on SD card
  - https://developer.nvidia.com/jetpack-421-archive

**OS remote install / set up**  
- Etcher
  - https://www.balena.io/etcher/
- brew install screen
- nmap to check available network locations  
  - https://nmap.org/download.html
- VNC connection to Jetson
- app = RealVNC  
  - https://www.realvnc.com/en/connect/download/viewer/  
  - https://medium.com/hacksters-blog/getting-started-with-the-nvidia-jetson-nano-developer-kit-43aa7c298797


5  
**YOLO for ASL**

**Custom objects - amending config files = 4 steps**    

27 classes and 5400 samples require these 4 darknet framework modifications:  
- https://github.com/AlexeyAB/darknet#how-to-train-to-detect-your-custom-objects


6  
**YOLO for ASL**

**Custom objects - labels and bounding boxes**
- compile labelimg
  - https://github.com/tzutalin/labelImg

5400 images from A to Z + space —> bounding boxes + labels
￼



7  
**YOLO for ASL**

**Steps for training custom YOLO**  

*use this Colab notebook:*    
https://colab.research.google.com/drive/1O5hRmzLjUbuh-kksxZGPefBKohgdaBFv?usp=sharing

**step 1: obtain the darknet zip from Steve’s drive**    

https://drive.google.com/file/d/13k7uWAEFmvjKV-0nXGuc4gFv-knwgInv/view?	usp=sharing

**step 2: ensure running CUDA 10**     
- 2a: check CUDA version  
- 2b: delete current CUDA version  
- 2c: install CUDA v10 (answer Y when asked)  
- 2d: confirm CUDA version = 10   

**step 3: compile darknet function**

**step 4: train YOLO ASL by 2 methods**   
- 4a. train from scratch with cfg/yolov4.conv.137   
- 4b. repeat steps 1-3 and continue training with cfg/yolo-obj_20000.weights or latests weight set available

**monitor:**   
- download weights from darknet/backup folder every 1000 iterations  
- observe mAP, IOU, GIUO, avg loss per bounding box per iteration  
- double click darknet/chart.png and darknet/chart_yolo-obj.png learning plots every 100 iterations




8   
**YOLO for ASL**

Steps to test/demo custom YOLO for 27 ASL objects on Nvidia Jetson Nano

1. download and unzipping Steve’s darknet.zip:  

	https://drive.google.com/file/d/13k7uWAEFmvjKV-0nXGuc4gFv-knwgInv/view?usp=sharing  
2. assign environment variables; Jetpack 4.2.1 selected for CUDA 10; so, please specify that here:   
   - export PATH=/usr/local/cuda-10.0/bin${PATH:+:${PATH}}  
   - export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}   
3. build: make   
4. run it   
  - run it on a video; escape key to exit this:    
     ./darknet detector demo build/darknet/x64/data/obj.data cfg/yolo-obj.cfg yolo-obj_16000.weights data/ASLAZ.mov -i 0 -thresh   0.25   
    
  - run it live; escape key to exit this:     
     ./darknet detector demo build/darknet/x64/data/obj.data cfg/yolo-obj.cfg yolo-obj_16000.weights "nvarguscamerasrc auto-exposure=1 ! video/x-raw(memory:NVMM), width=(int)1280, height=(int)720, format=(string)NV12, framerate=(fraction)60/1 ! nvvidconv flip-method=2 ! video/x-raw, width=(int)1280, height=(int)720, format=(string)BGRx ! videoconvert ! video/x-raw, format=(string)BGR ! appsink -e"



9   
**YOLO for ASL**

**Future / next steps:**

- **A better data set** 

  - *An Effective and Efficient Method for Detecting Hands in Egocentric Videos for Rehabilitation Applications*   
    - https://github.com/victordibia/handtracking  
    - https://arxiv.org/pdf/1908.10406.pdf  

- **Video recognition of arms**   

  - *Temporal Shift Module for Efficient Video Understanding*
    - https://github.com/mit-han-lab/temporal-shift-module/tree/master/online_demo
    - https://arxiv.org/abs/1811.08383  
        
- **A better tuned model**   

  - *YOLOv4: Optimal Speed and Accuracy of Object Detection*
    - https://arxiv.org/pdf/2004.10934.pdf
	
- **A different application**

  - *Extreme inbreeding likely spells doom for Isle Royale wolves*
    - https://www.sciencemag.org/news/2016/04/extreme-inbreeding-likely-spells-doom-isle-royale-wolves


10   
**YOLO for wildlife classification**  

a different application: The Wolves of Isle Royale, L. David Mech, U.S. National Park Service, Washington, DC, 1966.
￼


11   
**YOLO for better imaging**

1966 drones												wildlife classification
￼



