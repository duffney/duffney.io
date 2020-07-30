---
author:
  name: "Josh Duffney"
date: 2019-10-16
linktitle: You're an Engineer Be an Engineer
toc: true
type: post
type: posts
title: You're an Engineer Be an Engineer
aliases: 
- /YoureAnEngineerBeAnEngineer
tags: ["Career","Motivation","DevOps"]
categories: ["essays"]
---

Several weeks ago I got some feedback that caused me to pause and reflect. The two lines of feedback were; “I feel you always dig these holes of analysis paralysis and then jump in late.” And “You’re an engineer, go be one.”  To add context, these comments were made in a conversation I was having with a friend around how to best dive into Kubernetes. He is someone I look to when learning newer technologies. He dove into Azure, then to Containers, and most recently, Kubernetes. As he learned each technology, I meekly followed, trying to find the best ways to “learn” the technologies, which resulted in a very shallow understanding of each. I would gain a basic understanding and stop. Reflecting on the past few years, I can now understand his frustration with me. He was saying “Stop thinking, start doing”.

Had this feedback not come from him, someone I respect and knew had good intentions, I would have dismissed it. Even so, it wasn’t easy to digest. However, I couldn’t let my ego get the best of me. Such honest and candid feedback deserves some serious thought. This blog post is more or less my way of digesting that feedback.

## Maybe I’m a Printer?
In a conversation with a coworker of mine, we discussed how we had forgotten where our value as engineers comes from. Jaigene articulated it like this “Our industry hires people for specific skills when they’re hired. Just like how you’d buy a new printer. Does the printer have the features I want? Does this person have the skills I want?” I somehow let that infect me and I started to ask myself, “am I a printer?” Jaigene painted a much more colorful picture than I did, but I felt the same way. I felt stuck and even worse, I felt I was being left behind and my value as an engineer was starting to depreciate. Over the past year or so I seem to have forgotten my value isn’t in what I know, but what I can know, how quickly I can learn it, and the solutions I create from that knowledge. A common trap individual contributors fall into is defining themselves by their job and letting their job shape their career. We are not our titles, we are not defined by the lines on our resumes. It is not our employers responsibility to give us the opportunity to add new lines to our resume, that’s our responsibility. 

## Job != Career
Is there really a difference between your job and career? I have worked in IT operations for the past ten years and I’ve progressed through the various titles over the years. But what separates them from one another? Looking back, it’s easy to see. The answer is the investment I continually made in my career. My career is what separates them and is what has allowed me to progress. This is something I’ve known and taken advantage of previously. So what happened? Why was I asking myself “Am I a printer?” After some reflection, the best explanation I have is; I became unintentional about my learning and allowed the constraints of my job to dictate what I learn. But why did I allow this to happen?

Three years ago I started a position as a coveted DevOps engineer. During which my career goals and job goals were aligned. I had also achieved a monetary goal I set in my early twenties. Effectively, I had aligned three factors that lead to better performance and personal satisfaction; autonomy, mastery, and purpose.This was great, but it caused me to become complacent. Three years is a long time. I learned a lot of things, valuable things. But as we all know, the industry doesn’t stay put for long. As the industry moved forward and adopted new technologies, the complacency I had allowed to creep in, prevented me from redefining my career goals. Because of that I started to lose focus on mastery and more importantly I began to feel lost, discouraged, and left behind without purpose. 

For awhile I was in denial. I blamed my job for not presenting me with the opportunity to adopt these new technologies. I became increasingly unhappy when I’d push for such change or opportunity and it wouldn’t happen. Finally, I was brought to the conclusion that it’s not my employer’s responsibility to provide these opportunities. It’s my responsibility to keep a pulse on the industry and to make continual investments in my career. Even if there isn’t a direct application or project that requires knowledge of that technology at work. The other lesson I was able to learn from this is contentment is not the same as complacency. You can be content with where you are, but you still need to keep an eye on the path forward. This has been an extremely difficult concept for me to grok, it’s a daily struggle for me.


## Devote Yourself to Learning & Experimenting (The First 20 Hours)
Going back to the feedback I had gotten; “I feel you always dig these holes of analysis paralysis and then jump in late”- I discovered a pattern in my behavior. With all the new technologies that didn’t directly apply to my job, I’d do the following: Invest enough time to become vaguely familiar with the technology and then stop learning it because I couldn’t “apply” it… at work… I did this with learning Azure, AWS, and docker. The underlying problem was, I was not allowing myself the time to become competent. I was overwhelmed by what I didn’t know and I let that prevent me from continually learning. I had also convinced myself I couldn’t fully learn these technologies because I’m not a “developer”. Which I know now is just an excuse I used to make myself feel better. To my credit, I would eventually convince myself to learn some of the technology, but as was stated in the feedback, I’d jump in late. Not only did I jump in late, I’d  also jump into the shallow end and stay there...

After reflecting on all of this I was reminded of a book I read several years ago. The book is titled “The First 20 Hours: How to Learn Anything . . . Fast!” by Josh Kaufman. The book points out that you do not need 10,000 hours to learn a new skill. You need roughly 10,000 hours to completely master a skill. It then explains that it only takes around 20 hours of deliberate practice to become competent in something. Hearing this idea again gave me an AH-HA moment. I realized I didn’t need to devote endless time trying to learn these new skills. Spending 20 hours was a much more palatable investment of time. Armed with the 20 hour rule, I was able to give myself the freedom to experiment and learn. 

