# deep-photo-styletransfer
Based on "[Deep Photo Style Transfer](https://arxiv.org/abs/1703.07511)".
Amended (Now heavily amended) from [here](https://github.com/luanfujun/deep-photo-styletransfer).
PLEASE NOTE RESTRICTIONS ON USAGE OF ORIGINAL CODE.
### Features
* Dockerised for ease of installing.
* Matting Laplacian calculations are faster than original MATLAB code.
* No dependency on MATLAB.
* Adpoted newer neural sytle codebase, adding new features - including multi-GPU.
* Consistent image scaling is managed automatically, rather than having to manually rescale images.
* No longer a requirement to use particular filenames and directories.

## Setup

Build this image using something like:
```
docker build -t deep_photo .
```
To run the container you'll need recent nvidia drivers installed and nvidia-docker (from here: https://github.com/NVIDIA/nvidia-docker). Then run something like:
```
nvidia-docker run -it --name deep_photo deep_photo
```
## Usage

Run:

```python3 deep_photo.py <options>```

### Arguments

```
usage: deep_photo.py [-h] [-content_image CONTENT_IMAGE]
                     [-content_seg CONTENT_SEG] [-style_image STYLE_IMAGE]
                     [-style_blend_weights STYLE_BLEND_WEIGHTS]
                     [-style_seg STYLE_SEG] [-laplacian LAPLACIAN]
                     [-output_image OUTPUT_IMAGE] [-image_size IMAGE_SIZE]
                     [-gpu GPU] [-multigpu_strategy MULTIGPU_STRATEGY]
                     [-content_weight CONTENT_WEIGHT]
                     [-style_weight STYLE_WEIGHT] [-tv_weight TV_WEIGHT]
                     [-num_iterations NUM_ITERATIONS] [-init {random,image}]
                     [-init_image INIT_IMAGE] [-optimizer {lbfgs,adam}]
                     [-learning_rate LEARNING_RATE]
                     [-lbfgs_num_correction LBFGS_NUM_CORRECTION]
                     [-print_iter PRINT_ITER] [-save_iter SAVE_ITER]
                     [-style_scale STYLE_SCALE] [-original_colors {0,1}]
                     [-pooling {max,avg}] [-proto_file PROTO_FILE]
                     [-model_file MODEL_FILE] [-backend {nn,cudnn,clnn}]
                     [-cudnn_autotune] [-seed SEED]
                     [-content_layers CONTENT_LAYERS]
                     [-style_layers STYLE_LAYERS] [-lambda PHOTO_LAMBDA]
                     [-patch PATCH] [-eps EPS] [-f_radius F_RADIUS]
                     [-f_edge F_EDGE] [-color_codes COLOR_CODES] 

optional arguments:
  -h, --help            show this help message and exit
  -content_image CONTENT_IMAGE
                        content image location
  -content_seg CONTENT_SEG
                        content segmentation location
  -style_image STYLE_IMAGE
                        style image locations
  -style_blend_weights STYLE_BLEND_WEIGHTS
                        style image blending weights
  -style_seg STYLE_SEG  style segmentation locations
  -laplacian LAPLACIAN  laplacian file location
  -output_image OUTPUT_IMAGE
                        output image name
  -image_size IMAGE_SIZE
                        Maximum height / width of generated image
  -gpu GPU              GPU indices
  -multigpu_strategy MULTIGPU_STRATEGY
                        multi-GPU layer splits
  -content_weight CONTENT_WEIGHT
                        content weight
  -style_weight STYLE_WEIGHT
                        style weight
  -tv_weight TV_WEIGHT  tv weight
  -num_iterations NUM_ITERATIONS
                        iterations
  -init {random,image}  initialisation type
  -init_image INIT_IMAGE
                        initial image
  -optimizer {lbfgs,adam}
                        optimiser
  -learning_rate LEARNING_RATE
                        learning rate (adam only)
  -lbfgs_num_correction LBFGS_NUM_CORRECTION
                        lbfgs num correction
  -print_iter PRINT_ITER
                        print interval
  -save_iter SAVE_ITER  save interval
  -style_scale STYLE_SCALE
                        style scale
  -original_colors {0,1}
                        use original colours
  -pooling {max,avg}    pooling type
  -proto_file PROTO_FILE
                        VGG 19 proto file location
  -model_file MODEL_FILE
                        VGG 19 model file location
  -backend {nn,cudnn,clnn}
                        backend
  -cudnn_autotune       cudnn autotune flag
  -seed SEED            random number seed
  -content_layers CONTENT_LAYERS
                        VGG 19 content layers
  -style_layers STYLE_LAYERS
                        VGG 19 style layers
  -lambda PHOTO_LAMBDA  photorealism weight
  -patch PATCH          matting patch size
  -eps EPS              matting epsilon
  -f_radius F_RADIUS    f radius
  -f_edge F_EDGE        f edge
  
  -color_codes COLOR_CODES  content mask colors
  
```

### Examples
Using images and masks in the examples directory. Assumes 2 GPUs, but can be changed to one (or more for that matter) easily.

#### Masked style transfer
Example #7 from @luanfujun repo. Results are not identical, but then neither is the process...
```
python3 deep_photo.py -content_image examples/waterfront.png -content_seg examples/waterfront_seg.png -style_image examples/city_night.png -style_seg examples/city_night_seg.png -laplacian examples/waterfront700.csv -output_image examples/waterfront_city_night.png -image_size 700 -gpu 0,1 -multigpu_strategy 8
```
Multi-style image example
```
python3 deep_photo.py -content_image examples/vase.png -content_seg examples/vase_seg.png -style_image examples/fire.png,examples/glass.png -style_seg examples/fire_seg.png,examples/glass_seg.png -laplacian examples/vase700.csv -output_image examples/test.png -image_size 700 -gpu 0,1 -multigpu_strategy 8
```
