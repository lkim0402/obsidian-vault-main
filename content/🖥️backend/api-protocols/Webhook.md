>[!Note] Webhook
>A Webhook is an automated, server-to-server notification triggered by a specific event.

- Like a **doorbell**. 
	- You give a service (like GitHub or Stripe) your "address" (a unique URL on your server). You tell them, "When _event X_ happens, come to this address and ring the bell." You aren't keeping a constant connection open. You're just trusting them to notify you.
- **How it works:** This is not a persistent connection. It's simply an **HTTP POST request** that one server (the provider) sends to another server (yours) when an event occurs.
- **Data Flow:** **Unidirectional** (Server → Server). The "client" in this case is your server.
- Common Use Cases:
    - CI/CD: GitHub sends a webhook to your build server (like Jenkins or GitHub Actions) on a `git push`, triggering a new build.
    - Payments: Stripe sends a webhook to your e-commerce site to confirm "Payment successful" or "Payment failed."
    - Content Updates: A headless CMS sends a webhook to your website to trigger a rebuild when an editor publishes a new blog post.