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

```console
yshngg@fedora:/var/home/yshngg$ xfreerdp /v:192.168.0.108:3389 /u:yshngg /p:password [20:38:11:438] [31843:00007c63] [WARN][com.freerdp.client.common.cmdline] - [warn_credential_args]: Using /p is insecure [20:38:11:438] [31843:00007c63] [WARN][com.freerdp.client.common.cmdline] - [warn_credential_args]: Passing credentials or secrets via command line might expose these in the process list [20:38:11:438] [31843:00007c63] [WARN][com.freerdp.client.common.cmdline] - [warn_credential_args]: Consider using one of the following (more secure) alternatives: [20:38:11:438] [31843:00007c63] [WARN][com.freerdp.client.common.cmdline] - [warn_credential_args]: - /args-from: pipe in arguments from stdin, file, file descriptor or environment variable [20:38:11:438] [31843:00007c63] [WARN][com.freerdp.client.common.cmdline] - [warn_credential_args]: - /from-stdin pass the credential via stdin [20:38:11:438] [31843:00007c63] [WARN][com.freerdp.client.common.cmdline] - [warn_credential_args]: - set environment variable FREERDP_ASKPASS to have a gui tool query for credentials [20:38:11:440] [31843:00007c65] [WARN][com.freerdp.client.x11] - [load_map_from_xkbfile]: : keycode: 0x08 -> no RDP scancode found [20:38:11:440] [31843:00007c65] [WARN][com.freerdp.client.x11] - [load_map_from_xkbfile]: ZEHA: keycode: 0x5d -> no RDP scancode found [20:38:11:442] [31843:00007c65] [ERROR][com.freerdp.codec] - [openh264_init]: Failed to create OpenH264 decoder: 3 [20:38:11:442] [31843:00007c65] [WARN][com.freerdp.core.codecs] - [freerdp_client_codecs_prepare]: Failed to create h264 codec context [20:38:11:445] [31843:00007c65] [WARN][com.freerdp.core.rdp] - [log_build_warn][0x558912b2b760]: ************************************************* [20:38:11:445] [31843:00007c65] [WARN][com.freerdp.core.rdp] - [log_build_warn][0x558912b2b760]: This build is using [experimental] build options: [20:38:11:445] [31843:00007c65] [WARN][com.freerdp.core.rdp] - [log_build_warn][0x558912b2b760]: * 'WITH_GFX_AV1=ON' [20:38:11:445] [31843:00007c65] [WARN][com.freerdp.core.rdp] - [log_build_warn][0x558912b2b760]: * [20:38:11:445] [31843:00007c65] [WARN][com.freerdp.core.rdp] - [log_build_warn][0x558912b2b760]: [experimental] build options might crash the application [20:38:11:445] [31843:00007c65] [WARN][com.freerdp.core.rdp] - [log_build_warn][0x558912b2b760]: ************************************************* [20:38:11:545] [31843:00007c65] [WARN][com.freerdp.crypto] - [verify_cb]: Certificate verification failure 'self-signed certificate (18)' at stack position 0 [20:38:11:545] [31843:00007c65] [WARN][com.freerdp.crypto] - [verify_cb]: CN = GNOME, C = US [20:38:11:546] [31843:00007c65] [ERROR][com.winpr.sspi.Kerberos] - [kerberos_AcquireCredentialsHandleA]: krb5_parse_name (Configuration file does not specify default realm [-1765328160]) [20:38:11:546] [31843:00007c65] [ERROR][com.winpr.sspi.Kerberos] - [kerberos_AcquireCredentialsHandleA]: krb5_parse_name (Configuration file does not specify default realm [-1765328160]) [20:38:11:693] [31843:00007c65] [INFO][com.freerdp.gdi] - [gdi_init_ex]: Local framebuffer format PIXEL_FORMAT_BGRX32 [20:38:11:693] [31843:00007c65] [INFO][com.freerdp.gdi] - [gdi_init_ex]: Remote framebuffer format PIXEL_FORMAT_BGRA32 [20:38:11:695] [31843:00007c65] [INFO][com.freerdp.channels.rdpsnd.client] - [rdpsnd_load_device_plugin]: [static] Loaded fake backend for rdpsnd [20:38:11:695] [31843:00007c65] [INFO][com.freerdp.channels.drdynvc.client] - [dvcman_load_addin]: Loading Dynamic Virtual Channel ainput [20:38:11:696] [31843:00007c65] [INFO][com.freerdp.channels.drdynvc.client] - [dvcman_load_addin]: Loading Dynamic Virtual Channel rdpgfx [20:38:11:696] [31843:00007c65] [INFO][com.freerdp.channels.drdynvc.client] - [dvcman_load_addin]: Loading Dynamic Virtual Channel disp [20:38:11:696] [31843:00007c65] [INFO][com.freerdp.channels.drdynvc.client] - [dvcman_load_addin]: Loading Dynamic Virtual Channel rdpsnd [20:38:14:365] [31843:00007c65] [INFO][com.freerdp.core.redirection] - [rdp_recv_server_redirection_pdu]: flags: 0x0400, length: 3439, sessionID: 0x00000000, redirFlags: LB_LOAD_BALANCE_INFO|LB_USERNAME|LB_PASSWORD|LB_PASSWORD_IS_PK_ENCRYPTED|LB_REDIRECTION_GUID|LB_TARGET_CERTIFICATE [0x0001C016] [20:38:14:368] [31843:00007c65] [ERROR][com.freerdp.codec] - [openh264_init]: Failed to create OpenH264 decoder: 3 [20:38:14:368] [31843:00007c65] [WARN][com.freerdp.core.codecs] - [freerdp_client_codecs_prepare]: Failed to create h264 codec context [20:38:14:582] [31843:00007c65] [INFO][com.freerdp.channels.rdpsnd.client] - [rdpsnd_load_device_plugin]: [static] Loaded fake backend for rdpsnd [20:38:14:582] [31843:00007c65] [INFO][com.freerdp.channels.drdynvc.client] - [dvcman_load_addin]: Loading Dynamic Virtual Channel ainput [20:38:14:582] [31843:00007c65] [INFO][com.freerdp.channels.drdynvc.client] - [dvcman_load_addin]: Loading Dynamic Virtual Channel rdpgfx [20:38:14:582] [31843:00007c65] [INFO][com.freerdp.channels.drdynvc.client] - [dvcman_load_addin]: Loading Dynamic Virtual Channel disp [20:38:14:582] [31843:00007c65] [INFO][com.freerdp.channels.drdynvc.client] - [dvcman_load_addin]: Loading Dynamic Virtual Channel rdpsnd [20:38:14:594] [31843:00007c74] [ERROR][com.freerdp.codec] - [openh264_init]: Failed to create OpenH264 decoder: 3 [20:38:14:594] [31843:00007c74] [WARN][com.freerdp.core.codecs] - [freerdp_client_codecs_prepare]: Failed to create h264 codec context [20:38:15:274] [31843:00007c74] [ERROR][com.freerdp.codec] - [openh264_init]: Failed to create OpenH264 decoder: 3 [20:38:15:274] [31843:00007c74] [ERROR][com.freerdp.gdi] - [gdi_SurfaceCommand_AVC444]: unable to create h264 context [20:38:15:274] [31843:00007c74] [ERROR][com.freerdp.channels.rdpgfx.client] - [logSurfaceCommand]: context->SurfaceCommand failed with error 8 [20:38:15:274] [31843:00007c74] [ERROR][com.freerdp.channels.rdpgfx.client] - [rdpgfx_decode]: rdpgfx_decode_AVC444 failed with error 8 [20:38:15:274] [31843:00007c74] [ERROR][com.freerdp.channels.rdpgfx.client] - [rdpgfx_recv_wire_to_surface_1_pdu]: rdpgfx_decode failed with error 8! [20:38:15:274] [31843:00007c74] [ERROR][com.freerdp.channels.rdpgfx.client] - [rdpgfx_recv_pdu]: rdpgfx_recv_wire_to_surface_1_pdu failed with error 8! [20:38:15:274] [31843:00007c74] [ERROR][com.freerdp.channels.rdpgfx.client] - [rdpgfx_recv_pdu]: Error while processing GFX cmdId: RDPGFX_CMDID_WIRETOSURFACE_1 (0x0001) [20:38:15:274] [31843:00007c74] [ERROR][com.freerdp.channels.rdpgfx.client] - [rdpgfx_on_data_received]: rdpgfx_recv_pdu failed with error 8! [20:38:15:274] [31843:00007c74] [ERROR][com.freerdp.channels.drdynvc.client] - [drdynvc_process_data]: ChannelId 1 not found!
```

https://gitlab.com/freedesktop-sdk/freedesktop-sdk/-/work_items/1081

https://github.com/FreeRDP/FreeRDP/issues/6383

## Reference

https://www.freerdp.com/
https://github.com/FreeRDP/FreeRDP
https://flathub.org/en/apps/com.freerdp.FreeRDP
