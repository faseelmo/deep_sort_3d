Forked from [deep sort](https://github.com/nwojke/deep_sort).

### Changes made
1. Added support for scipy 0.23 and above.`
2. Extended the deep sort model to include height (z-axis) and rotation (yaw) of the bounding box.

### Usage

```
from deep_sort_3d.src.tracker import Tracker as DeepSort3D

tracker = DeepSort3D(
    metric=NearestNeighborDistanceMetric("cosine", 0.2, 100),
    max_age=30,
    n_init=3
)

# your code here

detections = ...  # list[detections]
# detection = [objectness, (x,y),(w,h),angle]

deep_sort_detection_list = []
rotation_list = []
feature = np.array([1,0]) # dummy feature

for box in detection:
  objectness, (x, y), (w, h), angle = box
  deep_sort_detection = Detection((x,y,w,h), objectness, dummy_feature))
  detection_list.append(deep_sort_detection)
  detection_rotations_list.append(angle)

tracker.predict()
tracker.update(detection_list, detection_rotations_list)

# Access the tracks
for track in tracker.tracks:
  print(track.track_id)
  print(track.to_tlwh())
  print(track.get_angle())
```
