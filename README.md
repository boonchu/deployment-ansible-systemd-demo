This repo contains the material used in the article "Deploying a service using ansible and systemd" ([original link](https://kkentzo.github.io/2020/03/25/deploying-with-ansible-systemd/), [post to dev.to](https://dev.to/kkentzo/deploying-a-service-using-ansible-and-systemd-4n11)).

  * Make binary.

```
$ gmake
env GOOS=linux go build -o ./bin/demo ./cmd/demo/...
```
 
  * Validate from folders.

```
$ tree cmd/ bin/
cmd/
└── demo
    └── main.go
bin/
└── demo
```

  * Deploy new systemd service.

```
$ gmake deploy
env GOOS=linux go build -o ./bin/demo ./cmd/demo/...
cp ./bin/demo ./roles/demo/files/demo
ansible-playbook -i ./hosts playbook.yml

PLAY [myservers] ***************************************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************************************
ok: [cow1]

TASK [create demo group] *******************************************************************************************************************
changed: [cow1]

TASK [create demo user] ********************************************************************************************************************
changed: [cow1]

TASK [demo : Copy systemd service file to server] ******************************************************************************************
changed: [cow1]

TASK [demo : Copy binary to server] ********************************************************************************************************
changed: [cow1]

RUNNING HANDLER [Start demo] ***************************************************************************************************************
changed: [cow1]

PLAY RECAP *********************************************************************************************************************************
cow1                       : ok=6    changed=5    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
  
  * output from systemctl status.

```
[root@cow1 bigchoo]# systemctl status demo
● demo.service - Demo service
   Loaded: loaded (/etc/systemd/system/demo.service; enabled; vendor preset: disabled)
   Active: active (running) since Sat 2020-10-10 16:20:18 +07; 2min 28s ago
 Main PID: 26932 (demo)
    Tasks: 4 (limit: 11332)
   Memory: 4.3M
   CGroup: /system.slice/demo.service
           └─26932 /usr/local/bin/demo

Oct 10 16:20:18 cow1 systemd[1]: Started Demo service.
```
