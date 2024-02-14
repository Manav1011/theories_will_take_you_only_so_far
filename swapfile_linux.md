It looks like you have 2.0GB of swap space, and currently, 1.9GB of it is being used. If you find that your system frequently relies on swap space and you want to increase the swap size, you can follow the steps I provided earlier to create and enable a larger swap file.

Here's a quick summary of the steps:

1. **Create a Larger Swap File:**

   ```bash
   sudo fallocate -l 4G /new_swapfile
   sudo chmod 600 /new_swapfile
   sudo mkswap /new_swapfile
   sudo swapon /new_swapfile
   ```
2. **Make the Change Permanent:**

   Open `/etc/fstab`:

   ```bash
   sudo nano /etc/fstab
   ```

   Add the following line at the end:

   ```text
   /new_swapfile none swap sw 0 0
   ```

   Save and close the file.
3. **Verify the Changes:**

   ```bash
   swapon --show
   ```

   or

   ```bash
   free -h
   ```

   Ensure that the new swap space is available.

Please replace `/new_swapfile` with the actual path and name you want for the new swap file. The size in the `fallocate` command can be adjusted according to your needs (e.g., `8G` for 8GB).

Remember, while increasing swap space can help in certain situations, it's generally recommended to address the root cause of high memory usage if possible, such as by optimizing processes or adding more physical RAM to the system.
