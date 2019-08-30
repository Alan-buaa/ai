# daemon config

### 配置更新问题

https://docs.docker.com/engine/reference/commandline/dockerd/#daemon-configuration-file

部分配置支持热更新

1. 修改/etc/docker/daemon.json
2. 利用sudo kill -SIGHUP $(pidof dockerd)触发daemon配置更新

```shell
[root@YZ-25-58-1 supdev]# journalctl -f -u docker.service   
-- Logs begin at Mon 2019-08-19 02:34:34 CST. --
Aug 21 16:02:40 YZ-25-58-1.h.chinabank.com.cn dockerd[2894702]: time="2019-08-21T16:02:40.158669220+08:00" level=info msg="Got signal to reload configuration, reloading from: /etc/docker/daemon.json"
Aug 21 16:02:40 YZ-25-58-1.h.chinabank.com.cn dockerd[2894702]: time="2019-08-21T16:02:40.158945480+08:00" level=warning msg="The \"graph\" config file option is deprecated. Please use \"data-root\" instead."
Aug 21 16:02:40 YZ-25-58-1.h.chinabank.com.cn dockerd[2894702]: time="2019-08-21T16:02:40.159099717+08:00" level=info msg="Reloaded configuration: {\"exec-opts\":[\"native.cgroupdriver=systemd\"],\"mtu\":1500,\"pidfile\":\"/var/run/docker.pid\",\"graph\":\"/export/App/docker\",\"data-root\":\"/export/App/docker\",\"exec-root\":\"/var/run/docker\",\"group\":\"docker\",\"deprecated-key-path\":\"/etc/docker/key.json\",\"max-concurrent-downloads\":3,\"max-concurrent-uploads\":5,\"shutdown-timeout\":15,\"hosts\":[\"fd://\"],\"log-level\":\"info\",\"swarm-default-advertise-addr\":\"\",\"swarm-raft-heartbeat-tick\":0,\"swarm-raft-election-tick\":0,\"metrics-addr\":\"\",\"log-driver\":\"json-file\",\"ip\":\"0.0.0.0\",\"icc\":true,\"iptables\":true,\"ip-forward\":true,\"ip-masq\":true,\"userland-proxy\":true,\"default-address-pools\":{},\"network-control-plane-mtu\":1500,\"insecure-registries\":[\"idockerhub.jd.com\",\"dockerhub.jd.com\",\"172.25.58.1:8000\",\"172.25.58.1:8089\"],\"disable-legacy-registry\":true,\"experimental\":false,\"containerd\":\"/run/containerd/containerd.sock\",\"builder\":{\"GC\":{}},\"runtimes\":{\"runc\":{\"path\":\"runc\"}},\"default-runtime\":\"runc\",\"oom-score-adjust\":-500,\"default-shm-size\":67108864,\"default-ipc-mode\":\"shareable\",\"resolv-conf\":\"/etc/resolv.conf\"}"
```

日志中可以看到-SIGHUP信号触发了daemon配置更新

