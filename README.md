# DingPy 🛎 

[![GitHub license](https://img.shields.io/github/license/tinahbu/dingpy)](https://github.com/tinahbu/dingpy/blob/master/LICENSE)
[![pypi](https://img.shields.io/badge/pypi-1.0.0-blue)](https://pypi.org/project/dingpy/)

A Python package that plays an audio alert when your program finishes, especially helpful for long running jobs and impatient developers.

## Examples 

```
import dingpy

dingpy.ding()  # plays the default alarm 'japanese_temple_bell'

# to use a different alarm sound:
dingpy.ding(sound='music_box')

# to list all available alarms
dingpy.list_alarms()
```

## Alarm Options

`Dingpy` comes pre-loaded with 10 royalty free alarm sounds (downloaded from http://soundbible.com/) and you can choose which one to use via the `sound` parameter.

- `'beep'`
- `'bell_tibetan'`
- `'birds'`
- `'clock_chimes'`
- `'computer_magic'`
- `'japanese_temple_bell'`
- `'music_box'`
- `'school_bell'`
- `'service_bell'`
- `'tinkle'`

The 10 audio files are packaged and downloaded when you install `dingpy`. You can further customize `dingpy` by asking it to use a mp3 file from your local directory via the `path` parameter:

```
dingpy.ding(path='/absolute_path_to_file/sound.mp3')
```

If you'd like to contribute your mp3 file for other `dingpy` users to access, you can upload it to the public `dingpy` s3 bucket:

```
# sound_name needs to be globally unique
dingpy.upload_alarm(
    file_path='/absolute_path_to_file/sound.mp3', 
    sound_name='beeep') 

# verify that your upload is successful by calling list_alarms again
# and tell it to look for all alarms 
# (without all=True list_alarms will only return the 10 pre-built sounds)
dingpy.list_alarms(all=True)
```

After your mp3 file is uploaded, they can be used by other people if they pass in the sound name and set the `s3` parameter to be `True`:

```
dingpy.ding(sound='beeep', s3=True)
```

Note that the mp3 file will be downloaded each time `dingpy.ding()` is called. 

To delete an uploaded alarm (the 10 pre-loaded alarms can't be deleted):

```
dingpy.delete_alarm('beeep')
```

## Installation 

`DingPy` can be installed via `pip` like this

```
$ pip install dingpy
```

or from the source code like this

```
$ pip install git+https://github.com/tinahbu/dingpy.git
```

or this

```
$ git clone git@github.com:tinahbu/dingpy.git
$ cd dingpy
$ python setup.py install
```

## Prerequisite

For the `pydub` library to work with non-`wav` files like `mp3`, you will need to have `ffmpeg` or `libav` installed locally.

```
$ # install ffmpeg
$ # for Mac
$ brew install ffmpeg --with-theora
$ 
$ # for Linux
$ apt-get install ffmpeg libavcodec-extra
```

```
$ # install libav
$ # for Mac
$ brew install libav --with-libvorbis --with-sdl --with-theora
$ 
$ # for Linux
$ apt-get install libav-tools libavcodec-extra
```

As stated above, the 10 default alarms are packaged with `dingpy` but user uploaded alarms are hosted in a public s3 bucket. So if you'd like to use the customization feature you will have to have aws cli configured. To do that, follow the doc [here](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html).

`dingpy` only works with Python3.

## Future Work

- Make available on conda
- Support other audio formats besides mp3
    + maybe not so important
    + maybe even delete the s3 upload/delete option
- Support text to speech alerts
    + MacOS you can do 
    ```
    import os 
    os.system('say "your program has finished"')
    ```
    + Linux
    ```
    import os 
    os.system('spd-say "your program has finished"')

    # will need to install the speech-dispatcher package in Ubuntu
    sudo apt install speech-dispatcher
    ```
    + for windows it seems you have to call the aws polly service 
- Support smart devices integration for remote server
    + alert user's alexa/google home/pixel phone 
    + [`pushsafer`](https://www.pushsafer.com/en/python)
    + [`pushjack`](https://pypi.org/project/pushjack/)
- Integrate with `pync` to send MacOS notifications 
    + not sure what's the linux/windows equivalent
    

## Inspirations <a name="inspirations"></a>

I always wanted a Python package that notifies me with a ding when my code completes so I can go about doing other work in the meantime. I couldn't really find one after quite some research so I decided to create `dingpy` for myself. And hopefully it will be helpful to you as well. That being said, if most of your work happens in the terminal or if you prefer to have a pop-up MacOS notification than an audible alarm, do checkout the projects below:

- it seems that if you are on windows, you can just do `print('\007')` and it will play a bell sound...
- [`pync`](https://pypi.org/project/pync/) a Python package to send MacOS notifications (Mac only) (it claims to offer sound notification with the pop-up notification but I couldn't make it work)
- [`knockknock`](https://github.com/huggingface/knockknock) is a Python package that sends notifications to users and it supports quite a few channels including slack, email, text, discord, desktop, etc. I think it's super cool but personally I never have notification sounds on for anything and I don't want to enable it just for this package. 
- [`ding`](https://github.com/xxv/ding/) a CLI alarm tool for local and remote jobs (it seems that you will have to provide your own alarm audio file and keep a terminal open running this code the whole time for the alarm to work. not a python package that can be imported)
- [`woof`](https://github.com/msbarry/woof) a set of CLI tools to send notifications (options: music, growl notification, text message, tweet, twitter DM, email, and text-to-speech) (Mac only) (have to save alarm audio locally and modify bash profile to set proper paths, only works in the terminal)
- [`notify2`](https://bitbucket.org/takluyver/pynotify2/src) a Python package that sends a MacOS notification (seems not maintained, after installation I got import error for `dbus` and wasn't able to install `dbus` properlly to test it out)

