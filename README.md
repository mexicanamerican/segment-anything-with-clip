# Segment Anything with Clip
[[HuggingFace Space](https://huggingface.co/spaces/curt-park/segment-anything-with-clip)] | [[COLAB]()] | [[Demo Video](https://youtu.be/vM7MfAc3BdQ)]

Meta released [a new foundation model for segmentation tasks](https://ai.facebook.com/blog/segment-anything-foundation-model-image-segmentation/).
It aims to resolve downstream segmentation tasks with prompt engineering, such as forground / background points, bounding box, mask, free-formed text.
However, the text prompt is not released yet.

Alternatively, I took the following steps:
1. Get all object proposals generated by SAM (Segment Anything Model).
2. Crop the object regions by bounding boxes.
3. Get cropped images' features and a query feature from [CLIP](https://openai.com/research/clip).
4. Calculate the similarity between image features and the query feature.
```python
# How to get the similarity.
img_features = clip.encode_image(preprocessed)
txt_features = clip.encode_text(token)
img_features /= img_features.norm(dim=-1, keepdim=True)
txt_features /= txt_features.norm(dim=-1, keepdim=True)
similarity = (100.0 * img_features @ txt_features.T).softmax(dim=0)
```

## How to run on local
[Anaconda](https://www.anaconda.com/) is required before start setup.
```bash
make env
conda activate segment-anything-with-clip
make setup
```

```bash
# this executes GRadio server.
make run
```
Open http://localhost:7860/
<img width="1270" alt="" src="https://user-images.githubusercontent.com/14961526/230437084-79ef6e02-a254-421e-bd4c-32e87415c623.png">


## References
- https://github.com/facebookresearch/segment-anything
- https://github.com/openai/CLIP
