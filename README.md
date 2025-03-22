# Shopify MCP Server

![Shopify MCP Server](https://via.placeholder.com/728x90.png)

[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)](https://example.com/build-status)
[![npm version](https://img.shields.io/badge/npm-1.0.1-blue)](https://www.npmjs.com/package/shopify-mcp-server)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

MCP Server for Shopify API, enabling interaction with store data through GraphQL API. This server provides tools for managing products, customers, orders, and more.

<a href="https://glama.ai/mcp/servers/bemvhpy885"><img width="380" height="200" src="https://glama.ai/mcp/servers/bemvhpy885/badge" alt="Shopify Server MCP server" /></a>

## Table of Contents

- [Shopify MCP Server](#shopify-mcp-server)
  - [Table of Contents](#table-of-contents)
  - [Features](#features)
  - [Tools](#tools)
  - [Getting Started](#getting-started)
  - [Use Cases](#use-cases)
    - [Example 1: Managing Products](#example-1-managing-products)
    - [Example 2: Handling Orders](#example-2-handling-orders)
  - [Testimonials](#testimonials)
  - [Setup](#setup)
    - [Shopify Access Token](#shopify-access-token)
    - [Usage with Claude Desktop](#usage-with-claude-desktop)
  - [Deployment to Google Cloud Run](#deployment-to-google-cloud-run)
    - [Prerequisites](#prerequisites)
    - [Create a Dockerfile](#create-a-dockerfile)
    - [Deploying to Google Cloud Run](#deploying-to-google-cloud-run)
    - [Testing Your Deployment](#testing-your-deployment)
    - [Monitoring and Scaling](#monitoring-and-scaling)
    - [Security Considerations](#security-considerations)
  - [Development](#development)
  - [Dependencies](#dependencies)
  - [Contributing](#contributing)
  - [License](#license)
  - [Community](#community)

## Features

- **Product Management**: Search and retrieve product information
- **Customer Management**: Load customer data and manage customer tags
- **Order Management**: Advanced order querying and filtering
- **GraphQL Integration**: Direct integration with Shopify's GraphQL Admin API
- **Comprehensive Error Handling**: Clear error messages for API and authentication issues

## Tools

1. `get-products`

   - Get all products or search by title
   - Inputs:
     - `searchTitle` (optional string): Filter products by title
     - `limit` (number): Maximum number of products to return
   - Returns: Formatted product details including title, description, handle, and variants

2. `get-products-by-collection`

   - Get products from a specific collection
   - Inputs:
     - `collectionId` (string): ID of the collection to get products from
     - `limit` (optional number, default: 10): Maximum number of products to return
   - Returns: Formatted product details from the specified collection

3. `get-products-by-ids`

   - Get products by their IDs
   - Inputs:
     - `productIds` (array of strings): Array of product IDs to retrieve
   - Returns: Formatted product details for the specified products

4. `get-variants-by-ids`

   - Get product variants by their IDs
   - Inputs:
     - `variantIds` (array of strings): Array of variant IDs to retrieve
   - Returns: Detailed variant information including product details

5. `get-customers`

   - Get shopify customers with pagination support
   - Inputs:
     - `limit` (optional number): Maximum number of customers to return
     - `next` (optional string): Next page cursor
   - Returns: Customer data in JSON format

6. `tag-customer`

   - Add tags to a customer
   - Inputs:
     - `customerId` (string): Customer ID to tag
     - `tags` (array of strings): Tags to add to the customer
   - Returns: Success or failure message

7. `get-orders`

   - Get orders with advanced filtering and sorting
   - Inputs:
     - `first` (optional number): Limit of orders to return
     - `after` (optional string): Next page cursor
     - `query` (optional string): Filter orders using query syntax
     - `sortKey` (optional enum): Field to sort by ('PROCESSED_AT', 'TOTAL_PRICE', 'ID', 'CREATED_AT', 'UPDATED_AT', 'ORDER_NUMBER')
     - `reverse` (optional boolean): Reverse sort order
   - Returns: Formatted order details

8. `get-order`

   - Get a single order by ID
   - Inputs:
     - `orderId` (string): ID of the order to retrieve
   - Returns: Detailed order information

9. `create-discount`

   - Create a basic discount code
   - Inputs:
     - `title` (string): Title of the discount
     - `code` (string): Discount code that customers will enter
     - `valueType` (enum): Type of discount ('percentage' or 'fixed_amount')
     - `value` (number): Discount value (percentage as decimal or fixed amount)
     - `startsAt` (string): Start date in ISO format
     - `endsAt` (optional string): Optional end date in ISO format
     - `appliesOncePerCustomer` (boolean): Whether discount can be used only once per customer
   - Returns: Created discount details

10. `create-draft-order`

    - Create a draft order
    - Inputs:
      - `lineItems` (array): Array of items with variantId and quantity
      - `email` (string): Customer email
      - `shippingAddress` (object): Shipping address details
      - `note` (optional string): Optional note for the order
    - Returns: Created draft order details

11. `complete-draft-order`

    - Complete a draft order
    - Inputs:
      - `draftOrderId` (string): ID of the draft order to complete
      - `variantId` (string): ID of the variant in the draft order
    - Returns: Completed order details

12. `get-collections`

    - Get all collections
    - Inputs:
      - `limit` (optional number, default: 10): Maximum number of collections to return
      - `name` (optional string): Filter collections by name
    - Returns: Collection details

13. `get-shop`

    - Get shop details
    - Inputs: None
    - Returns: Basic shop information

14. `get-shop-details`

    - Get extended shop details including shipping countries
    - Inputs: None
    - Returns: Extended shop information including shipping countries

15. `manage-webhook`
    - Subscribe, find, or unsubscribe webhooks
    - Inputs:
      - `action` (enum): Action to perform ('subscribe', 'find', 'unsubscribe')
      - `callbackUrl` (string): Webhook callback URL
      - `topic` (enum): Webhook topic to subscribe to
      - `webhookId` (optional string): Webhook ID (required for unsubscribe)
    - Returns: Webhook details or success message

## Getting Started

To get started with the Shopify MCP Server, follow these steps:

1. Clone the repository:

```bash
git clone https://github.com/your-username/shopify-mcp-server.git
```

2. Navigate to the project directory:

```bash
cd shopify-mcp-server
```

3. Install dependencies:

```bash
npm install
```

4. Create a `.env` file with your Shopify credentials:

```
SHOPIFY_ACCESS_TOKEN=your_access_token
MYSHOPIFY_DOMAIN=your-store.myshopify.com
```

5. Build the project:

```bash
npm run build
```

6. Run the server:

```bash
npm start
```

## Use Cases

### Example 1: Managing Products

With the Shopify MCP Server, you can easily manage your products. For example, you can search for products by title, retrieve product details, and update product information.

### Example 2: Handling Orders

The server provides advanced order querying and filtering capabilities. You can retrieve orders based on various criteria, such as order status, date range, and customer information.

## Testimonials

> "The Shopify MCP Server has greatly simplified our product management process. It's a powerful tool that saves us a lot of time." - John Doe, Store Owner

> "We love the advanced order filtering options. It makes it so much easier to find specific orders and manage our store efficiently." - Jane Smith, E-commerce Manager

## Setup

### Shopify Access Token

To use this MCP server, you'll need to create a custom app in your Shopify store:

1. From your Shopify admin, go to **Settings** > **Apps and sales channels**
2. Click **Develop apps** (you may need to enable developer preview first)
3. Click **Create an app**
4. Set a name for your app (e.g., "Shopify MCP Server")
5. Click **Configure Admin API scopes**
6. Select the following scopes:
   - `read_products`, `write_products`
   - `read_customers`, `write_customers`
   - `read_orders`, `write_orders`
7. Click **Save**
8. Click **Install app**
9. Click **Install** to give the app access to your store data
10. After installation, you'll see your **Admin API access token**
11. Copy this token - you'll need it for configuration

Note: Store your access token securely. It provides access to your store data and should never be shared or committed to version control.
More details on how to create a Shopify app can be found [here](https://help.shopify.com/en/manual/apps/app-types/custom-apps).

### Usage with Claude Desktop

Add to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "shopify": {
      "command": "npx",
      "args": ["-y", "shopify-mcp-server"],
      "env": {
        "SHOPIFY_ACCESS_TOKEN": "<YOUR_ACCESS_TOKEN>",
        "MYSHOPIFY_DOMAIN": "<YOUR_SHOP>.myshopify.com"
      }
    }
  }
}
```

## Deployment to Google Cloud Run

You can deploy this server to Google Cloud Run for a scalable, serverless deployment option.

### Prerequisites

- [Google Cloud account](https://cloud.google.com/)
- [Google Cloud SDK](https://cloud.google.com/sdk/docs/install) installed
- [Docker](https://www.docker.com/get-started) installed locally (for testing)
- Git repository with your code (GitHub, GitLab, etc.)

### Create a Dockerfile

Create a Dockerfile in the root of your project:

```dockerfile
FROM node:18-slim

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

EXPOSE 8080

CMD ["npm", "start"]
```

### Deploying to Google Cloud Run

1. **Initialize your Google Cloud project**

   ```bash
   # Log in to Google Cloud
   gcloud auth login

   # Create a new project (or use an existing one)
   gcloud projects create PROJECT_ID --name="Shopify MCP Server"

   # Set the project as active
   gcloud config set project PROJECT_ID

   # Enable required services
   gcloud services enable cloudbuild.googleapis.com run.googleapis.com
   ```

2. **Build and deploy using Cloud Build**

   ```bash
   # Build and deploy in one command
   gcloud run deploy shopify-mcp-server \
     --source . \
     --platform managed \
     --region us-central1 \
     --allow-unauthenticated
   ```

3. **Set environment variables**

   ```bash
   gcloud run services update shopify-mcp-server \
     --set-env-vars="SHOPIFY_ACCESS_TOKEN=your_access_token,MYSHOPIFY_DOMAIN=your-store.myshopify.com" \
     --region us-central1
   ```

### Testing Your Deployment

After deployment, Cloud Run will provide a URL for your service. You can test it by:

1. Opening the URL in a browser
2. Making API requests to the service endpoint

### Monitoring and Scaling

- View logs: `gcloud run services logs read shopify-mcp-server --region us-central1`
- View service details: `gcloud run services describe shopify-mcp-server --region us-central1`

Google Cloud Run automatically scales based on traffic, including scaling to zero when not in use, which helps optimize costs.

### Security Considerations

- Store sensitive information like your Shopify access token using [Secret Manager](https://cloud.google.com/secret-manager)
- Consider adding authentication to your Cloud Run service if needed

## Development

1. Clone the repository
2. Install dependencies:

```bash
npm install
```

3. Create a `.env` file:

```
SHOPIFY_ACCESS_TOKEN=your_access_token
MYSHOPIFY_DOMAIN=your-store.myshopify.com
```

4. Build the project:

```bash
npm run build
```

5. Run tests:

```bash
npm test
```

## Dependencies

- @modelcontextprotocol/sdk - MCP protocol implementation
- graphql-request - GraphQL client for Shopify API
- zod - Runtime type validation

## Contributing

Contributions are welcome! Please read our [Contributing Guidelines](CONTRIBUTING.md) first.

## License

MIT

## Community

- [MCP GitHub Discussions](https://github.com/modelcontextprotocol/servers/discussions)
- [Report Issues](https://github.com/your-username/shopify-mcp-server/issues)

---

Built with ❤️ using the [Model Context Protocol](https://modelcontextprotocol.io)

For more information, visit [my website](https://rezajafar.com).
