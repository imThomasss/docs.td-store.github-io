# TD-Store Documentation

<div align="center">
  <img src="https://img.shields.io/badge/FiveM-Store%20System-blue?style=for-the-badge&logo=fivem" alt="TD-Store">
  <img src="https://img.shields.io/badge/Version-1.0.0-green?style=for-the-badge" alt="Version">
  <img src="https://img.shields.io/badge/License-Premium-gold?style=for-the-badge" alt="License">
</div>

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Installation](#installation)
- [Configuration](#configuration)
- [API Reference](#api-reference)
- [Troubleshooting](#troubleshooting)
- [Support](#support)

## 🎯 Overview

TD-Store is a premium FiveM store system that provides a modern, feature-rich in-game store experience with Tebex integration, vehicle garage management, and a comprehensive credit system. Built with security and performance in mind, it offers both players and administrators powerful tools for managing virtual economies.

### Key Highlights

- 🚀 **Modern Architecture** - Built with Lua 5.4 and modern FiveM standards
- 🔒 **Enterprise Security** - Anti-cheat protection and ownership validation
- 💳 **Tebex Integration** - Seamless payment processing and webhook handling
- 🎨 **Beautiful UI** - Responsive, modern interface with dark theme
- 📊 **Comprehensive Logging** - Discord webhook integration for monitoring
- ⚡ **High Performance** - Optimized database queries and efficient resource usage

## ✨ Features

### 🛒 Store System
- **Vehicle Store** - Browse, preview, and purchase vehicles with real-time previews
- **Credit Packages** - Tebex-integrated credit purchasing system
- **Item Management** - Add/edit store items through manager interface
- **Purchase History** - Complete transaction logging and history

### 🏠 Garage Management
- **My Garage** - Personal vehicle collection management
- **Vehicle Spawning** - Spawn owned vehicles with protection systems
- **Ownership Validation** - Secure vehicle access control
- **Spawn Protection** - Temporary invincibility after spawning

### 👨‍💼 Administration Tools
- **Admin Panel** - Player management and refund processing
- **Manager Tools** - Store item and credit package management
- **Player Commands** - Credit management and vehicle administration
- **Security Monitoring** - Unauthorized access detection and logging

### 🔧 Technical Features
- **Database Integration** - MySQL with oxmysql support
- **Webhook System** - Real-time Tebex payment processing
- **Custom Notifications** - Support for various notification systems
- **Permission System** - ACE-based permission management
- **Anti-Cheat Protection** - Multiple security layers

## 🚀 Installation

### Prerequisites

- FiveM Server
- MySQL Database
- oxmysql Resource
- Tebex Account (for payment processing)

### Step 1: Download and Install

1. Download TD-Store from your purchase source
2. Extract the files to your server's `resources` folder
3. Ensure oxmysql is installed and loaded **before** TD-Store

### Step 2: Database Setup

1. Import the `database.sql` file into your MySQL database
2. This creates all necessary tables for the store system
3. Verify the tables were created successfully

### Step 3: Resource Configuration

Add TD-Store to your `server.cfg`:

```cfg
# Ensure oxmysql loads first
ensure oxmysql
ensure td-store
```

### Step 4: Permissions Setup

Add these ACE permissions to your `server.cfg`:

```cfg
# Admin permissions
add_ace group.admin tdstore.admin allow

# Manager permissions  
add_ace group.manager tdstore.manager allow
```

## ⚙️ Configuration

### Server Configuration (`s_config.lua`)

The server configuration contains sensitive information and should be configured carefully:

#### Tebex Integration
```lua
SConfig.Tebex = {
    private_key = "YOUR_TEBEX_PRIVATE_KEY",
    public_token = "YOUR_TEBEX_PUBLIC_TOKEN", 
    project_id = "YOUR_TEBEX_PROJECT_ID",
    webhook_secret = "YOUR_WEBHOOK_SECRET",
    webhook = {
        url = "http://your-server-ip:30120/webhook"
    }
}
```

#### Security Settings
```lua
SConfig.Security = {
    validateOwnershipOnEnter = true,
    allowPassengers = true,
    ejectUnauthorizedDrivers = true,
    maxVehiclesPerPlayer = 10,
    cooldownBetweenSpawns = 3,
    enableAntiCheat = true
}
```

#### Discord Webhook
```lua
SConfig.Discord = {
    enabled = true,
    webhookUrl = "YOUR_DISCORD_WEBHOOK_URL",
    serverName = "Your Server Name",
    logPurchases = true,
    logCreditPurchases = true,
    logUnauthorizedAccess = true
}
```

### Client Configuration (`c_config.lua`)

The client configuration contains non-sensitive settings:

#### General Settings
```lua
CConfig.Command = 'store'        -- Command to open store
CConfig.Keybind = 'F7'           -- Keybind to open store
CConfig.EnableKeybind = true     -- Enable/disable keybind
```

#### Vehicle Preview
```lua
CConfig.VehiclePreview = {
    dealershipCoords = vector3(-56.79, -1096.63, 25.42),
    vehicleSpawnCoords = vector3(-50.0, -1100.0, 25.5),
    cameraCoords = vector3(-45.0, -1095.0, 27.0),
    cameraRotation = vector3(-10.0, 0.0, 45.0)
}
```

## 📚 API Reference

### Admin Commands

| Command | Permission | Description | Usage |
|---------|------------|-------------|-------|
| `/givestorecredits` | `tdstore.admin` | Give credits to player | `/givestorecredits [id] [amount]` |
| `/removestorecredits` | `tdstore.admin` | Remove credits from player | `/removestorecredits [id] [amount]` |
| `/resetstoreplayer` | `tdstore.admin` | Reset player data | `/resetstoreplayer [id]` |
| `/viewstoreplayer` | `tdstore.admin` | View player information | `/viewstoreplayer [id]` |
| `/givestorevehicle` | `tdstore.admin` | Give vehicle to player | `/givestorevehicle [id] [vehicle_id]` |
| `/removestorevehicle` | `tdstore.admin` | Remove vehicle from player | `/removestorevehicle [id] [vehicle_id]` |
| `/refundstorepurchase` | `tdstore.admin` | Refund a purchase | `/refundstorepurchase [player_id] [purchase_id]` |

### Manager Commands

| Permission | Description |
|------------|-------------|
| `tdstore.manager` | Access to store management interface for adding/editing items and credit packages |

### Events

#### Client Events
- `td-store:openStore` - Opens the store interface
- `td-store:closeStore` - Closes the store interface
- `td-store:customNotify` - Custom notification event

#### Server Events
- `td-store:purchaseItem` - Handles item purchases
- `td-store:spawnVehicle` - Handles vehicle spawning
- `td-store:webhook` - Handles Tebex webhook processing

### Database Tables

| Table | Description |
|-------|-------------|
| `td_store_players` | Player credit balances and data |
| `td_store_purchases` | Purchase history and transactions |
| `td_store_ownership` | Vehicle ownership records |
| `td_store_items` | Store items and configurations |

## 🔧 Troubleshooting

### Common Issues

#### Webhook Not Working
- **Problem**: Tebex webhooks not processing payments
- **Solution**: 
  - Ensure server is running on port 30120
  - Verify webhook URL is accessible from internet
  - Check webhook secret matches configuration
  - Restart server after webhook setup

#### Database Connection Issues
- **Problem**: Database errors or connection failures
- **Solution**:
  - Verify oxmysql is loaded before TD-Store
  - Check database credentials in oxmysql config
  - Ensure database tables were created properly

#### Permission Errors
- **Problem**: Admin commands not working
- **Solution**:
  - Verify ACE permissions are set correctly
  - Check player has proper group assignment
  - Restart server after permission changes

#### Vehicle Spawning Issues
- **Problem**: Vehicles not spawning or disappearing
- **Solution**:
  - Check spawn coordinates in config
  - Verify vehicle model names are correct
  - Ensure spawn protection settings are appropriate

### Debug Mode

Enable debug mode in `s_config.lua`:
```lua
SConfig.Debug = true
```

This will provide detailed console output for troubleshooting.

### Log Files

Check server console and F8 console for error messages. Discord webhook logs can help identify issues with transactions and security events.

## 📞 Support

### Getting Help

- **Discord Support**: [Join our Discord](https://discord.gg/tWwfPnxz47)
- **Documentation**: This documentation covers most common scenarios
- **Issues**: Report bugs and issues through Discord

### Before Requesting Support

1. Check this documentation thoroughly
2. Enable debug mode and check console output
3. Verify your configuration matches the examples
4. Test with minimal configuration first

### Support Information

When requesting support, please provide:
- Server console errors
- Configuration files (remove sensitive data)
- Steps to reproduce the issue
- Server information (FiveM version, resources)

---

<div align="center">
  <p><strong>TD-Store</strong> - Premium FiveM Store System</p>
  <p>Built with ❤️ for the FiveM community</p>
</div>
