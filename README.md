# Working Reolink 811A 4k Camera - Frigate-Config

This is the configuration that I found to actually work with smooth 4k. 

Trying to figure this out from random outdated articles was rough.

This was tested on the latest firmware as of August, 2025. 

The working setup was ultimately switching to go2rtc based on this article https://docs.frigate.video/configuration/restream#with-sub-stream


## Frigate config

```yaml
mqtt:
  enabled: false

go2rtc:
  streams:
    back:
      - https://cameraIp/flv?port=1935&app=bcs&stream=channel0_main.bcs&user=username&password=passwordValue
      - ffmpeg:back#audio=opus
    back_sub:
      - https://cameraIp/flv?port=1935&app=bcs&stream=channel0_ext.bcs&user=username&password=passwordValue
      - ffmpeg:back_sub#audio=opus

cameras:
  back:
    ffmpeg:
      output_args:
        record: preset-record-generic-audio-copy
      inputs:
        - path: rtsp://127.0.0.1:8554/back # <--- the name here must match the name of the camera in restream
          input_args: preset-rtsp-restream
          roles:
            - record
        - path: rtsp://127.0.0.1:8554/back_sub # <--- the name here must match the name of the camera_sub in restream
          input_args: preset-rtsp-restream
          roles:
            - audio # <- only necessary if audio detection is enabled
            - detect
```

## Camera Settings

<img width="502" height="585" alt="image" src="https://github.com/user-attachments/assets/5847d5d2-0454-4a3d-a12c-1456b77ee3da" />

<img width="691" height="637" alt="image" src="https://github.com/user-attachments/assets/8cc28784-f11a-47d8-9dcb-72ecb3448a2d" />

<img width="505" height="562" alt="image" src="https://github.com/user-attachments/assets/a53c0034-d6d0-405e-b816-fc97498da4a2" />

## HTTPS Note:
My example config uses HTTPS. If you don't want to use HTTPS, you can switch to http in the yaml config for the 2 streams. 
Then in your camera settings you can enable http in the Server Settings. 
