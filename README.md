# Working Reolink 811A 4k Camera - Frigate-Config

This is the configuration that I found to actually work with smooth 4k. 

Trying to figure this out from random outdated articles was rough.

This was tested on the latest firmware as of August, 2025. 

The working setup was ultimately switching to go2rtc based on this article https://docs.frigate.video/configuration/restream#with-sub-stream



```yaml
mqtt:
  enabled: false

go2rtc:
  streams:
    back:
      - http://cameraIp/flv?port=1935&app=bcs&stream=channel0_main.bcs&user=username&password=passwordValue
      - ffmpeg:back#audio=opus
    back_sub:
      - http://cameraIp/flv?port=1935&app=bcs&stream=channel0_ext.bcs&user=username&password=passwordValue
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

Some other things that I changed:

Enabled HTTP and RSTP in the camera settings.
