# ROOMZ's Webhook Specification
Receive ROOMZ events in your webhook endpoint and trigger reactions in your integration.

![](../assets/webhook.png)

1. When a resource changes, a Webhook event is posted to your webhook url.
2. Your application [receives an event](#receive-events) and must respond to it.
3. Your integration makes a separate API call to [get full event details](#get-full-event-details).

## Receive events 

### The WebhookEvent Object

For every change in resources, ROOMZ will post a WebhookEvent object to your webhook url, that includes a few important fields.

The WebhookEvent object is described in [ROOMZ's OpenAPI Specification](../openapi/spec3.yml)

### Use HTTPS

You should ensure that your receiving url uses an HTTPS connection.

### Use the webhook secret

Your webhook secret will be used to generate a hash signature that is sent to your application with the event in order to ensure that it is from ROOMZ.

The hash signature is included as the value of the `X-Roomz-Signature` header.

The hash signature is generated using your webhook secret and the event payload.

### Respond fast
Your integration should respond with a  ``2xx`` status code response as fast as possible.
Any other status codes are considered a delivery failure.

You might want to process the event asynchronously. Your integration can respond when it receives the webhook, and then process the payload in the background.

>Requests aren't time limited at the moment, but time-to-live is subject to change in the future.

### Handle duplicate events
Your webhook url might sometimes receive the same event multiple times. 

To prevent receiving duplicate events, ensure your event handling is idempotent. 

One method is to log processed events and avoid reprocessing those already logged.

## Get full event details 

The WebhookEvent Object do not contain specific information about what has changed for the resource, you will have to make a separate API call to get the full event details.

For instance,  your integration has received a WebhookEvent that a workspace status has changed:
````
{
    "id":"4f6aa5b6-8d68-4b48-b779-861fba5c20ba",
    "type":"workspace.status.updated",
    "timestamp":"2024-02-02T09:13:44+0000",
    "workspaceId": "df801b47-b7be-48e8-8e21-20e20c75906c"
}
````
Then,  your integration can fetch the new workspace status using the workspace ID provided in the WebhookEvent object:

````
GET <ROOMZAPIURL>/workspaces/df801b47-b7be-48e8-8e21-20e20c75906c/status
````
````
response:
{
  "status": "Free"
}
````