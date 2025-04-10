# Background

I've always wanted to create a blog and write, ever since I was like, in 2nd grade. I actually had owned an kpop/art/anime blog when I was in about 4th or 5th grade, and I used Naver blog. For those that aren't familiar, [Naver blog](https://section.blog.naver.com/BlogHome.naver?directoryNo=0&currentPage=1&groupId=0) is a free online blogging platform, usually one of the default options for koreans along with Tistory/Postype; I'm Korean, and at that age I thought this was the only available option lol. I used to spend all day writing posts and looking at other people's blogs, writing comments and subscribing to bunch of random people. I don't know what it was, but interacting with internet strangers and engaging in their posts & having what I wrote being read by others gave me so much fun. My peak was that I got around 700-800 subscribers, and about 300 visitors ~day. Unfortunately as I grew older, I nuked the blog as I thought it interfered with my studies & grades (RIP).

Now growing up as a college student majoring in CS, I kept having the idea of starting my own blog again, and the thought never really left my mind. I actually tried using tools like [Hugo](https://gohugo.io/) and [Ghost](https://ghost.org/); I didn't use Hugo because I wanted full control of my website, and Ghost because I am still a college student and did not want to afford paying every month for their subscription fee (I love their clean aesthetic though).

And now that I am learning how to code as a student, I wanted to

1. challenge myself
    
2. document my projects, learnings, and everything else because I'm a forgetful person
    
3. build a public portfolio that showcases my skills and growth over time (and also consider a free option because I did not want to pay😭).
    

I know I lack the skills/projects/extracurricular compared to my peers at UW, and I’ve spent way too much time comparing myself to them—seeing people with full resumes, published research papers, internships at FAANG, and dozens of side projects. It used to give me 100% imposter syndrome, but I decided I’d start small. This blog is kind of my way of challenging myself and getting rid of my imposter syndrome.

As a learning student, I know there are lots of room for improvement for this project, and there are tons of features I want to implement gradually (such as a guestbook, potential comment section, more dynamic components, decoration, etc). It's going to be a long procress, even after I graduate, but I'm happy with that.

## Tech Stack & Architecture

![](https://res.cloudinary.com/dwa6lkvcr/image/upload/v1744095535/s9jveyzv20u6l0cfzxud.png)

_My tech stack & workflow (excalidraw)_

My blog website consists mainly of 3 parts:

1. The frontend blog
    
    - This website with the domain [leejunkim.com](https://www.leejunkim.com/), where you're reading this post.
        
    - This is the main blog/webpage public to the internet.
        
    - Connects to the backend to fetch the posts stored in Mongodb atlas, created in Next.js and Tailwind, and deployed in Vercel.
        
2. The frontend CMS (content management system)
    
    - It's where I can create, read, update, and delete (namely CRUD) my blog posts.
        
    - Created in Next.js + Tailwind, used TipTap as my editor with a bunch of plugins, and Cloudinary to upload & store images.
        
    - Used Firebase Auth for user authentication.
        
    - Has a tagging system and a category system (both also saved as separate collections in mongodb atlas).
        
    - Deployed in Vercel
        
3. The backend
    
    - Handles the fetch requests from both the frontend blog (1) and the frontend CMS (2).
        
    - Using MongoDB Atlas for blog/tag storage, Node.js and Express server + REST API endpoints
        
    - Deployed on Render
        

Reasons

- React + TailwindCSS - They are the tool most familiar to me as of now as I had briefly learnt about them in the past, and there's a HUGE community /resource pool so it would be easy to get help when stuck.
    
- Vercel + Render - As for deployment, I wanted a free tier that wouldn't be a pressure for users to pay, so I went with Vercel for the frontend and Render for the backend, although Render is not 24/7 so it may take a while to spin up the db.
    
- MongoDB - I chose this its flexibility w/ document structure and ease of use. MongoDB driver gave me more direct control over my db operations.
    
- Custom CMS - Mainly 2 reasons:
    
    - I actually didn't know tools like Nelify or Sanity existed, only until I actually completed the main features of my project💀 So I just decided it's a learning opportunity for me
        
    - That being said, I would have complete control over features & no subscription costs
        

The flow of data in my system works like this: I create/edit concent through the CMS, which sents requests to the backend API depoyed at Render. The backend processes these requests and updates the MongoDB database. When users visit my blog, the frontend then fetches the latest content from the backend API and displays it.

## Schema design

For my database, I kept things simple but flexible with these main collections ("tables"/"groups" stored in MongoDB):

#### Post collection

```ts
export interface Post {
  _id: string;
  title: string;
  content: string;
  body: string;
  category: string;
  date: string;
  updated_date?: string;
  cover_image?: string;
  tags?: Tag[];
}
```

#### Tags collection

```ts
type Tag = {
  _id: string;
  tag: string;
};
```

## Planning Process

Though I didn't create wireframes, I had a good idea of what I wanted. I wanted to make my website look aesthetic but clean, focusing on simplicity and functionality.

I approached the development through phases:

1. Finish building the frontend for the main website, then the frontend for the CMS
    
2. Build the backend API & set up database structure
    
3. Connect the frontend for main website & CMS to the backend
    

In the next post, I'll dive deeper into how I built the backend and CMS components, including code samples and implementation details.

Thanks for reading!