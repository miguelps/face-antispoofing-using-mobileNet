# CNN for face-anti-spoofing

The face anti-spoofing is an technique that could prevent face-spoofing attack. For example, an intruder might use a photo of the legal user to "deceive" the face recognition system. Thus it is important to use the face anti-spoofing technique to enhance the security of the system.

![facial-anti-spoofing](https://paperswithcode.com/media/tasks/facial-anti-spoofing_gHfingq.png)

All the face images listed below are in the dataset of CASIA-FASD

## MobileNet
An implementation of `Google MobileNet` introduced in TensorFlow. According to the authors, `MobileNet` is a computationally efficient CNN architecture designed specifically for mobile devices with very limited computing power. It can be used for different applications including: Object-Detection, Finegrain Classification, Face Attributes and Large Scale Geo-Localization.

---

Link to the original paper: [MobileNets: Efficient Convolutional Neural Networks for Mobile Vision Applications](https://arxiv.org/abs/1704.04861)

This implementation was made to be clearer than TensorFlow original implementation. It was also made to be an example of a common DL software architecture. The weights/biases/parameters from the pretrained ImageNet model that was implemented by TensorFlow are dumped to a dictionary in pickle format file (`pretrained_weights/mobilenet_v1.pkl`) to allow a less restrictive way of loading them.

## Depthwise Separable Convolution
![Fig](figures/dws.png)

## ReLU6
The paper uses ReLU6 as an activation function. ReLU6 was first introduced in [Convolutional Deep Belief Networks on CIFAR-10](https://www.cs.toronto.edu/~kriz/conv-cifar10-aug2010.pdf) as a ReLU with clipping its output at 6.0.

---

## Why MobileNet?

MobileNet is an architecture which is more suitable for mobile and embedded based vision applications where there is lack of compute power. This architecture was proposed by Google.

## Usage

### Main Dependencies

```
Python 3 or above
tensorflow 1.3.0
numpy 1.13.1
tqdm 4.15.0
easydict 1.7
matplotlib 2.0.2
pillow 5.0.0
```

## Prepare Dataset

CASIA-FASD datasets are consist of videos, each of which is made of 100 to 200 video frames. For each video, I captured 30 frames (with the same interval between each frame). Then, with the Haar_classifier, I was able to crop a person’s face from an image. These images make up training datasets and test datasets.

### Train and Test

1. Prepare your data, and modify the data_loader.py/DataLoader/load_data() method.
1. Modify the config/test.json to meet your needs.

Note: If you want to test that the model is pretrained and working properly, I've added some test images from different classes in directory 'data/'. All of them are classified correctly.

### Run

```python
python3 main.py --config config/test.json
```

The file 'test.json' is just an example of a file. If you run it as is, it will test the model against the images in directory 'data/test_images'. You can create your own configuration file for training/testing.

## Benchmarking

In my implementation, I have achieved approximately 1140 MFLOPS. The paper counts multiplication+addition as one unit. My result verifies the paper as roughly dividing 1140 by 2 is equal to 569 unit.

To calculate the FLOPs in TensorFlow, make sure to set the batch size equal to 1, and execute the following line when the model is loaded into memory.

```python
    tf.profiler.profile(
        tf.get_default_graph(),
        options=tf.profiler.ProfileOptionBuilder.float_operation(),
        cmd='scope')
```

I've already implemented this function. It's called ```calculate_flops()``` in `utils.py`. Use it directly if you want.

## Updates

* Inference and training are working properly.

## License

This project is licensed under the Apache License 2.0 - see the LICENSE file for details.

## Test

![tset](https://i.ibb.co/G2784c7/test.png)
