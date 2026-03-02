
>[!Firebase]
>Google's All-in-One BaaS (Backend-as-a-Service)
>- Firebase is a BaaS platform offered by Google. It provides an **integrated suite of backend services, including authentication, a real-time database, storage, push notifications (FCM), and its own serverless functions (Cloud Functions).**
>- Part of [[Serverless Architectures & APIs]], along with [[Supabase]]
>- Used a lot with mobile applications / real time interaction based apps

A typical Firebase architecture might look like this:

```
[Client App]
    ↓
[Firebase Auth ←→ Firebase Realtime DB]
    ↓
[Cloud Functions (Serverless Functions)]
    ↓
Firebase Storage, FCM, Firestore, etc._
```

# Core Features

| Feature                     | Description                                                          |
| --------------------------- | -------------------------------------------------------------------- |
| **Firebase Authentication** | Provides easy sign-in with providers like Google, Apple, Kakao, etc. |
| **Firestore / Realtime DB** | Enables real-time data synchronization across all clients.           |
| **Cloud Functions**         | Event-driven serverless functions for running your backend code.     |
| **FCM**                     | Firebase Cloud Messaging (for sending mobile push notifications).    |
# Example (w/ [[Node.js]])
```javascript
const functions = require("firebase-functions");

exports.helloWorld = functions.https.onRequest((req, res) => {
  res.send("Hello from Firebase!");
});
```
# Advantages
- **Powerful Real-time Features**: Excellent for building applications that require instant data synchronization.
- **Strong Mobile Integration**: Offers deep and robust integration with mobile apps.
- **Rich Client SDKs**: Provides comprehensive SDKs for various platforms, including iOS, Android, and [[JavaScript]].
# Disadvantages
- **Steep Pricing**: The cost can increase sharply once you move beyond the free tier to paid plans.
- **NoSQL Only**: Primarily supports NoSQL databases (Firestore, Realtime DB), which makes complex queries and relations difficult compared to SQL.
	- [[SQL (Relational) vs NoSQL (Non Relational)]]
- **Limited Server Regions**: There are restrictions on where you can host your data. For example, while some newer Firebase services can be hosted in Seoul, the original Realtime Database cannot be, forcing data to be stored elsewhere in Asia.
    

