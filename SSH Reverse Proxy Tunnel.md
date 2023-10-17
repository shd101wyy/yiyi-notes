1. SSH into the server:

  ```bash
  ssh -ND 9999 user@server
  ```
  - `-N`: Do not execute remote commands, just open the connection for forwarding
  - `D 9999` - Create a local "dynamic" forwarding port on your local machine, and route all traffic through it to the remote machine via SOCKS v4 or v5

2. Leave the terminal open, go to Firefox
3. Open firefox settings, search for "SOCKS" and open the Network Proxy settings button that is now highlighted
4. Select "Manual proxy configuration"
5. Enter `localhost`` for the SOCKS host, `9999` for the port, and SOCKS v5 selected. Click "OK" to save it.

- Leave the HTTP Proxy, SSL Proxy, and FTP proxy fields blank, with port 0 for each
- If you know what to fiddle with here, consider adjusting the "No proxy for" and "Proxy DNS when using SOCKS v5", if needed

