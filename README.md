# Steem Account Creation Service

A service for creating Steem accounts with automatic VESTS delegation.

## Setup

1. Install dependencies:
```bash
npm install
```

2. Create `.env` file with the following configuration:
```env
PORT=3000
STEEMD_URL=https://api.steemit.com
CREATOR_ACCOUNT=creator_account
CREATOR_KEY=your_creator_private_key
API_KEY=your_secure_api_key_here
SHOULD_DELEGATE=true
DELEGATION_AMOUNT=6020.000000 VESTS
ACCOUNT_PREFIX=dw-
POSTING_AUTHORITIES="drugwars,fundition,future.app"
```

3. Start the server:
```bash
npm start
```

## API Endpoints

### Create Account
```bash
curl -H "x-api-key: your_api_key_here" http://localhost:3000/createwallet/username
```

Response:
```json
{
  "ownerKey": "5J...",
  "activeKey": "5J...",
  "postingKey": "5J...",
  "memo_key": "STM..."
}
```

## Configuration Options

- `PORT`: Server port (default: 3000)
- `STEEMD_URL`: Steem blockchain API endpoint
- `CREATOR_ACCOUNT`: Account used to create new accounts
- `CREATOR_KEY`: Private key of the creator account
- `API_KEY`: Secure key for API authentication
- `SHOULD_DELEGATE`: Enable/disable VESTS delegation (true/false)
- `DELEGATION_AMOUNT`: Amount of VESTS to delegate to new accounts
- `ACCOUNT_PREFIX`: Prefix for account names (e.g., "dw-" + username)
- `POSTING_AUTHORITIES`: Comma-separated list of accounts with posting authority

## Account Name Format

The service creates accounts with the following format:
- Prefix: Configured in `ACCOUNT_PREFIX` (default: `dw-`)
- Username: The provided username

Example: For username `john`, the account name will be: `dw-john`

## Security Features

- API key authentication
- CORS protection
- Helmet security headers
- HTTPS recommended for production use

## Error Handling

The API returns appropriate HTTP status codes:

- `200`: Success
- `400`: Bad request (invalid input format)
- `401`: Unauthorized (invalid or missing API key)
- `500`: Server error

Error responses include a message explaining the issue:
```json
{
  "error": "Error message here"
}
```

## Security Notes

1. Always use HTTPS in production
2. Keep your API key secure
3. Implement proper error handling in your client applications
4. Consider implementing additional security measures like IP whitelisting
5. Regularly rotate your API keys
6. Monitor your API usage for suspicious activity

## Important Notes

- The service uses the configured creator account to create new accounts
- Each new account receives the configured amount of VESTS delegation (if enabled)
- The posting authority includes accounts specified in `POSTING_AUTHORITIES`
- Make sure to securely store the returned keys as they cannot be recovered if lost
- Keep your API key secure and never share it publicly 