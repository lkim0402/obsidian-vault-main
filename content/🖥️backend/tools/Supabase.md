
>[!Supabase]
>The Open-Source Alternative to [[Firebase]]
>- Supabase is a serverless backend platform built on the open-source database **PostgreSQL**. ([[SQL (Relational) vs NoSQL (Non Relational)]])
>- This allows you to use standard [[SQL]] to create complex table structures and relationships, just like a traditional database. 
>- Provides a feature set *very similar to Firebase*, including serverless functions, real-time subscriptions, authentication, and file storage.
>- Part of [[Serverless Architectures & APIs]]

# Core Features

|Feature|Description|
|---|---|
|**Supabase Auth**|Provides authentication via email or social logins.|
|**Supabase DB**|A full SQL database built on PostgreSQL.|
|**Edge Functions**|Serverless functions that run on the Deno runtime.|
|**Realtime**|Automatically detects database changes and pushes them to listening clients.|
|**Storage**|Allows you to store images and files, with fine-grained access permissions.|
# Example - Supabase Edge Function
```ts
// hello-world.ts
import { serve } from "https://deno.land/std/http/server.ts";

serve((_req) => new Response("Hello from Supabase Edge Function"));
```

# Advantages
- **SQL-Based**: It can naturally handle complex queries and relationships because it's built on a standard SQL database.
- **Open-Source**: You have the option to self-host your entire backend, giving you full control.
- **Easy CI/CD**: Offers simple CI/CD (Continuous Integration/Deployment) through its native GitHub integration.
# Disadvantages
- **Weaker Real-time Features**: Its real-time capabilities are generally considered less mature than [[Firebase]]'s.
- **Beta-Level Functions**: The Edge Functions feature is still *relatively new* and may *not be as stable or feature-rich* as more mature solutions.
- **Paid Korean Region**: To host your project in the Seoul, Korea region, you must be on a paid plan.