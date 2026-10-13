# areex-automation

## Security hardening notes

- Admin console route: `/rtx5090`. The server still authorizes every admin API request; a hidden/unguessable URL is not an authorization control.
- Customer accounts must enroll in authenticator-app TOTP before a customer session is issued. Existing accounts are prompted to enroll at their next login.
- Admin login requires a server-side password and a current TOTP code. Configure `ADMIN_PASSWORD_1`, `ADMIN_PASSWORD_2`, `ADMIN_TOTP_SECRET_1`, and `ADMIN_TOTP_SECRET_2` in the hosting environment before deploying.
- Set unique random values for `SESSION_SECRET` and `AES_MASTER_KEY` (at least 32 characters each). Never commit `.env` or real secrets to GitHub. Rotate any credentials that were ever present in source code.
- The server writes request/change metadata to `.areex-audit.jsonl` and displays the latest 1,000 entries under Admin Console → Audit Log. The log intentionally stores field names, not submitted values, passwords, OTP codes, or tokens. It records API requests; it cannot record client-side clicks that never reach the server.
- `.areex-store.json`, `.areex-users.json`, and `.areex-audit.jsonl` must live on persistent, writable server storage and be included in backups. Ephemeral/serverless deployments need a persistent database or mounted volume; otherwise runtime files may disappear on redeploy.
- Use HTTPS, restrict server access, keep dependencies patched, and test login/recovery flows before switching production traffic. No code can guarantee absolute security.


For required production secrets and authenticator setup, follow [`SECURITY_SETUP.md`](./SECURITY_SETUP.md) before redeploying.
