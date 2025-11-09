# ZBackup
ZBackup is a comprehensive backup and restoration system for Discord servers. It can backup and restore all server settings, permissions, roles, channels, emojis, stickers, bans, and even messages with attachments and embeds.
> 💡 **Built for the Zygnal Ecosystem — to download and use this extension, you must be part of the Zygnal Ecosystem.**  
> This extension (cog) is part of the **Zygnal Ecosystem** and is only available through its supported platforms.  
> You can use it with:  
> - The **[Discord Bot Framework](https://github.com/TheHolyOneZ/discord-bot-framework)** — ideal for developers who want full control and flexibility *(includes an integrated extension marketplace)*, or  
> - The **[ZygnalBot](https://zygnalbot.de)** — a prebuilt, plug-and-play Discord bot *(also includes an integrated extension marketplace)*.  
>
> Browse and install extensions at [zygnalbot.com/extension](https://zygnalbot.com/extension).  
> For help or community discussions, join us on Discord: [discord.gg/sgZnXca5ts](https://discord.gg/sgZnXca5ts)
# ZBackup - Advanced Discord Server Backup Extension

ZBackup is a comprehensive backup and restoration system for Discord servers. It can backup and restore all server settings, permissions, roles, channels, emojis, stickers, bans, and even messages with attachments and embeds.

## Features

- **Complete Server Backup**: Backup all aspects of your Discord server including:
  - Server settings (name, description, icon, banner, verification level, etc.)
  - Roles with permissions and colors
  - Channels with permission overwrites (text, voice, categories, forums, stage channels)
  - Emojis and stickers
  - Ban lists
  - Messages with attachments and embeds
  
- **Selective Restoration**: Choose which components to restore:
  - Restore only specific elements (roles, channels, emojis, etc.)
  - Keep your current server structure while restoring only what you need
  
- **Message Backup**: Preserve your server's history:
  - Backup messages with their original author information
  - Include attachments, embeds, and reactions
  - Control how many messages to backup per channel
  
- **Scheduled Backups**: Set up automatic protection:
  - Configure backup intervals (hourly, daily, weekly)
  - Set retention policies to manage disk space
  - Automatic cleanup of old backups
  
- **Backup Management**: Complete control over your backups:
  - List all available backups with detailed information
  - View backup contents before restoration
  - Delete unwanted backups
  - Export backups to files for external storage
  - Import backups from files

## Commands

**All Commands are Hybrid Commands!**

**And If it says [something] Its Optional**

### Basic Commands 

- `/zbackup create [include_messages] [message_limit]` - Create a new backup
  - `include_messages` (optional): Whether to include messages (default: True)
  - `message_limit` (optional): Maximum number of messages to backup per channel (default: 100, max: 1000)

- `/zbackup list` - List all available backups with pagination

- `/zbackup info <backup_id>` - Show detailed information about a backup
  - `backup_id`: The ID of the backup (timestamp format)

- `/zbackup delete <backup_id>` - Delete a backup
  - `backup_id`: The ID of the backup to delete

- `/zbackup restore <backup_id> [options]` - Restore a backup
  - `backup_id`: The ID of the backup to restore
  - Options (all default to True):
    - `roles`: Whether to restore roles
    - `channels`: Whether to restore channels
    - `emojis`: Whether to restore emojis
    - `stickers`: Whether to restore stickers
    - `bans`: Whether to restore bans
    - `messages`: Whether to restore messages
    - `server_settings`: Whether to restore server settings

### Advanced Commands

- `/zbackup export <backup_id>` - Export a backup to a downloadable file
  - `backup_id`: The ID of the backup to export

- `/zbackup import` - Import a backup from a file
  - After running this command, upload a backup zip file

- `/zbackup schedule [interval] [include_messages] [message_limit] [keep_count]` - Schedule automatic backups
  - `interval` (optional): Backup interval in hours, 0 to disable (default: 24)
  - `include_messages` (optional): Whether to include messages (default: True)
  - `message_limit` (optional): Maximum messages per channel (default: 100)
  - `keep_count` (optional): Number of backups to keep, 0 for unlimited (default: 5)

## Installation

1. Place the `zbackup.py` file in your bot's `Extensions` directory
2. Load the cog in your bot using one of these methods:
   - Using command: `!load zbackup`
   

## Requirements

- Python 3.10+
- discord.py 2.0+
- Administrator permissions for both the bot and the user running the commands
- Sufficient disk space for storing backups (especially with message attachments)

## How It Works

### Backup Process

1. **Server Settings**: Backs up basic server information (name, icon, verification level, etc.)
2. **Roles**: Captures all roles with their permissions, colors, and positions
3. **Channels**: Saves all channels with their settings and permission overwrites
4. **Emojis & Stickers**: Stores custom emojis and stickers with their images
5. **Bans**: Records all banned users and ban reasons
6. **Messages**: If enabled, saves messages with author information, content, attachments, and embeds

### Restoration Process

1. **Confirmation**: Requires explicit confirmation before proceeding
2. **Clearing**: Optionally clears existing server elements (roles, channels, etc.)
3. **Restoration**: Recreates server elements in the correct order:
   - First roles (to establish permissions)
   - Then categories and channels with proper hierarchy
   - Finally emojis, stickers, and messages
4. **Messages**: Uses webhooks to restore messages with original author names and avatars

### Storage System

- Backups are stored in a `backups` directory organized by server ID
- Each backup includes:
  - JSON data file with server structure
  - Separate directories for attachments, emojis, and stickers
  - Metadata file with summary information

## Usage Examples

### Creating a Basic Backup

```
/zbackup create
```

This creates a complete backup including messages (up to 100 per channel).

### Creating a Backup Without Messages

```
/zbackup create include_messages=False
```

This creates a backup of server structure only, without messages.

### Creating a Backup With More Messages

```
/zbackup create message_limit=500
```

This creates a backup including up to 500 messages per channel.

### Restoring Only Channels and Roles

```
/zbackup restore 20230101_120000 channels=True roles=True emojis=False stickers=False bans=False messages=False server_settings=False
```

This restores only the channels and roles from the backup, leaving other elements untouched.

### Setting Up Daily Backups

```
/zbackup schedule interval=24 include_messages=True message_limit=100 keep_count=5
```

This schedules automatic backups every 24 hours, keeping the 5 most recent backups.

### Setting Up Weekly Backups Without Messages

```
/zbackup schedule interval=168 include_messages=False keep_count=10
```

This schedules weekly backups (168 hours) without messages, keeping the 10 most recent backups.

## Best Practices

1. **Regular Backups**: Schedule automatic backups to protect against accidental changes or raids
2. **External Storage**: Export important backups and store them outside Discord
3. **Selective Restoration**: Use the selective restoration options to avoid overwriting wanted changes
4. **Message Limits**: For large servers, consider limiting the number of messages backed up
5. **Test Restoration**: Test the restoration process on a test server before using in production

## Troubleshooting

If you encounter any issues:

1. **Check Logs**: Examine the `zbackup.log` file for detailed error information
2. **Permission Issues**: Ensure the bot has Administrator permissions
3. **Role Hierarchy**: The bot cannot modify roles higher than its own role
4. **Rate Limits**: For large servers, restoration might hit Discord rate limits; wait and try again
5. **Disk Space**: Ensure sufficient disk space for backups with attachments
6. **Partial Restoration**: If full restoration fails, try restoring components selectively

7. **If you're still having issues, please open an Ticket in the Support Server**

## Common Errors and Solutions

- **"Unknown Channel" errors**: These can occur during restoration if the channel where the command was run gets deleted. The bot will try to use the system channel as a fallback.
- **"Invalid Role" errors**: These are normal when trying to delete roles that don't exist or are managed by integrations.
- **"Missing Permissions" errors**: Ensure the bot's role is higher than the roles it's trying to modify.
- **Webhook errors**: For message restoration, ensure the bot can create and manage webhooks.

## License

This extension is provided under the MIT License. Feel free to modify and distribute it as needed.

## Credits

Developed by TheHolyOneZ
