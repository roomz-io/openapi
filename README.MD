# ROOMZ's Public API Specification 
![](https://img.shields.io/badge/Status-Private_Preview-blue)
![Static Badge](https://img.shields.io/badge/Available-Mid--July_2024-brightgreen)


This repository contains [OpenAPI specifications](https://www.openapis.org/) for ROOMZ's Public API.

>_Please contact the [Sales team](mailto:sales@roomz.io) if you are interested in joining the private preview, which is expected to open in Mid-July 2024._

## Authentication
The ROOMZ API uses API keys to authenticate requests. You can view and manage your API keys in the ROOMZ Portal (administrator role is required).

**Never share your secret API keys.**

Authentication to the API is performed via HTTP `Basic Auth`. Use your API key as the basic auth username value, no password is needed.

API requests __must__ be made over HTTPS. All other calls will fail, as well as call without auth.

## Rate Limits
>There aren't any rate limits at the moment, but this is subject to change in the future.