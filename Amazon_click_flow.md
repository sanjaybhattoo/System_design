# Amazon.com Click Flow -- System Design Notes

## Overview

This document explains what happens when a user clicks on amazon.com,
covering networking, backend, and frontend flow.

------------------------------------------------------------------------

## 1. DNS Resolution

-   Browser checks cache
-   If not found → DNS query resolves domain to IP
-   Routed to nearest CDN/edge server

------------------------------------------------------------------------

## 2. Connection Setup

-   TCP 3-way handshake
-   TLS handshake (HTTPS encryption)

------------------------------------------------------------------------

## 3. HTTP Request

-   Browser sends GET request
-   Load balancer receives and routes request

------------------------------------------------------------------------

## 4. Backend Processing

### Microservices involved:

-   Authentication Service
-   Product Catalog Service
-   Pricing Service
-   Inventory Service
-   Recommendation Service

### Data Layer:

-   Cache (Redis/Memcached)
-   Databases (DynamoDB, Aurora, S3)

------------------------------------------------------------------------

## 5. Response

-   HTML + JSON generated
-   Static assets served via CDN

------------------------------------------------------------------------

## 6. Browser Rendering

-   HTML → DOM
-   CSS → CSSOM
-   JS execution
-   Render Tree → Layout → Paint

------------------------------------------------------------------------

## 7. Post Load

-   Analytics tracking
-   Lazy loading APIs
-   Dynamic updates

------------------------------------------------------------------------

## Interview Summary (Short Answer)

User click → DNS → TCP/TLS → Load balancer → Microservices → Cache/DB →
CDN → Browser render → Interactive page

------------------------------------------------------------------------

## Key Concepts

-   CDN for low latency
-   Microservices architecture
-   Heavy caching
-   Async loading
-   Precomputed recommendations
