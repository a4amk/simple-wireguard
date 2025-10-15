# Simple WireGuard Manager

A powerful and easy-to-use Bash script for setting up and managing a WireGuard VPN exit node on Ubuntu. This project simplifies the deployment of a WireGuard server, client configuration, and ongoing management, including port changes and client additions/removals.

## Installation and Setup

To set up your WireGuard exit node, clone the repository and run the `main` script:

```bash
git clone https://github.com/a4amk/simple-wireguard.git
cd simple-wireguard
sudo ./main
```

The script will guide you through the following steps:

1.  **Server Endpoint**: Enter your server's public IP address or hostname.
2.  **WireGuard Port**: Choose a UDP port for WireGuard (default: `51820`).
3.  **IPv6 Routing (Optional)**: Decide whether to enable IPv6 for your VPN clients.
4.  **Key Generation**: Generate new server keys or provide an existing private key.
5.  **First Client Configuration (Optional)**: Generate an initial client configuration file or QR code immediately after setup.

## Features

*   **Automated Setup**: Guides you through the initial WireGuard server setup, including key generation, IP forwarding, and firewall configuration (UFW/Firewalld).
*   **Flexible Configuration**: Supports custom WireGuard ports and optional IPv6 routing.
*   **Client Management**: Easily add, remove, and list client peers with automatic IP assignment.
*   **Client Configuration Generation**: Generate client `.conf` files directly, display them in the terminal, or create QR codes for easy mobile import.
*   **Installation Modification**: Change the WireGuard port or server endpoint after initial setup.
*   **Uninstallation**: Provides a clean uninstallation process to revert the server to its pre-setup state.
*   **Backup System**: Automatically backs up critical system configuration files before making changes.

## Prerequisites

*   **Operating System**: Designed for Ubuntu.
*   **Root Access**: The script must be run as root or with `sudo`.
*   **Internet Connection**: Required for package installation (`wireguard`, `qrencode`, `ufw`).

## Usage

After successful installation, run the `main` script again to access the management menu:

```bash
sudo ./main
```

You will be presented with the following options:

1.  **Manage Clients (Add/Remove/List Peers)**: Accesses the `tools/client-manager` script to handle client-related tasks.
2.  **Modify Installation (Change Port or Endpoint)**: Allows you to update the WireGuard port and/or server endpoint.
3.  **Uninstall WireGuard (Cleanup and Restore)**: Completely removes WireGuard and restores system settings.
4.  **Exit**: Close the manager.

### Client Management

The `Manage Clients` option (or running `sudo ./tools/client-manager` directly) provides:

*   **Add New Client Peer**: Generates a new key pair, assigns internal IP addresses (IPv4 and optionally IPv6), adds the peer to the server configuration, and provides the client configuration in text, QR code, or file format.
*   **Remove Existing Client Peer**: Prompts for the client's public key to remove its configuration from the server.
*   **List Active Client Peers**: Displays public keys and assigned internal IPs of currently active clients.

### Modifying Installation

The `Modify Installation` option allows you to:

*   Change the WireGuard `ListenPort`. This will automatically update local firewall rules (UFW/Firewalld).
*   Update the public `Endpoint` (IP/Hostname) of your server. **Note**: Changing the endpoint requires you to generate new client configurations through the client manager, as existing client `.conf` files will not be automatically updated.

## Client Configuration

When adding a new client, you can choose how to receive its configuration:

1.  **Display in Terminal**: Shows the `.conf` content directly in your terminal. This is the most secure method as the private key is not saved to disk.
2.  **Generate QR Code**: Requires `qrencode` to be installed. Ideal for importing configurations to mobile WireGuard clients.
3.  **Save to Server Disk**: Saves the `.conf` file to the `wg-clients/` directory. **WARNING**: This is the least secure option. You **MUST** transfer the file to your client device and **DELETE** it from the server immediately after transfer.

## Security Considerations

*   **Cloud Provider Firewall**: If your server is hosted with a cloud provider (e.g., AWS, Oracle Cloud), you **MUST** manually open the chosen UDP port (`51820` by default) in your cloud security group or network access control list. The script cannot configure external cloud firewalls.
*   **Client Configuration Files**: Client `.conf` files contain sensitive private keys. If you choose to save them to disk, ensure they are deleted from the server after secure transfer to prevent unauthorized access.
*   **Root Access**: The scripts require root privileges. Always be cautious when running scripts as root.

## Contributing

This project is open-source under the MIT License. Contributions are welcome! If you find a bug or have a feature request, please open an issue or submit a pull request on the project's GitHub repository.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.