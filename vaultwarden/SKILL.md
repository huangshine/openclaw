---
name: vaultwarden
description: Access vaultwarden passwords and secrets for automation. Use when needing credentials for servers, APIs, or other secured resources.
metadata:
  openclaw:
    requires:
      bins: ["bw"]
---

# Vaultwarden Skill

Access passwords and secrets stored in Vaultwarden for automation tasks.

## Setup

### 1. Install Bitwarden CLI
```bash
npm install -g @bitwarden/cli
```

### 2. Login to vaultwarden
```bash
bw login https://your-vaultwarden-server.com
# Enter email and master password
```

### 3. Unlock vault
```bash
bw unlock
# Enter master password to get BW_SESSION
```

## Usage

### Get password
```bash
bw get password "item-name" --session $BW_SESSION
```

### Get item details
```bash
bw get item "item-name" --session $BW_SESSION
```

### List items
```bash
bw list items --session $BW_SESSION
```

## Examples

### Server SSH connection
```bash
# Get SSH password
PASSWORD=$(bw get password "ssh-server" --session $BW_SESSION)
ssh user@server -p $PASSWORD
```

### API authentication
```bash
# Get API token
TOKEN=$(bw get password "api-service" --session $BW_SESSION)
curl -H "Authorization: Bearer $TOKEN" https://api.example.com/data
```

### Website login
```bash
# Get website credentials
USERNAME=$(bw get username "website" --session $BW_SESSION)
PASSWORD=$(bw get password "website" --session $BW_SESSION)
```

## Security Notes

- BW_SESSION expires after some time
- Store BW_SESSION in environment securely
- Never log passwords or session tokens
- Use environment variables for sensitive data

## Troubleshooting

### Session expired
If commands fail with authentication errors:
```bash
# Re-unlock vault
bw unlock
```

### Sync data
To get latest data from server:
```bash
bw sync --session $BW_SESSION
```

## Best Practices

1. **Always sync before operations**
   ```bash
   bw unlock && bw sync
   ```

2. **Use environment variables**
   ```bash
   export BW_SESSION=$(bw unlock --raw)
   ```

3. **Secure session storage**
   - Store BW_SESSION in secure environment
   - Avoid logging sensitive information
   - Use temporary session files if needed
