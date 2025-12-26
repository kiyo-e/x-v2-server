# X(Twitter) V2 MCP Server

[![smithery badge](https://smithery.ai/badge/@NexusX-MCP/x-v2-server)](https://smithery.ai/server/@NexusX-MCP/x-v2-server)

An MCP server implementation that provides tools for interacting with the [Twitter/X API v2](https://docs.x.com/x-api/introduction). This service allows AI assistants to retrieve tweets, post new content, reply to tweets, and quote tweets and more programmatically.

## Tools
The X MCP Service provides the following tools for interacting with the Twitter/X API:

### get_tweets_by_userid
Retrieves tweets from a specific user's timeline.
- `userId`: The Twitter user ID to search for tweets
- `paginationToken` (optional): Token for fetching the next page of results
- `exclude` (optional): Types of tweets to exclude (retweets, replies)
- `maxResults` (optional): Maximum number of tweets to return (default: 10)

### get_tweet_by_id
Retrieves a single tweet by its ID.
- `tweetId`: The ID of the tweet to retrieve

### get_user_mentions
Retrieves tweets that mention a specific user.
- `userId`: The Twitter user ID to get mentions for
- `paginationToken` (optional): Token for fetching the next page of results
- `maxResults` (optional): Maximum number of mentions to return (default: 10)

### quote_tweet
Creates a quote tweet with custom text.
- `tweetId`: The ID of the tweet to quote
- `replyText`: The text to include with the quote

### reply_to_tweet
Replies to an existing tweet.
- `tweetId`: The ID of the tweet to reply to
- `replyText`: The text content of the reply

### post_tweet
Post a new tweet.
- `text`: The content that you want to post.
- `imageBase64`: Image that you want to post.

### like_tweet
Like a specific tweet.
- `tweetId`: The ID of the tweet to like

### follow_user
Follow a Twitter user.
- `targetUserId`: The ID of the user to follow

### unfollow_user
Unfollow a Twitter user.
- `targetUserId`: The ID of the user to unfollow

### get_user_by_username
Get information about a Twitter user by their username.
- `username`: The Twitter username (without @ symbol)

### search_tweets
Search for tweets using a query string.
- `query`: The search query
- `maxResults` (optional): Maximum number of results to return (default: 10)

### get_trending_topics
Get trending topics for a specific location.
- `woeid` (optional): The 'Where On Earth ID' (WOEID) for the location (1 for worldwide, default: 1)

### create_list
Create a new Twitter list.
- `name`: The name of the list
- `description` (optional): Optional description for the list
- `isPrivate` (optional): Whether the list should be private (default: false)

### add_list_member
Add a user to a Twitter list.
- `listId`: The ID of the list
- `userId`: The ID of the user to add

### remove_list_member
Remove a user from a Twitter list.
- `listId`: The ID of the list
- `userId`: The ID of the user to remove

### get_owned_lists
Get all lists owned by the authenticated user.
- No parameters required

## Configuration

## Env Configuration

### X API Authentication

You can get all of the token below via [X Developer Dashboard](https://developer.x.com/en/portal/products)

```
TWITTER_API_KEY=your_api_key
TWITTER_API_KEY_SECRET=your_api_secret
TWITTER_ACCESS_TOKEN=your_access_token
TWITTER_ACCESS_TOKEN_SECRET=your_access_token_secret
```

## Development

### Local Development (Node.js)

```bash
npm i

npm run build

npx @modelcontextprotocol/inspector node dist/index.js
```

Open http://127.0.0.1:6274 set up env, and interact with the tools.

### Cloudflare Workers Deployment (Hono + MCP)

This project can be deployed to Cloudflare Workers using:
- **Hono** - Lightweight web framework
- **@hono/mcp** - Streamable HTTP Transport for Model Context Protocol
- **Twitter API v2** - Social media integration

Note: Deployment commands are configured but actual deployment is not performed by default.

#### Setup

1. Install dependencies:
```bash
npm install
```

2. Create a `.dev.vars` file for local development with your Twitter credentials:
```
TWITTER_API_KEY=your_api_key
TWITTER_API_KEY_SECRET=your_api_secret
TWITTER_ACCESS_TOKEN=your_access_token
TWITTER_ACCESS_TOKEN_SECRET=your_access_token_secret
```

#### Local Testing (Cloudflare Workers)

```bash
npm run dev:workers
```

This will start the Cloudflare Workers development server locally.

#### Production Deployment (Manual)

To deploy to Cloudflare Workers, you need to:

1. Set up secrets in Cloudflare Workers:
```bash
wrangler secret put TWITTER_API_KEY
wrangler secret put TWITTER_API_KEY_SECRET
wrangler secret put TWITTER_ACCESS_TOKEN
wrangler secret put TWITTER_ACCESS_TOKEN_SECRET
```

2. Deploy using:
```bash
npm run deploy:workers
```

Note: Make sure you have authenticated with Cloudflare CLI (`wrangler login`) before deploying.

#### MCP Endpoints

Once deployed, the following endpoints are available:

**Health Check:**
- `GET /` - Server status and information

**MCP Streamable HTTP:**
- `POST /mcp` - MCP protocol endpoint using Streamable HTTP Transport
  - All MCP tools are accessible through this endpoint
  - Supports standard MCP protocol communication

#### Available MCP Tools

The following tools are available through the `/mcp` endpoint:
- `get_tweets_by_userid` - Get tweets by user ID
- `get_tweet_by_id` - Get a tweet by ID
- `get_user_mentions` - Get mentions for a user
- `quote_tweet` - Quote a tweet with custom text
- `reply_to_tweet` - Reply to a tweet
- `post_tweet` - Post a new tweet (with optional image)
- `like_tweet` - Like a tweet
- `follow_user` - Follow a user
- `unfollow_user` - Unfollow a user
- `get_user_by_username` - Get user information by username
- `search_tweets` - Search for tweets
- `get_trending_topics` - Get trending topics
- `create_list` - Create a Twitter list
- `add_list_member` - Add a member to a list
- `remove_list_member` - Remove a member from a list
- `get_owned_lists` - Get all lists owned by authenticated user

#### Connecting to the MCP Server

To connect to this MCP server from a client application, use the Streamable HTTP transport pointing to your deployed URL:

```
https://your-worker.workers.dev/mcp
```

## License
This MCP server is licensed under the MIT License. This means you are free to use, modify, and distribute the software, subject to the terms and conditions of the MIT License. For more details, please see the LICENSE file in the project repository.
