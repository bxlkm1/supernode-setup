### Create a directory to work off of: `~/node/carrier`:
   ```bash
   mkdir -p ~/node/carrier;
   cd ~/node/carrier
   ```

### Download .deb package for "ela-bootstrapd" at [https://github.com/elastos/Elastos.NET.Carrier.Bootstrap/releases/download/release-v5.2.3/elastos-carrier-bootstrap-5.2.717741-linux-x86_64-Debug.deb](https://github.com/elastos/Elastos.NET.Carrier.Bootstrap/releases/download/release-v5.2.3/elastos-carrier-bootstrap-5.2.717741-linux-x86_64-Debug.deb) and copy to `~/node/carrier`
   ```bash
   wget https://github.com/elastos/Elastos.NET.Carrier.Bootstrap/releases/download/release-v5.2.3/elastos-carrier-bootstrap-5.2.717741-linux-x86_64-Debug.deb
   ```

### Install the .deb package locally and run the ela-bootstrapd service
   ```bash
   sudo dpkg -i elastos-carrier-bootstrap-5.2.717741-linux-x86_64-Debug.deb
   ```

### Modify ela-bootstrapd configuration file: `/etc/elastos/bootstrapd.conf`
- Update bootstrap nodes list under the section "bootstrap_nodes" if you need to(the default bootstrap nodes are already in the config file but you could add more if you want to)
- Set external IP to turn server explicitly: Some Linux VPS servers, for example, servers from AWS, can't fetch public IP address directly by itself, so you have manually update the public IP address of item external_ip under the section "turn"

### Check ela-bootstrapd node status
   ```bash
   sudo systemctl status ela-bootstrapd
   ```

   Should return something like:
   ```
   ● ela-bootstrapd.service - Elastos Carrier Bootstrap Daemon
      Loaded: loaded (/lib/systemd/system/ela-bootstrapd.service; enabled; vendor preset: enabled)
      Active: active (running) since Wed 2019-05-29 19:35:06 EDT; 36min ago
   Main PID: 729 (ela-bootstrapd)
      Tasks: 8 (limit: 3549)
      CGroup: /system.slice/ela-bootstrapd.service
            ├─729 /usr/bin/ela-bootstrapd --config=/etc/elastos/bootstrapd.conf
            └─738 /usr/bin/ela-bootstrapd --config=/etc/elastos/bootstrapd.conf

   May 29 19:35:06 kpxubuntu carrier-bootstrapd[738]: 0: IPv4. UDP listener opened on: 172.18.0.1:3478
   May 29 19:35:06 kpxubuntu carrier-bootstrapd[738]: 0: IPv4. UDP listener opened on: 172.18.0.1:3479
   May 29 19:35:06 kpxubuntu carrier-bootstrapd[738]: 0: IPv6. UDP listener opened on: ::1:3478
   May 29 19:35:06 kpxubuntu carrier-bootstrapd[738]: 0: IPv6. UDP listener opened on: ::1:3479
   May 29 19:35:06 kpxubuntu carrier-bootstrapd[738]: 0: Total General servers: 2
   May 29 19:35:06 kpxubuntu carrier-bootstrapd[738]: 0: IO method (admin thread): epoll (with changelist)
   May 29 19:35:06 kpxubuntu carrier-bootstrapd[738]: 0: IPv4. CLI listener opened on : 127.0.0.1:5766
   May 29 19:35:06 kpxubuntu carrier-bootstrapd[738]: 0: IO method (auth thread): epoll (with changelist)
   May 29 19:35:06 kpxubuntu carrier-bootstrapd[738]: 0: IO method (auth thread): epoll (with changelist)
   May 29 19:35:07 kpxubuntu carrier-bootstrapd[738]: 0: SQLite DB connection success: /var/lib/ela-bootstrapd/db/turndb
   ```