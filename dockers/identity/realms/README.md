# Keycloak Realm Configuration

## Security Notice

This directory contains Keycloak realm configuration files that are automatically imported when the identity service starts with the `--import-realm` flag.

### Important Security Considerations

⚠️ **WARNING**: The realm configuration defines privileged user accounts with the "System admin" role, which grants full realm administration privileges including `realm-admin` access.

#### Current Security Measures

1. **Disabled by Default**: All "System admin" users are created in a disabled state and cannot authenticate until manually enabled by an administrator through the Keycloak admin console.

2. **No Hardcoded Passwords**: Privileged accounts do not have hardcoded credentials in the configuration file.

3. **Required Password Update**: All "System admin" users have the `UPDATE_PASSWORD` required action, which forces them to set a new password upon first login (after being enabled).

#### Setup Instructions for Development

To use the "System admin" accounts in a development environment:

1. Access the Keycloak admin console at `http://identity.local` (or your configured domain)
2. Log in with the Keycloak admin credentials (defined in `.env` as `KC_ADMIN` and `KC_ADMIN_PASSWORD`)
3. Navigate to the `node-msa` realm
4. Go to Users and select the user you want to enable
5. Enable the user account
6. Set a temporary password for the user (or send a password reset email)
7. The user will be required to change their password on first login

#### Production Deployment

🚨 **CRITICAL**: This realm configuration file is intended for development and testing purposes only.

For production deployments:

- **DO NOT** use this realm import file
- Create users manually through the Keycloak admin console
- Use strong, unique passwords for all administrative accounts
- Enable multi-factor authentication (MFA) for privileged accounts
- Implement proper access controls and audit logging
- Consider using external identity providers (LDAP, SAML, OIDC) instead of local accounts
- Regularly review and audit user permissions

#### Non-Privileged Users

The configuration includes a sample non-privileged user (`thangongduy`) with the "User" role for testing basic authentication flows. This account has limited permissions and does not pose a significant security risk in development environments. However, it should also be removed or have its password changed in production.

## Realm Structure

- **Realm Name**: `node-msa`
- **Roles**:
  - `User`: Basic user role with account management permissions
  - `Admin`: Administrator role with user management capabilities
  - `System admin`: Full system administrator with realm-admin privileges
- **Default Role**: `User`

## References

- [Keycloak Server Administration Guide](https://www.keycloak.org/docs/latest/server_admin/)
- [Keycloak Realm Import/Export](https://www.keycloak.org/docs/latest/server_admin/#_export_import)
