# VideoProcessingFramework

VPF stands for Video Processing Framework. It’s set of C++ libraries and Python bindings which provides full HW acceleration for video processing tasks such as decoding, encoding, transcoding and GPU-accelerated color space and pixel format conversions.

VPF also supports exporting GPU memory objects such as decoded video frames to PyTorch tensors without Host to Device copies. Check the [Wiki page](https://github.com/NVIDIA/VideoProcessingFramework/wiki/Building-from-source) on how to build from source.

## Docker Instructions (Linux)

1. Install [docker](https://docs.docker.com/engine/install/ubuntu/),  [docker-compose](https://docs.docker.com/compose/install/) and [nvidia-docker](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html) following the official instructions for your distribution. You can find your distribution as follows.

```
lsb_release -a
Distributor ID:	Ubuntu
Description:	Ubuntu 20.04.3 LTS
Release:	20.04
Codename:	focal
```
2. Install dependencies

```
# the basics
sudo apt-get update
sudo apt-get install git build-essential python3 python3-pip python3-virtualenv
sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common

# python poetry
curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/install-poetry.py | python3
```

3. Set path to [Video_Codec_SDK](https://developer.nvidia.com/nvidia-video-codec-sdk)

```
# Done once
export VIDEO_CODEC_SDK=<path_to_your_Video_Codec_SDK>
export DOCKER_BUILDKIT=1
export COMPOSE_DOCKER_CLI_BUILD=1
cp -a $(VIDEO_CODEC_SDK) Video_Codec_SDK
```

4. Build & Run image

```
docker-compose -f docker/docker-compose.yml build vpf
# Get test sample
wget http://www.scikit-video.org/stable/_static/bikes.mp4 $HOME/Downloads/
# run image
docker-compose -f docker/docker-compose.yml run -v $HOME/Downloads:/Downloads vpf
# or this way
docker run  -it --gpus=all -e NVIDIA_DRIVER_CAPABILITIES=video,compute,utility -v $HOME/Downloads:/Downloads nvidia/videoprocessingframework:vpf
python Tests.py 0 /Downloads/bikes.mp4 /Downloads/bikes-vpf.mp4
```

You can build [`tensorrt`](https://developer.nvidia.com/tensorrt) enabled image by replacing `vpf` with `vpf-tensorrt` in the above steps and test the following.

```
python SampleTensorRTResnet.py 0 /Downloads/bikes.mp4
```
## Community Support
If you did not find the information you need or if you have further questions or problems, you are very welcome to join the developer community at [NVIDIA](https://forums.developer.nvidia.com/categories). We have dedicated categories covering diverse topics related to [video processing and codecs](https://forums.developer.nvidia.com/c/professional-graphics-and-rendering/video-technologies/184).

The forums are also a place where we would be happy to hear about how you made use of VPF in your project.