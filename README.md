# Fix MongoDB DNS Resolution Error in Node.js

## Problem

MongoDB connection may fail due to DNS resolution issues, especially when connecting to MongoDB Atlas.

Common errors include:

- `ECONNREFUSED`
- `querySrv ECONNREFUSED`
- `DNS resolution failed`

## Solution

Use Node.js's built-in `dns` module to configure Google DNS servers.

### Code

```js
import dns from "node:dns";

dns.setServers(["8.8.8.8", "8.8.4.4"]);
```

### How to Use

Add this code at the **beginning of your main server file**, before connecting to MongoDB.

Example:

```js
import dns from "node:dns";
import mongoose from "mongoose";

dns.setServers(["8.8.8.8", "8.8.4.4"]);

mongoose
  .connect(process.env.MONGODB_URI)
  .then(() => console.log("MongoDB connected successfully"))
  .catch((error) => console.error("MongoDB connection error:", error));
```

## Explanation

- `node:dns` is Node.js's built-in DNS module.
- `dns.setServers()` configures the DNS servers used by Node.js's default DNS resolver.
- `8.8.8.8` is Google's primary public DNS server.
- `8.8.4.4` is Google's secondary public DNS server.

This can resolve certain DNS-related MongoDB connection problems.

## Important Notes

- Add this configuration before the MongoDB connection.
- No additional npm package is required.
- This is a DNS configuration workaround, not a universal fix for every MongoDB error.
- If the issue persists, check your internet connection, MongoDB Atlas IP access list, database credentials, and network restrictions.
- Changing DNS servers does not encrypt DNS queries. Use an appropriate trusted DNS configuration for your environment.

## Requirements

- Node.js
- MongoDB or MongoDB Atlas
- Mongoose (if used in your project)
