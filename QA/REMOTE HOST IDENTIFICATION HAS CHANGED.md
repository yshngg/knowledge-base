## Question

> WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!

```
$ ssh root@139.180.223.228

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ED25519 key sent by the remote host is
SHA256:PzOjMhDUksOFtkXe6keZDj0FI+i9YX1S7B62+lH9IHI.
Please contact your system administrator.
Add correct host key in /home/yshngg/.ssh/known_hosts to get rid of this message.
Offending ED25519 key in /home/yshngg/.ssh/known_hosts:33
Host key for 139.180.223.228 has changed and you have requested strict checking.
Host key verification failed.
```

## Answer

```bash
ssh-keygen -R 139.180.223.228
```

Ref:
https://stackoverflow.com/a/23150466
