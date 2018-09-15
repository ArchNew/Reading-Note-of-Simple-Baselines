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
This process is responsible for all annotation loading, not just for validation.

- Search in other files \
After searching all files for 'score', I finally locked down the key line in JointsDataset.py
```python
score = db_rec['score'] if 'score' in db_rec else 1
```
It seems if I use the setting `USE_GT_BBOX: true`, the system automatically signs 1 to the 'score' section.
Upon further reading, the final score used in cocoapi to evaluate, is the product of bbox_score and kpt_score.
```python
kpt_score = kpt_score / valid_num
# ...
n_p['score'] = kpt_score * box_score
```
