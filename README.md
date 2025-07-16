# Ansible Project - Spotify Installation

This Ansible project is designed to download and install Spotify on Windows systems.

## Project Structure

```
ansible-project/
├── inventory/
│   ├── hosts                    # Inventory file with host definitions
│   └── group_vars/
│       └── windows_hosts.yml    # Variables for Windows hosts
├── playbooks/
│   └── install-spotify.yml      # Main playbook to install Spotify
├── roles/                       # Custom roles directory (empty for now)
├── ansible.cfg                  # Ansible configuration file
└── README.md                    # This file
```

## Quick Start Guide

### Step 1: Install Prerequisites
1. Install Ansible on your control machine
2. Ensure Python is installed on target Windows machines
3. Configure network access for downloading Spotify installer

### Step 2: Clone and Setup
1. Clone this repository to your local machine
2. Navigate to the project directory:
   ```bash
   cd ansible-project
   ```

### Step 3: Configure Target Hosts
1. Update `inventory/hosts` with your target Windows machines
2. Modify `inventory/group_vars/windows_hosts.yml` with appropriate credentials
3. For remote execution, configure WinRM on target machines (see Configuration section)

### Step 4: Test Connection
```bash
# Test local connection
ansible localhost -m ping

# Test Windows hosts (for remote execution)
ansible windows_hosts -m win_ping
```

### Step 5: Run Spotify Installation
```bash
# Run the playbook locally
ansible-playbook playbooks/install-spotify.yml

# Run on specific hosts
ansible-playbook -i inventory/hosts playbooks/install-spotify.yml -l windows_hosts
```

### Step 6: Verify Installation
Check that Spotify has been installed successfully on your target machines.

---

## Prerequisites

1. **Ansible installed** on your control machine
2. **Python** installed on target Windows machines
3. **WinRM configured** on target Windows machines (for remote execution)
4. **Network access** to download Spotify installer

## Configuration

### For Local Windows Execution

If running Ansible locally on a Windows machine:
1. Use the `localhost` target in the inventory
2. Ensure Python is available
3. Run with `ansible_connection=local`

### For Remote Windows Execution

1. Configure WinRM on target Windows machines:
   ```powershell
   # Run as Administrator
   winrm quickconfig
   winrm set winrm/config/service/auth '@{Basic="true"}'
   winrm set winrm/config/service '@{AllowUnencrypted="true"}'
   ```

2. Update the inventory file with correct credentials

## Usage

### Run the Spotify Installation Playbook

```bash
# Navigate to the project directory
cd ansible-project

# Run the playbook locally
ansible-playbook playbooks/install-spotify.yml

# Run on specific hosts
ansible-playbook -i inventory/hosts playbooks/install-spotify.yml -l windows_hosts

# Run with verbose output
ansible-playbook playbooks/install-spotify.yml -v
```

### Test Connectivity

```bash
# Test local connection
ansible localhost -m ping

# Test Windows hosts
ansible windows_hosts -m win_ping
```

## What the Playbook Does

1. **Creates temporary directory** for downloads
2. **Downloads Spotify installer** from official source
3. **Checks if Spotify is already installed**
4. **Installs Spotify silently** (if not already installed)
5. **Waits for installation to complete**
6. **Cleans up temporary files**
7. **Provides installation status**

## Customization

### Variables

You can customize the following variables in the playbook:

- `spotify_url`: URL to download Spotify (default: official Spotify installer)
- `download_path`: Where to temporarily store the installer
- `install_path`: Expected Spotify installation directory

### Alternative Download Sources

If you need to use a different download source, update the `spotify_url` variable in the playbook.

## Troubleshooting

### Common Issues

1. **Permission errors**: Run as Administrator or configure proper privileges
2. **Network issues**: Check firewall and internet connectivity
3. **WinRM errors**: Verify WinRM configuration on target machines
4. **Python not found**: Ensure Python is installed and in PATH

### Debug Mode

Run with increased verbosity:
```bash
ansible-playbook playbooks/install-spotify.yml -vvv
```

## Security Notes

- The playbook downloads from the official Spotify URL
- Always verify URLs before running
- Consider using internal mirrors for enterprise environments
- Review firewall and antivirus settings if downloads fail

## License

This project is for educational and automation purposes. Respect Spotify's terms of service when using their installer.
