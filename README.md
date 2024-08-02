Forked from [deep sort](https://github.com/nwojke/deep_sort).


### Changes made
1. Added support for scipy 0.23 and above. (See [issue](https://github.com/nwojke/deep_sort/issues/330))
2. Extended the deep sort model to include height (z-axis) and rotation (yaw) of the bounding box.
> Note: The Kalman filter has not been modified to account for rotation and height.
Instead, these parameters are stored in the track object as a moving average over the last 10 frames.


### Usage

```python
from deep_sort_3d.src.tracker import Tracker as DeepSort3D
from deep_sort_3d.src.detection import Detection
from deep_sort_3d.src.nn_matching import NearestNeighborDistanceMetric

tracker = DeepSort3D(
    metric=NearestNeighborDistanceMetric("cosine", 0.2, 100),
    max_age=30,
    n_init=3
)

# your code here

detection_list = ...  # list[bbox], bbox = (objectness, (x, y), (w, h), angle)

deep_sort_detection_list = []
rotation_list = []

dummy_feature = np.array([1,0]) # dummy feature

for box in detection_list:
  objectness, (x, y), (w, h), angle = box
  deep_sort_detection = Detection((x,y,w,h), objectness, dummy_feature)
  deep_sort_detection_list.append(deep_sort_detection)
  rotation_list.append(angle)

tracker.predict()
tracker.update(deep_sort_detection_list, rotation_list)

# Access the tracks
for track in tracker.tracks:
  print(track.track_id)
  print(track.to_tlwh())
  print(track.get_angle())
```
