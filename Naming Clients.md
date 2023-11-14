## Setting up Custom Snapcast Client Names 🎉

After successfully setting up Snapcast clients, they automatically connect to the server with default names assigned by the server. However, you might want to personalize these names to easily identify each client. Follow the steps below to set custom names for Snapcast clients:

1. **Stop Snapcast Server:**

    Before making changes, ensure that the Snapcast Server is stopped to prevent any conflicts with the new configurations.

    ```bash
    sudo service snapserver stop
    ```

2. **Edit the Snapcast Server Configuration File:**

    Use a text editor to open the Snapcast Server configuration file.

    ```bash
    sudo vi /var/lib/snapcast/server.json
    ```

3. **Modify Client Names:**

    In the JSON file, locate the path `config` -> `name` and change the names of each client to your desired custom names.

    Example:
    ```json
    "config": {
        "name": "🏠-living-room-server",
        "stream": {
            ...
        },
        "groups": [
            ...
        ]
    }
    ```

4. **Save the Changes:**

    Save the changes to the configuration file.

5. **Start Snapcast Server:**

    Restart the Snapcast Server to apply the new configurations.

    ```bash
    sudo service snapserver start
    ```

    The clients will automatically reconnect. 🔄