My biggest challenge with using the 20 hours rule to learn new things is becoming comfortable being “unproductive”. Learning new things is fun and exciting, but it’s also time spent that you could have invested elsewhere which might of been more “productive”. Productive in the sense of an existing project or task at work. In all honesty, this is something I still struggle with and the way I overcome it is to commit and focus on the strategy, not the tactics. Acknowledging that  this learning will greatly benefit me in the future does a lot to push the obsession of productivity out of my mind. 


## Learning Isn’t the Same as Practice

Another flaw I discovered in my understanding of skill acquisition is learning is not the same as practice. I would waste a lot of mental cycles researching the best learning material (my favorite are Manning books) and after much deliberation, jump in. Only problem was, I was learning, not practicing. I would read the chapters, take notes, and do all the labs. I would leave each chapter having learned something, but not knowing something. Frustrated that I wasn’t absorbing the knowledge, my willpower would eventually deplete and I’d stop reading the book.

Within the same conversation that sparked this blog post. It was recommended that I check out Traefik as an ingress controller for Kubernetes. Opening the url link, I felt completely overwhelmed and replied with “I’ll just go back to reading the book I bought.” I could feel a very dramatic eye roll as I awaited a reply. The reply was “You’re an engineer, go be one.”  I might not have been able to understand the content in the blog post but that wasn’t the point, the point was I could figure it out. That was the mindset I was missing. The gap I had in my new skill acquisition was deliberate practice and application. Would I get that from my job? No I would not. However, the key question I wasn’t asking was “why can’t I create them?”

I then started to seek out a project that I could use as application. I finally decided on TeamCity as an application to put into Kubernetes. I chose it because docker images exist for it already and I’m very familiar with the architecture. As I went through my Kubernetes book I applied the lessons there to my TeamCity project. It was magical! I began to truly acquire the knowledge I was seeking. I fall in and out of this now, but the phrase that keeps me on track is “Learn enough to practice, practice until you understand.” The best advice I can give here is to seek application through bluesky projects. Will I ever deploy TeamCity on Kubernetes at my day job? Unlikely. Is that going to stop me? No.

## Default Aggressive

Another piece of advice I’ve been given several times lately is to be aggressive. I’m still working through the application of this advice, but here is what I’ve come up with so far. Be aggressive in your pursuit of knowledge. Have an aggressive will to win. Do not allow daily constraints hold you back from what you want to do. Aggressively find avenues to apply the knowledge you’ve acquired and just because it hasn’t been done before or no one is asking you to do it, it doesn’t mean it can’t be done. On a more practical level, I’ve taken this advice to mean learn and learn as much as I can. Being aggressive to me means, waking up early before my day begins and opening a nearly 600 page book on Kubernetes and getting through maybe 10 pages in an hour. It means not letting the constraints of my job limit my learning of new technology. It means constantly battling the voice of imposter syndrome that says “I’m not an expert” with a reply of “No I’m not, but I will be.”

## Takeaways

* You’re an engineer, go be one.
* Adding new lines to your resume is your responsibility.
* Contentment is not the same as complacency.
* It only takes 20 hours of deliberate practice to become competent at something.
* Learn enough to practice, practice until you understand.
* Be aggressive, go after what you want. Don’t wait to be told.
* Community is important, find someone to push you and take action on their advice.
* Enjoy the process.
* Be decisive.


## Resources, Motivation, & Credits

### Books

[Be the Master Third Edition](https://leanpub.com/bethemaster3)

[Extreme Ownership: How U.S. Navy SEALs Lead and Win](https://www.amazon.com/Extreme-Ownership-U-S-Navy-SEALs-ebook/dp/B00VE4Y0Z2)

[The First 20 Hours: How to Learn Anything… Fast](https://first20hours.com/)

### WebSites

[Be the Master](https://bethemaster.com/)

[Cloudskills](https://cloudskills.io/)

### Podcasts

[Cloudskills Podcast](https://cloudskills.fm/)

[Jocko Podcast](https://jockopodcast.com/)

### Motivation

[JOCKO Willink - Time Is Running Out](https://www.youtube.com/watch?v=tOYgiWEoJgs&t=1s)

[Jocko Motivation GOOD](https://www.youtube.com/watch?v=IdTMDpizis8)

### Credits

Friend - Josephe Beaudry

Co-worker - Jaigene Kang

Influencers - Don Jones, Mike Pfeiffer

---

<iframe width="100%" height="450" scrolling="no" frameborder="no" allow="autoplay" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/playlists/983314672&color=%23ff5500&auto_play=false&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true"></iframe><div style="font-size: 10px; color: #cccccc;line-break: anywhere;word-break: normal;overflow: hidden;white-space: nowrap;text-overflow: ellipsis; font-family: Interstate,Lucida Grande,Lucida Sans Unicode,Lucida Sans,Garuda,Verdana,Tahoma,sans-serif;font-weight: 100;"><a href="https://soundcloud.com/joshduffney" title="joshduffney" target="_blank" style="color: #cccccc; text-decoration: none;">joshduffney</a> · <a href="https://soundcloud.com/duffney_io/sets/be-an-engineer" title="Be An Engineer" target="_blank" style="color: #cccccc; text-decoration: none;">Be An Engineer</a></div>
