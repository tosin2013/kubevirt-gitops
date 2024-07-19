# SSH Access Instructions

To access the RHEL9 NFS server via SSH, follow these steps:

1. **Get the IP Address:**
   ```sh
   kubectl get svc ssh-access -o jsonpath='{.spec.clusterIP}'
   ```

2. **SSH into the server:**
   ```sh
   ssh username@<cluster-ip>
   ```

3. **Copy files to the NFS server:**
   ```sh
   scp /path/to/local/file username@<cluster-ip>:/path/to/remote/directory
   ```

Replace `username` with the appropriate username for the NFS server.
