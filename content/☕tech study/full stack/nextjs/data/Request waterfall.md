
>[!what is it]
>A sequence of network requests that depend on the completion of previous requests
>- In the case of data fetching, each request can only begin once the previous request has returned data

- the total time it takes to load all the necessary data for the page is the _sum_ of the time it takes for each individual data request
- the data requests are unintentionally blocking each other
- The Next.js documentation is highlighting a potential pitfall when fetching data within Server Components _sequentially_. If you fetch data in one Server Component and _only after_ that data arrives do you decide to fetch more data in a child Server Component, you create this waterfall effect.
solution
- key: initiate as many data requests as possible **concurrently**
- use [[React Server Components]]!

####  example
```tsx
const revenue = await fetchRevenue();
const latestInvoices = await fetchLatestInvoices(); // wait for fetchRevenue() to finish
const {
  numberOfInvoices,
  numberOfCustomers,
  totalPaidInvoices,
  totalPendingInvoices,
} = await fetchCardData(); // wait for fetchLatestInvoices() to finish
```
