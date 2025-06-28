```markdown
# Discord Permissions Configuration Guide

This README outlines how to configure Discord permissions using a structured JSON file. It provides examples for bot developers and server administrators to apply permissions programmatically or manually.

---

## 📁 JSON Permissions Structure

Permissions are stored as a JSON array, where each object includes:

- `code`: The Discord permission constant used in development (e.g., `ADMINISTRATOR`)
- `description`: A brief explanation of what the permission allows

### Example:

```json
{
  "code": "MANAGE_CHANNELS",
  "description": "Allows creating, editing, and deleting channels."
}
```

You can find the full list [here](./permissions.json)

---

## ⚙️ 1. Using Permissions in Discord OAuth2 Bot URLs

To invite a bot with specific permissions:

1. Go to [Discord Developer Portal](https://discord.com/developers/applications)
2. Open your application → “OAuth2” → “URL Generator”
3. Select:
   - `bot` scope
   - Desired permissions (or calculate the bitwise sum manually)
4. Copy the generated link

### Example:

```
https://discord.com/oauth2/authorize?client_id=YOUR_BOT_ID&scope=bot&permissions=164497195519
```

You can calculate `permissions` as a bitfield sum of required flags using a script or site like [Discord Permissions Calculator](https://discordapi.com/permissions.html)

---

## 💻 2. Applying Permissions with Discord.js (v14+)

### a. Creating a New Role with Full Permissions:

```js
await guild.roles.create({
  name: 'Admin',
  permissions: ['ADMINISTRATOR'],
  reason: 'Create an admin role with all permissions'
});
```

### b. Creating a Custom Role from JSON:

```js
const permissionsData = require('./permissions.json');

const moderatorPermissions = permissionsData
  .map(p => p.code)
  .filter(code =>
    [
      'KICK_MEMBERS',
      'BAN_MEMBERS',
      'MANAGE_MESSAGES',
      'MODERATE_MEMBERS'
    ].includes(code)
  );

await guild.roles.create({
  name: 'Moderator',
  permissions: moderatorPermissions,
  reason: 'Moderator role with scoped permissions'
});
```

---

## 🔄 3. Automating Role Setup from JSON

You can create a CLI tool or script (e.g., using Node.js, Python, or PowerShell) that:

1. Reads `permissions.json`
2. Filters required permissions
3. Applies them to a new or existing role

This approach supports modular, scalable server setups, ideal for enterprise-style environments.

---

## 🛡️ Notes

- `ADMINISTRATOR` overrides all other permission flags
- Permission order matters only for readability, not functionality
- Always test in a development server before deploying to production
- For bots, remember to check if the bot's highest role is above the one it's assigning permissions to

---

## 📎 Resources

- [Discord Developer Portal](https://discord.com/developers/applications)
- [Discord.js Docs](https://discord.js.org/#/docs/)
- [Discord Permissions Bitfield Reference](https://discord.com/developers/docs/topics/permissions)

---

## © 2025 • Created by Alpha.
```
I wish you good use, I hope I could help. I would be very happy if you give me a star <3
