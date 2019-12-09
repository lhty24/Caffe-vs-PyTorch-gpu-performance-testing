# Caffe-vs-PyTorch-Inference-gpu-performance-testing

### Context
The purpose of the testing is to compare the inference gpu performance between the two frameworks Caffe and Pytorch. 
Particularly, the inference performance between FOR and Detectron2, both implemented Faster-RCNN, with the framework of Caffe and Pytorch respectively.

For models, FOR uses a model with the backbone configuration of VGG16; while Detectron2 uses a range of ResNet backbones (with RN50-FPN-3x being the best and fastest).
The reason why the models have different backbone is that, Detectron2 is a fairly new system, and there are newer backbone configurations for it, which are not available for Caffe. There is no point to make a strictly apple-to-apple comparison, as no one will use Detectron2 with an old config, and there isn't any newer pre-trained model for FOR.

### Inference Speed
|   |   |   |
|---|---|---|
|   |   |   |
|   |   |   |



									            Inference				    Load Model					
FOR (Caffe, VGG16)					  0.35s / 1.1s				1.7s (~500MB)				

Detectron2 (Pytorch, RN50)		0.29s					      4.1 (1st, ~160MB)
															                    0.8 (after, ~160MB)
                                                  
Note: 
1. FOR Inference time: around 0.35s perimage (calling im_detect function alone), total around 1.2s (handling request, setting parameters)
2. Detectron2 loading model: 4.1s the first time, all models afterwards take much less time; this is because when doing the first inference, the underlying code creates a predictor, which is not to be repeated after the first inference, and it takes about 3 seconds. Loading model itself does not take too long.

Clearly, Detectron2 has a great advantage when it comes to inference. It takes about 17% less time comparing to FOR for each inference.

### GPU Memory Usage
In terms of gpu memory usage, Detectron2 is slightly better. FOR with VGG16 takes less than 1.4GB gpu memry, while Detectron2 with RN50-FPN-3x takes less than 1.1GB gpu memory at peak.
Note that Pytorch's memory usage is not directly reflected by the command `nvidia-smi`, as it shows the maximum memory cached since the beginning of the program, as opposed to the actual real time memory usage. There are several Pytorch CUDA semantics available for checking the memory closely, please refer to [Pytorch Memory management](https://pytorch.org/docs/stable/notes/cuda.html#cuda-memory-management)
Detectron2 with RN50-FPN-3x takes about 14% less gpu memory compared to FOR with VGG16.

### Conclusion
To conclude, Detectron2 with RN50-FPN-3x is a better inference solution compared to FOR with VGG16.

### Scripts and Details
Scripts used for testing and log files will be included in the repo.

