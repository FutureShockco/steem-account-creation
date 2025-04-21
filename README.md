# Steem Account Creation Service

This service allows you to create new Steem accounts with automatic delegation of VESTS. It's designed to be used as a backend service for applications that need to create Steem accounts programmatically.

## Setup

1. Clone the repository
2. Install dependencies:
```bash
npm install
```

3. Create a `.env` file with the following variables:
```env
PORT=3000
STEEMD_URL=https://api.steemit.com
CREATOR_ACCOUNT=fundition
CREATOR_KEY=your_creator_private_key
API_KEY=your_secure_api_key_here
SHOULD_DELEGATE=true
DELEGATION_AMOUNT=6020.000000 VESTS
ACCOUNT_PREFIX=dw-
POSTING_AUTHORITIES="drugwars,fundition,future.app"
```

4. Start the server:
```bash
npm start
```

## API Usage

### Create a New Steem Account

**Endpoint:** `GET /createwallet/:name`

**Parameters:**
- `:name` - String containing the name to use for account generation (minimum 3 characters)

**Headers:**
- `x-api-key`: Your API key

**Response:**
```json
{
  "ownerKey": "STM...",
  "activeKey": "STM...",
  "postingKey": "STM...",
  "memo_key": "STM..."
}
```

**Example Requests:**

Using curl:
```bash
curl -H "x-api-key: your_api_key_here" http://localhost:3000/createwallet/john
```

Using JavaScript fetch:
```javascript
fetch('http://localhost:3000/createwallet/john', {
  headers: {
    'x-api-key': 'your_api_key_here'
  }
})
.then(response => response.json())
.then(data => console.log(data))
.catch(error => console.error('Error:', error));
```

Using Python requests:
```python
import requests

headers = {
    'x-api-key': 'your_api_key_here'
}

response = requests.get(
    'http://localhost:3000/createwallet/john',
    headers=headers
)

print(response.json())
```

**Example Response:**
```json
{
  "ownerKey": "STM8Qyk...",
  "activeKey": "STM7Qyk...",
  "postingKey": "STM6Qyk...",
  "memo_key": "STM5Qyk..."
}
```

## Account Format

The service creates accounts with the following format:
- Prefix: Configured in `ACCOUNT_PREFIX` (default: `dw-`)
- First letter of the name
- 4 random characters
- Next 6 characters of the name
- 1 random character

Example: For name `john`, the account name might be: `dw-j4x2john3`

## Features

- Automatic account creation on the Steem blockchain
- Configurable VESTS delegation (default: 6020.000000 VESTS)
- Secure key generation
- CORS protection
- Security headers via Helmet
- API key authentication

## Configuration

The service can be configured through environment variables:

- `SHOULD_DELEGATE`: Set to `true` to enable automatic delegation (default: true)
- `DELEGATION_AMOUNT`: Amount of VESTS to delegate to new accounts (default: "6020.000000 VESTS")
- `ACCOUNT_PREFIX`: Prefix for generated account names (default: "dw-")
- `POSTING_AUTHORITIES`: Comma-separated list of accounts to add as posting authorities

## Notes

- The service uses the `fundition` account as the creator
- Each new account receives the configured amount of VESTS delegation (if enabled)
- The posting authority includes accounts specified in `POSTING_AUTHORITIES`
- Make sure to securely store the returned keys as they cannot be recovered if lost
- Keep your API key secure and never share it publicly

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