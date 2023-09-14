# V4L2 Camera

"This app captures video streams using Video4Linux and visualizes them using Holoviz." 
See following references for further details: 
https://github.com/nvidia-holoscan/holoscan-sdk/tree/main/examples/v4l2_camera 


## Use with V4L2 Loopback Devices

```
sudo apt-get install v4l2loopback-dkms ffmpeg
sudo modprobe v4l2loopback video_nr=3 max_buffers=4
ffmpeg -stream_loop -1 -re -i /path/to/video.mp4 -pix_fmt yuyv422 -f v4l2 /dev/video3
```
https://github.com/nvidia-holoscan/holoscan-sdk/tree/main/examples/v4l2_camera#example-streaming-an-mp4-as-a-loopback-device 

## Checking video formats in cagx

v4l2-ctl -d /dev/video0 --list-formats-ext
```
ioctl: VIDIOC_ENUM_FMT
	Type: Video Capture

	[0]: 'AR24' (32-bit BGRA 8-8-8-8)
		Size: Discrete 1920x1080
			Interval: Discrete 0.017s (60.000 fps)
		Size: Discrete 3840x2160
			Interval: Discrete 0.017s (60.000 fps)
		Size: Discrete 1280x720
			Interval: Discrete 0.017s (60.000 fps)
```

v4l2-ctl -d /dev/video1 --list-formats-ext
```
ioctl: VIDIOC_ENUM_FMT
	Type: Video Capture

	[0]: 'YUYV' (YUYV 4:2:2)
		Size: Discrete 640x480
			Interval: Discrete 0.033s (30.000 fps)
			Interval: Discrete 0.040s (25.000 fps)
			Interval: Discrete 0.050s (20.000 fps)
			Interval: Discrete 0.067s (15.000 fps)
			Interval: Discrete 0.100s (10.000 fps)
			Interval: Discrete 0.200s (5.000 fps)

	...

	[1]: 'MJPG' (Motion-JPEG, compressed)
		Size: Discrete 640x480
			Interval: Discrete 0.033s (30.000 fps)
			Interval: Discrete 0.040s (25.000 fps)
			Interval: Discrete 0.050s (20.000 fps)
			Interval: Discrete 0.067s (15.000 fps)
			Interval: Discrete 0.100s (10.000 fps)
			Interval: Discrete 0.200s (5.000 fps)

	...

```


## Running example
1. Edit yaml to make use of the right parameters:
	`device: "/dev/video1"`
	`width: 160`
	`height: 120`
	`pixel_format: "YUYV"


2. Lauch python app.

Terminal output
```

cd /examples/v4l2_camera/python
python v4l2_camera.py 


[info] [gxf_executor.cpp:210] Creating context
[info] [gxf_executor.cpp:1595] Loading extensions from configs...
[info] [gxf_executor.cpp:1741] Activating Graph...
[info] [gxf_executor.cpp:1771] Running Graph...
[info] [gxf_executor.cpp:1773] Waiting for completion...
[info] [gxf_executor.cpp:1774] Graph execution waiting. Fragment: 
[info] [greedy_scheduler.cpp:190] Scheduling 2 entities
[info] [context.cpp:50] _______________
[info] [context.cpp:50] Vulkan Version:
[info] [context.cpp:50]  - available:  1.2.131
[info] [context.cpp:50]  - requesting: 1.2.0
[info] [context.cpp:50] ______________________
[info] [context.cpp:50] Used Instance Layers :
[info] [context.cpp:50] 
[info] [context.cpp:50] Used Instance Extensions :
[info] [context.cpp:50] VK_KHR_surface
[info] [context.cpp:50] VK_KHR_xcb_surface
[info] [context.cpp:50] VK_EXT_debug_utils
[info] [context.cpp:50] VK_KHR_external_memory_capabilities
[info] [context.cpp:50] ____________________
[info] [context.cpp:50] Compatible Devices :
[info] [context.cpp:50] 0: Quadro RTX 6000
[info] [context.cpp:50] Physical devices found : 
[info] [context.cpp:50] 1
[info] [context.cpp:50] ________________________
[info] [context.cpp:50] Used Device Extensions :
[info] [context.cpp:50] VK_KHR_swapchain
[info] [context.cpp:50] VK_KHR_external_memory
[info] [context.cpp:50] VK_KHR_external_memory_fd
[info] [context.cpp:50] VK_KHR_external_semaphore
[info] [context.cpp:50] VK_KHR_external_semaphore_fd
[info] [context.cpp:50] VK_KHR_push_descriptor
[info] [context.cpp:50] VK_EXT_line_rasterization
[info] [context.cpp:50] 
[info] [vulkan_app.cpp:777] Using device 0: Quadro RTX 6000 (UUID aa494f54ba4286b71c9ddc3f846)
[warning] [cuda_stream_handler.hpp:222] Parameter `cuda_stream_pool` is not set, using the default CUDA stream for CUDA operations.
[info] [holoviz.cpp:1425] Input spec:
- type: color
  name: ""
  opacity: 1.000000
  priority: 0

[info] [greedy_scheduler.cpp:369] Scheduler stopped: Some entities are waiting for execution, but there are no periodic or async entities to get out of the deadlock.
[info] [greedy_scheduler.cpp:398] Scheduler finished.
[info] [gxf_executor.cpp:1783] Graph execution deactivating. Fragment: 
[info] [gxf_executor.cpp:1784] Deactivating Graph...
[info] [gxf_executor.cpp:1787] Graph execution finished. Fragment: 
[info] [gxf_executor.cpp:229] Destroying context
```

