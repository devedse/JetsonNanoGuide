# JetsonNanoGuide
This is a Guide to setup a Jetson Nano with Python/Keras to do Object Detection

## Usefull links

https://devtalk.nvidia.com/default/topic/1049071/pytorch-for-jetson-nano/
https://devtalk.nvidia.com/default/topic/1051139/jetson-tx2/questions-about-implementing-retinanet-with-tensorrt-on-jetson-tx2/
https://devtalk.nvidia.com/default/topic/1048776/official-tensorflow-for-jetson-nano-/

I want to start with following this guide:

https://www.pyimagesearch.com/2019/05/06/getting-started-with-the-nvidia-jetson-nano/

This thing doesn't work great:

https://github.com/Archiconda/build-tools/releases

This dude has cool information on the Jetson Nano:

https://www.pyimagesearch.com/author/adrian/

## My Repositories

### DeveLicensePlateRecognizer

https://github.com/devedse/DeveLicensePlateRecognizer

### DS-Parking

https://avanadedigitalstudionl.visualstudio.com/DS-Parking

`git clone https://avanadedigitalstudionl.visualstudio.com/DS-Parking/_git/VideoAnalytics`

### DS-CarDetectionDataSetExtended

`git clone https://avanadedigitalstudionl.visualstudio.com/DS-CarDetectionDataSetExtended/_git/DS-CarDetectionDataSetExtended`

## Install steps

### 1. The prereqs

```
sudo apt-get install git-lfs

Dan deze:
git lfs install

git config --global credential.helper store
```

### 2. Setup pyhon

https://www.pyimagesearch.com/2019/05/06/getting-started-with-the-nvidia-jetson-nano/

## Interesting things

### Jetson Inference

Die Library voor ook wat detection

https://github.com/dusty-nv/jetson-inference

Note: Je moet wel je webcam op id: 0 zetten in een paar van die sample filetjes

### VSCode

From: https://devtalk.nvidia.com/default/topic/1049448/quick-build-guide-for-visual-studio-code-on-the-nano/


```
# Yarn package repo
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list

# Install packages
sudo apt update
sudo apt install -y libx11-dev libxkbfile-dev libsecret-1-dev fakeroot npm curl nodejs nodejs-dev yarn

# Clone repo
git clone https://github.com/Microsoft/vscode.git

pushd vscode

# Reduce max_old_space_size
sed -i 's/4095/2048/g' package.json

# Kill the UI to free up memory ### WARNING THIS WILL DROP YOU TO A CONSOLE
sudo systemctl isolate multi-user

# Fetch deps, build, and create deb
yarn
yarn run gulp vscode-linux-arm64-min
yarn run gulp vscode-linux-arm64-build-deb
sudo dpkg -i ./.build/linux/deb/arm64/deb/code-oss_1.??.?-*_arm64.deb

# Reboot
code-oss
```
