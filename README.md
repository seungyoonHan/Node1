# Node1

1. image_converter
(1) pkg: nesfr3_human_detection 

(2) type: trt_async_ros.py

(3) task: 
  orginal img data subscribing / publish inference results
  create child thread for camera input / inference (TensorRT optimized SSD) while main thread drawing detection results and displaying video

(4) function:
  parse_args(): parse input arguments.
  
  class TrtThread(threading.Thread): It implements the child thread (read images from cam to TRT inferencing). The child thread stores the input image and detectino results (global & condition variable)
    run(): run until running flag is set to False in main thread. trt_ssd.detect(img, self.conf_th)로 detect
  
  class image_converter: parse, and set robot args, publish and subscribe image
    callback(self,data): Converting ROS image messages to OpenCV images, put cv_image to img_raw_queue, find bounidng boxes
  
  bbox(box1, box2): returns the iou of tow bbox. get the coordinates of bbox (if x1min>x1max x1min -w, else do nothing), get the coordinates of intersec rec
  (if x1min>x1max and x2min<x2max calculate iou1 as original(b1_x1 ..) and io2 as previous original box value(b1_x1+w) and set iou as max
  
  calculate_iou(coordinates..): find intersection and calculate iou by ratio of area
  
  duplicate_box_removal(bbox): remove bbox for same object, 
  
  
  
