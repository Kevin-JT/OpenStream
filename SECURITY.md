# Security

## Principles

1. **No hardcoded secrets** - Credentials, keys, tokens must never be embedded in source
2. **Platform security** - Follow Android/iOS security best practices
3. **Data privacy** - User data handled according to permissions
4. **Network security** - HTTPS for all network communication
5. **Input validation** - Validate all external data

## Requirements

- No secrets in source code or commit history
- Secure by default configuration
- Regular security review of added dependencies
- Authentication/authorization design TBD

## Undecided
- Specific encryption methods
- Authentication framework
- Secure storage solution