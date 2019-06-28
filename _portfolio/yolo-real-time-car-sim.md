---
title: "Real Time Object Detection in Autonomous Driving Simulator"
excerpt: "ML web application to predict wait times on Disney World rides in various parks and display in angular app."
header:
  image: /assets/images/yolo-image.png
  teaser: assets/images/yolo-object-detection.png
sidebar:
  - title: "Role"
    image: /assets/images/dennis-simon.jpg
    image_alt: "logo"
    text: "ML Engineer, Paper creator"
  - title: "Responsibilities"
    text: "Enable ROS based communication between simulator and client and implement object detection.."

---
## Disclaimer

All work is done for educational use and is not monetized, shell script taken from LGSVL lane following project and revised to implement object detection instead with YOLO model.  YOLO model created by Darknet and used for educational use.

## Technologies

Python, Linux, C++, YOLOv2, YOLOv2 Tiny, Keras, Tensorflow, LGSVL Simulator


### Overview

+ This project aims to take LGSVL simulator and enable real time object detection within the simulator using YOLOv2 darknet model.
+ Tiny YOLOv2 and YOLOv2 comparison.
+ Conversion of darknet model to Keras using YAD2k library.
+ Detection of objects boxed and classified with percentage of assurance.
+ Should be done in real time and using direct camera feed from LGSVL simulator using ROS communication bridge.
+ Shell Docker taken from LGSVL lane following project.
+ NOTE: Predictions are not up to speed, predicts once every second or two and backlogs, likely due to tensor graph recreation so predictions not up to speed.
+ [Github](https://github.com/DVSimon/ObjectDetection_LGSVL)
+ [Paper](https://drive.google.com/file/d/1kSPJaYjbIcJ0Imjg-Kv2sFp44Ll90XrT/view?usp=sharing)
+ [Video](https://www.youtube.com/watch?v=piksbwYxSM4)


### ROS Communication bridge and model call

+ Setup pub/sub subscription and get YOLO model path
```python
def get_model(self, model_path):
        self.get_logger().info('Model loading: {}'.format(model_path))
        model = load_model(model_path)
        self.get_logger().info('Model loaded: {}'.format(model_path))
        return model
```

+ Load model
```python
self.image_sub = self.create_subscription(CompressedImage, '/simulator/sensor/camera/center/compressed', self.image_callback)
self.object_model_path = self.get_parameter('object_model_path').value
```

### Image preprocessing for cv2/PIL libraries
+ Convert image to cv2(and color) for preprocessing
```python
c = np.fromstring(bytes(img.data), np.uint8)
img = cv2.imdecode(c, cv2.IMREAD_COLOR)
#im_pil
img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
im_pil = Image.fromarray(img)
width, height = im_pil.size
width = np.array(width, dtype=float)
height = np.array(height, dtype=float)
```

### YOLO
+ Get class names and anchors for yolo model and get yolo outputs using head
```python
#class names and anchors
class_names = read_classes("/lanefollowing/ros2_ws/src/lane_following/model/coco_classes.txt")
anchors = read_anchors("/lanefollowing/ros2_ws/src/lane_following/model/yolov2-tiny_anchors.txt")
#yolo head
yolo_outputs = yolo_head(self.object_model.output, anchors, len(class_names))
```
+ Yolo evaluation and image preprocessing call
```python
boxes, scores, classes = yolo_eval(yolo_outputs, image_shape)
pil_img, img_data = preprocess_image_object(im_pil, model_image_size = (608, 608))
```
+ Yolo prediction
```python
out_scores, out_boxes, out_classes = sess.run([scores, boxes, classes],feed_dict={self.object_model.input:img_data,K.learning_phase(): 0})
t1 = time.time()
self.inference_time = t1 - t0
```

### Prediction image stream window creation
+ Draw box and display predictions to console
```python
print('Found {} boxes for '.format(len(out_boxes)))
colors = generate_colors(class_names)
draw_boxes(pil_img, out_scores, out_boxes, out_classes, class_names, colors)
```
+ Put images predicted and drawn upon into cv2 image stream for real time display.
```python
image = np.asarray(pil_img)
image = cv2.cvtColor(image, cv2.COLOR_RGB2BGR)
cv2.putText(image, "Prediction time: %d ms" % (self.inference_time * 1000), (30, 220), cv2.FONT_HERSHEY_PLAIN, 3, (0, 255, 0), 2)
cv2.putText(image, "Frame speed: %d fps" % (self.fps), (30, 270), cv2.FONT_HERSHEY_PLAIN, 3, (0, 255, 0), 2)
image = cv2.resize(image, (round(image.shape[1] / 2), round(image.shape[0] / 2)), interpolation=cv2.INTER_AREA)
cv2.imshow('YOLO Detector', image)
cv2.waitKey(1)
```
