## 🎵 Enhance AirPlay for Your Multi-room Audio System

Boost your audio experience by enabling AirPlay on your multi-room audio setup with `shairport-sync` ([GitHub](https://github.com/mikebrady/shairport-sync)). Follow these simple steps to integrate AirPlay seamlessly:

### 1. **Install `shairport-sync`:**

Run the command to install the `shairport-sync` libraries:

```bash
sudo apt-get install shairport-sync
```

### 2. **Configure `shairport-sync.conf`:**

Ensure `shairport-sync` runs as a daemon after installation. Run these commands:

```bash
sudo systemctl start shairport-sync
sudo systemctl enable shairport-sync
sudo systemctl status shairport-sync
```

### 3. **Modify Configuration Settings:**

Edit the `shairport-sync.conf` file located in `/etc`:

```bash
sudo vi /etc/shairport-sync.conf
```

Update the configuration settings based on your preferences. For example, change the device name:

```bash
general =
{
        name = "Multi Room Audio";
        output_backend = "pipe";
};
```

### 4. **Restart `shairport-sync`:**

Apply the changes by restarting the `shairport-sync` service:

```bash
sudo systemctl restart shairport-sync
```

### 5. **Enjoy AirPlay:**

Now, you can stream content to your AirPlay-supported devices effortlessly.

### 🛠️ Troubleshooting:

Check the service status to ensure there are no errors:

```bash
sudo systemctl status shairport-sync
```

### 📚 Additional Reading:

- [Snapcast](https://github.com/badaix/snapcast)
- [Airplay Setup in Snapcast](https://github.com/badaix/snapcast/blob/master/doc/player_setup.md#airplay)
- [shairport-sync](https://github.com/mikebrady/shairport-sync)

[Back to home page](https://github.com/footcricket05/EchoLink)
