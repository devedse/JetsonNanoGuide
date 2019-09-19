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

### 3. Setup OpenCV

Follow this guide:

--https://github.com/jetsonhacks/buildOpenCVXavier--

Fixed issue where OpenCV addons could not be used. E.g. Aruca for marker detection.
https://github.com/devedse/buildOpenCVXavier


Afterwards copy this file to the virtualenv to make OpenCV work in virtual env too:
```
jetsonhacks build:
/usr/local/lib/python3.6/dist-packages/cv2.cpython-36m-aarch64-linux-gnu.so

Devedse build:
/usr/local/lib/python3.6/dist-packages/cv2/python-3.6/cv2.cpython-36m-aarch64-linux-gnu.so

to
/home/avanade/.virtualenvs/deep_learning/lib/python3.6/site-packages/cv2.cpython-36m-aarch64-linux-gnu.so
```

### 4. Setup Keras-Retinanet

```
workon deep_learning

pip install all these:
keras six scipy cython Pillow progressbar2 matplotlib graphviz scikit-learn pydot flask flask-restful

pip install keras-resnet

# Install keras retinanet
git clone https://github.com/fizyr/keras-retinanet
cd to this folder
modify setup.py and remove the dependency on OpenCV
pip install .
```

### 5. Setup other Python libraries

**MQTT:**
http://www.steves-internet-guide.com/into-mqtt-python-client/
```
workon deep_learning
pip install paho-mqtt
```

## Interesting things

### Jetson Inference

Die Library voor ook wat detection

https://github.com/dusty-nv/jetson-inference

Note: Je moet wel je webcam op id: 0 zetten in een paar van die sample filetjes

### VSCode

From: https://devtalk.nvidia.com/default/topic/1049448/quick-build-guide-for-visual-studio-code-on-the-nano/


```Shell
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
## Usefull Commands

```Python
# Start python environment
workon deep_learning

# Stop python environment
deactivate
```

## Docker Raspberry PI Like Custom Vision thing

```
(Fill in access key as password)
sudo docker login -u avanadedigitalstudionl avanadedigitalstudionl.azurecr.io

sudo docker pull avanadedigitalstudionl.azurecr.io/dsobjectdetectorappdocker
```

And to run it:
```
How to run locally:
==============================================
sudo docker run -p 127.0.0.1:80:80 -d avanadedigitalstudionl.azurecr.io/dsobjectdetectorappdocker
sudo docker run -it --rm -p 0.0.0.0:80:80 avanadedigitalstudionl.azurecr.io/dsobjectdetectorappdocker

Then use your favorite tool to connect to the end points.

POST http://127.0.0.1/image with multipart/form-data using the imageData key
e.g
	curl -X POST http://127.0.0.1/image -F imageData=@some_file_name.jpg

POST http://127.0.0.1/image with application/octet-stream
e.g.
	curl -X POST http://127.0.0.1/image -H "Content-Type: application/octet-stream" --data-binary @some_file_name.jpg

POST http://127.0.0.1/url with a json body of { "url": "<test url here>" }
e.g.
    curl -X POST http://127.0.0.1/url -d '{ "url": "<test url here>" }'

For information on how to use these files to create and deploy through AzureML check out the readme.txt in the azureml directory.
```

Ofwel bijvoorbeeld deze:
```
curl -X POST http://127.0.0.1/image -F imageData=@/home/Avanade/Desktop/Detection/stuff/Image.jpg
```

## Automating stuff

### Boot to terminal mode

From: https://devtalk.nvidia.com/default/topic/1050739/how-to-boot-jetson-nano-in-text-mode-/

It's also possible to directly boot to terminal mode

Disable GUI
```
sudo systemctl set-default multi-user.target
```

Enable GUI
```
sudo systemctl set-default graphical.target
```

Boot to GUI once
```
sudo systemctl start gdm3.service
```

### Autologin in Terminal

From: https://askubuntu.com/questions/175248/how-to-autologin-without-entering-username-and-passwordin-text-mode

Run this
```
sudo systemctl edit getty@tty1
```

Enter this (replace avanade by your username)
```
[Service]
ExecStart=
ExecStart=ExecStart=-/sbin/agetty -a avanade --noclear %I $TERM
```

Reboot

### Auto start script on boot

From: https://raspberrypi.stackexchange.com/questions/8734/execute-script-on-start-up

Create startup script:

```
sudo nano /etc/init.d/superscript
```

Enter the following:

```
#!/bin/bash
source `which virtualenvwrapper.sh`
cd /home/avanade/Desktop/Detection/VideoAnalytics/Src
workon deep_learning
python CarFinderWebCamMqttAruco.py
```

Save the file with Ctrl + X, then Y, then enter

Chmod the file:

```
sudo chmod 755 /etc/init.d/superscript
```

Register to run at startup:

```
sudo update-rc.d superscript defaults
```

## Starting commands

### Start the Car Detector

Open a terminal and type:
```
sudo systemctl isolate multi-user
```

The UI now closes and you end up in a full screen terminal.
Login using:
Username: avanade
Password: ********

Then type:
```
cd Desktop/Detection/VideoAnalytics/Src
workon deep_learning           # Simply type workon <tab> and it will autocomplete
python CarFinderWebCamMqttAruco.py
```
