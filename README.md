# Layer Edge Auto Bot - Advanced Version
- Website: https://dashboard.layeredge.io/

## Features
- **Auto Run Node**
- **Support for Multiple Proxy Types**
- **Auto Claim Points Every Hour**
- **Auto Restart on Disconnection**
- **Persistent Operation After System Reboot**
- **Performance Monitoring**
- **Error Handling & Logging**

## Prerequisites
- Node.js 16+ installed on your machine
- PM2 for process management (will be installed in setup)

## Installation

1. Clone the repository:
    ```sh
    git clone https://github.com/airdropinsiders/LayerEdge-Auto-Bot.git
    cd LayerEdge-Auto-Bot
    ```

2. Install the required dependencies:
    ```sh
    npm install
    npm install pm2 -g
    ```

3. Configure your proxies (optional but recommended):
    ```sh
    nano proxy.txt
    ```
    Supported formats:
    ```
    http://username:password@ip:port
    socks5://username:password@ip:port
    http://ip:port
    socks5://ip:port
    ```

4. Configure wallet information:
    ```sh
    nano wallets.json
    ```
    Format:
    ```json
    [
      {
        "privateKey": "YOUR_PRIVATE_KEY",
        "proxyIndex": 0  // Use specific proxy from proxy.txt (0=first proxy)
      }
    ]
    ```
    
    If you don't want to use proxies:
    ```json
    [
      {
        "privateKey": "YOUR_PRIVATE_KEY",
        "proxyIndex": -1  // -1 means no proxy will be used
      }
    ]
    ```

5. Create auto-restart configuration:
    ```sh
    nano ecosystem.config.js
    ```
    Paste the following content:
    ```js
    module.exports = {
      apps: [{
        name: "layeredge-bot",
        script: "npm",
        args: "start",
        watch: false,
        max_memory_restart: "500M",
        env: {
          NODE_ENV: "production",
        },
        exp_backoff_restart_delay: 100,
        restart_delay: 3000,
        max_restarts: 10
      }]
    };
    ```

6. Create a startup script for persistence:
    ```sh
    nano start-bot.sh
    ```
    Paste the following content:
    ```sh
    #!/bin/bash
    cd "$(dirname "$0")"
    npm install
    pm2 start ecosystem.config.js
    ```
    
7. Make the script executable:
    ```sh
    chmod +x start-bot.sh
    ```

8. Set up automatic startup on boot:
    ```sh
    pm2 startup
    ```
    Follow the instructions that PM2 provides, then run:
    ```sh
    pm2 save
    ```

## Running the Bot

### For Normal Operation:
```sh
npm run start
```

### For Auto-Restart & Persistence:
```sh
./start-bot.sh
```
or
```sh
pm2 start ecosystem.config.js
```

### Additional PM2 Commands:
- View logs: `pm2 logs layeredge-bot`
- Monitor status: `pm2 monit`
- Stop bot: `pm2 stop layeredge-bot`
- Enable startup persistence: `pm2 startup` (follow prompts)
- Save current configuration: `pm2 save`

## Troubleshooting

If you encounter issues, check the logs:
```sh
pm2 logs layeredge-bot
```

Common solutions:
- Ensure Node.js version is 16 or higher
- Verify proxy format is correct
- Check wallet private keys
- Ensure proper internet connectivity

## Performance Optimization

For better performance:
- Use high-quality, low-latency proxies
- Run on a server with at least 2GB RAM
- Use wired internet connection when possible
- Consider distributing instances across multiple IPs

## License
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
This project is licensed under the [MIT License](LICENSE).
