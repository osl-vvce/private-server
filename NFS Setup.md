# Network File System (NFS) Setup

It is a popular distributed filesystem protocol that enables users to mount remote directories on their server. NFS lets you leverage storage space in a different location and allows you to write onto the same space from multiple servers or clients in an effortless manner. It, thus, works fairly well for directories that users need to access frequently.

## Steps

- Installing NFS at server end
- Installing NFS client end

## Installing NFS at server end

1. Install required packages on the CentOS server

   - yum install nfs-utils

2. Create the directory(If needed, else you can use any directory) that will be shared by NFS and change the permissions of the folder

```
  mkdir /var/nfsshare
  chmod -R 755 /var/nfsshare
  chown nfsnobody:nfsnobody /var/nfsshare
```

3. Now restart the following services and enable them

   - systemctl enable rpcbind
   - systemctl enable nfs-server
   - systemctl enable nfs-lock
   - systemctl enable nfs-idmap
   - systemctl start rpcbind
   - systemctl start nfs-server
   - systemctl start nfs-lock
   - systemctl start nfs-idmap

4. Add the sharing points by editing the exports file `nano /etc/exports`

   Add the following line to make nfsshare as sharing point :

```
/var/nfsshare    <client-ip>(rw,sync,no_root_squash,no_all_squash)
```

5. Finally, start the NFS service

   - systemctl restart nfs-server

## Installing NFS client end

1. Install required packages on the CentOS server

   - yum install nfs-utils

2. Now create the NFS directory mount points

   - mkdir -p /mnt/nfs/var/nfsshare

3. Next, we will mount the NFS shared home directory in the client machine

   - mount -t nfs server-ip:/var/nfsshare /mnt/nfs/var/nfsshare/

- Check whether it is connected with the NFS share using `df -kh`

4. Permanently mount it by adding the NFS-share in /etc/fstab file on client machine `nano /etc/fstab`
   - server-ip:/var/nfsshare /mnt/nfs/var/nfsshare nfs defaults 0 0
