# VoIP

A Voice over Internet Protocol (VoIP) configuration setup for Asterisk, featuring SIP (Session Initiation Protocol) communication with voicemail capabilities.

## Overview

This project contains configuration files for an Asterisk-based VoIP system that enables voice communication over IP networks. It includes SIP endpoint definitions and dialplan configurations for managing calls and voicemail.

## Features

- **SIP Support**: Dual SIP endpoints (7001 and 7002) with dynamic host assignment
- **Call Routing**: Internal dialplan with extension-based call routing
- **Voicemail System**: Integrated voicemail for unavailable extensions
- **Voice Codec Support**: G.711 μ-law codec support for audio transmission
- **Security**: Authentication-based SIP registration with secret keys
- **NAT Traversal**: NAT support for endpoints behind firewalls
- **Local Network**: Preconfigured for private network (192.168.0.0/24)

## Project Structure

```
.
├── README.md           # This file
├── sip.conf           # SIP endpoint and general configuration
└── extensions.conf    # Dialplan and call routing logic
```

## Configuration Files

### sip.conf
Defines the SIP protocol settings and endpoint configuration:
- **General settings**: Port binding (5060), codec selection (G.711 μ-law), and network preferences
- **Endpoints**: Two SIP users (7001, 7002) with dynamic registration capability
- **Security**: Authentication required for all SIP registrations
- **Network**: Configured for 192.168.0.0/24 local network

### extensions.conf
Specifies the dialplan for call routing and handling:
- **Internal Context**: Routes calls between extensions 7001 and 7002
- **Call Flow**: Answer → Dial → Voicemail (if unavailable) → Hangup
- **Voicemail Access**: Extensions 8001 and 8002 for voicemail retrieval
- **Timeout**: 60-second dial timeout before voicemail kicks in

## Getting Started

### Prerequisites
- Asterisk VoIP server installed and running
- Network access to the Asterisk server
- SIP-compatible phones or soft clients (e.g., Linphone, MicroSIP, Jitsi)

### Installation

1. Locate your Asterisk configuration directory (typically `/etc/asterisk/`)
2. Copy or merge the configuration files:
   ```bash
   cp sip.conf /etc/asterisk/
   cp extensions.conf /etc/asterisk/
   ```

3. Reload Asterisk configuration:
   ```bash
   asterisk -rx "module reload"
   ```

## Usage

### Registering a SIP Phone

1. Configure your SIP client with:
   - **Server/Host**: IP address of your Asterisk server
   - **Username**: `7001` or `7002`
   - **Password**: `7001` or `7002` (respectively)
   - **Port**: `5060`

2. The phone will register as the extension number and be ready to make/receive calls

### Making Calls

- **Dial an extension**: Enter `7001` or `7002` to call another registered phone
- **Access voicemail**: Dial `8001` (for extension 7001) or `8002` (for extension 7002)

### Call Flow Example

1. User dials extension 7001
2. Asterisk answers the call
3. Asterisk dials the registered SIP phone for 7001 (60-second timeout)
4. If no answer, the "vm-nobodyavail" prompt plays
5. Call is transferred to voicemail for extension 7001
6. After voicemail is left, the call ends

## Configuration Notes

- **Static Network**: Local network is set to `192.168.0.0/255.255.255.0`
- **No Guest Calls**: Guest SIP calls are disabled (`allowguest=no`)
- **Authentication Required**: All SIP registrations must provide valid credentials
- **Codec**: Only G.711 μ-law (ulaw) is allowed for audio transmission

## Security Considerations

⚠️ **Important**: This configuration uses simple authentication. For production environments, consider:
- Using strong, randomly generated passwords instead of matching username/password
- Implementing IP-based access controls
- Using TLS/SRTP for encrypted communications
- Regular security audits of the Asterisk configuration
- Keeping Asterisk and related packages updated

## Troubleshooting

| Issue | Solution |
|-------|----------|
| SIP phones won't register | Check credentials, ensure port 5060 is accessible, verify network connectivity |
| No audio in calls | Ensure codecs match between endpoints, check NAT settings |
| Voicemail not recording | Verify voicemail spool directory exists, check Asterisk permissions |
| Calls drop | Review session timer settings, check network stability |

## Contributing

Feel free to submit issues and enhancement requests!

## License

This project is provided as-is. Ensure compliance with local regulations regarding VoIP usage.

## Resources

- [Asterisk Documentation](https://www.asterisk.org/docs/)
- [SIP Protocol Overview](https://tools.ietf.org/html/rfc3261)
- [Asterisk Configuration Guide](https://wiki.asterisk.org/wiki/display/AST/Configuration)

---

**Created**: March 15, 2025  
**Asterisk Configuration Project**
