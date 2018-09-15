## What happend in evaluation time?
There's no bbox score in gt. So I'm curious what would happen if I use the setting `USE_GT_BBOX: true` when I validate a model.
- I start with loading process \
When I use the `USE_GT_BBOX`, it will load the gt into the database. The actual function responsible for loading annotations called
`def _load_coco_keypoint_annotation_kernal(self, index)`. It is in coco.py. Below is what it actually loads.
```python
            rec.append({
                'image': self.image_path_from_index(index),
                'center': center,
                'scale': scale,
                'joints_3d': joints_3d, # joints coordinates
                'joints_3d_vis': joints_3d_vis, #joints visibility
                'filename': '',
                'imgnum': 0,
            })

        return rec
```
