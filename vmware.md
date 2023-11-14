## 🎧 Setup Virtual Sound Card on ESXi

If you're running Ubuntu Server as a host on VMware (ESXi 6.5) and need a virtual sound card, follow these steps. Please note that starting from ESXi 6.0, the virtual sound card option is no longer available in the configuration settings. Instead, you can enable it via SSH and modify the corresponding host's `.vmx` file.

### 1. **Enable SSH:**
   
   If SSH is not enabled, follow these steps:
   
   - Click on the ESXi server name in the vSphere Client.
   - Go to the "Configuration" tab.
   - Click on the "Security Profile" link on the left navigation menu.
   - Click on the "Properties" link on the right side of the Security Profile -> Services section.
   - Select SSH in the dialog box and start the SSH Server.

### 2. **SSH into ESXi:**

   SSH into your ESXi host.

### 3. **Find VMX Files:**

   Run the following command to find all `.vmx` files:

   ```bash
   find /vmfs/volumes/ -type f -name "*.vmx"
   ```

### 4. **Edit VMX File:**

   - Before making any changes, properly shut down the server.
   - Take a backup of the `.vmx` file.

   Edit the `.vmx` file using `vi` or your preferred editor:

   ```bash
   vi /vmfs/volumes/YOUR_HOST_GUID/YOUR_VM_NAME/YOUR_VM_NAME.vmx
   ```

### 5. **Add Virtual Sound Card Config:**

   Add the following lines to enable the virtual sound card:

   ```bash
   sound.present = "true"
   sound.allowGuestConnectionControl = "false"
   sound.virtualDev = "hdaudio"
   sound.fileName = "-1"
   sound.autodetect = "true"
   ```

   Save the changes and exit the editor.

### 6. **Restart Host:**

   Restart the ESXi host.

### 7. **Verify Configuration:**

   After the restart, check the host configuration settings, and you should see `hdaudio` as the virtual sound card.

[Back to home page](https://github.com/footcricket05/EchoLink)
