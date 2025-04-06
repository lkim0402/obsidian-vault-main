- fixing the image getting bigger when scrolling on mobile
	- https://stackoverflow.com/questions/42476248/fixed-background-image-zooms-in-when-scrolling-on-mobile
``` tsx
<div
	className="fixed inset-0 z-[-1] h-screen w-full bg-cover bg-center"
	style={{ backgroundImage: `url(${imgUrl})` }}
/>
```