## Install

### rpm-ostree

```bash
rpm-ostree install freerdp
```

### flatpak

```bash
flatpak install org.freedesktop.Platform.ffmpeg-full
flatpak install flathub com.freerdp.FreeRDP
```

## Example

```bash
xfreerdp /v:192.168.0.108:3389 /u:yshngg /p:password

flatpak run com.freerdp.FreeRDP /v:192.168.0.108:3389 /u:yshngg /p:password
```

## QA

### Failed to create h264 codec context

https://gitlab.com/freedesktop-sdk/freedesktop-sdk/-/work_items/1081

https://github.com/FreeRDP/FreeRDP/issues/6383

## Reference

https://www.freerdp.com/
https://github.com/FreeRDP/FreeRDP
https://flathub.org/en/apps/com.freerdp.FreeRDP
