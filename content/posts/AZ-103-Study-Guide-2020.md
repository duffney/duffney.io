---
author:
  name: "Josh Duffney"
date: 2020-01-02
linktitle: AZ-103 Study Guide 2020
type: post
type: posts
toc: true
title: AZ-103 Study Guide 2020
aliases: 
- /AZ103-StudyGuide
tags: ["Azure","AZ-103","Study Guide","Certifications"]
categories: ["how-to"]
---

I recently passed the AZ-103 Microsoft Azure Administrator Associate. I studied for around six weeks for the exam and in this post I’ll cover all of the resources, systems, and routines I used to study for the exam. Prior to studying for this exam I had very little Azure exposure. 

I’d blindly click around in the portal before finding information for people, but that’s about it. Studying for this exam was a lot of fun and I learned a ton. It is something I wish I had done two years ago. If you have a good background in systems administration, all the learning required for this exam is Azure focused. It does assume you have a good systems administration foundation. For example, you’re expected to understand DNS and how subnetting works. 


# Commit & Find Study Material

I have a lot of people that work with Azure in my social networks, so I asked a few questions. I first wanted to gauge the difficulty of the exam. Basically I wanted to know if this was anything like the CCNA. 

The answer was a stress relieving, no. I then began to ask what they thought was adequate time to study before the exam. Most said four weeks, so I scheduled it. Scheduling it put my feet to the fire. After scheduling the exam I asked the same group of people for recommendations on what resources I should use to study. Microsoft Learning modules came up a lot and after looking into them, it seemed to be a good choice and it is free!

---
**Tips**

* Schedule the exam 4-6 weeks out.
* Research study material.
* Curate excellent study material.

---

# Design a Study Strategy

At this point I had the exam scheduled and I had training material. My first goal was to just get through as much per day as I could bear and hope I made it through the training material by the time the exam came along. This strategy worked for about two days and I already felt like giving up. As you’d expect, day one was something like four hours of study and on day two I got through maybe thirty minutes. I had already burned myself out. This was a poor strategy. I never knew if I was going to make it through the training and I never felt like I studied enough even if I studied for four hours. I needed a better strategy.

I decided I was going to plan out my study hours. To design that, I first added up all the hours of the Microsoft Learning path modules, which totaled around sixteen hours. I then committed to at least one hour of study each day. It would take me roughly sixteen days to get through the learning modules. I then used PowerShell to see how many days I had left until my exam. New-TimeSpan -Start '11/01/2019' -End '12/05/19'. The result of that was thirty four days. Subtracting the time until the exam and the time required to complete the learning modules I got eighteen days. I had an eighteen day buffer. I already felt better and now I knew how much study was enough. I just needed to get through one hour per day, which to me seemed doable.

---
**Formula**

( Training Hours Total) / ( Hours of Study per day )  = Days to complete Training

Example: 16 / 1 = 16 days

(Days before Exam) - (Days to complete Training) = Remaining time before exam

Example: 34 - 16 = 18 days

---

## Study Routine

I knew what to do and for how long, but I still ran into problems with execution. I’d try to study for an hour in the morning after the gym and my son would wake up early and want to play or have breakfast. I’d try to study over my lunch break and I’d get pulled into something at work or would need to help out at home. I was still getting some studying done, but getting interrupted meant I was not absorbing as much as I could. There was a lot of context switching and that hour I needed prior, turned into an hour and a half to two hours just to get the labs set back up. I also realized that it took longer to get through the labs than estimated. I needed around two hours of study per day, not just one. Where was I going to find this time?

At this time I was reading a chapter in Atomic Habits about implementation intentions. Implementation intentions is one tactic you can use when building new habits. I decided I’d use this to build a habit for my studying. To do this you need to define a date, time, and place for the new habit. After reflecting on my daily schedule, there are two times in the day that I have a very low chance of being interrupted and normally have enough energy to focus. My two implementation intentions became: I will study for at least one hour everyday at 4:30am in the kitchen and I will study for at least one hour everyday at 1:00pm in my office. At first it was difficult, but anything is possible with coffee. 

Using these two times to study worked wonderfully for me. I was completing one to two learning modules per day. After two and a half weeks I completed all the learning modules that were recommended for the AZ-103 exam. I had learned a lot and was really enjoying learning Azure. Overall I was feeling really good about the exam, until I took a practice test.

_Questions to ask yourself:_

* At which time(s) am I least likely to be interrupted?
* Will I have enough energy to focus at these times?

---
**Tips**

* Create studying implementation intentions.
    * Example: I will [ Study ] for at least [ Time ] at or in [ Place ].
* Commit to a fixed amount of study hours per day.

---

## Find Gaps & Readjust

I’ll start by answering the question “Why bother with practice tests?”. It’s a fair question, but to me, opting out of practice tests is somewhat like reading all the chapters of a math book and not doing the exercises or worksheets before you take the math test. You might have the knowledge, but are not prepared to be asked about it. Practice tests provide more than just testing your knowledge, they teach you how to think through questions and problems. They are also great at finding the gaps in your knowledge. 

So what happened with my first practice test? I got a 20%, that’s what happened. I was immediately struck with fear, uncertainty, and doubt. A lot went through my mind. Should I cancel the exam? Should I focus on AWS again? Why am I doing this? Is this worth the time investment? But I had made my decision and I needed to stay committed. I decided to ignore those thoughts and continue to study. I took a few more practice tests and did better, but not well enough to pass them. I had some significant gaps in my knowledge, mainly around Azure storage and Azure Active Directory. Which made sense, as they were lightly covered in the learning modules I worked through. The bottom line was, I didn’t have enough general knowledge in two of the five content domains. I needed to readjust. 

<p align="center">
   <a href="https://twitter.com/joshduffney/status/1198975594605764608?s=20">
  <img width="514" height="433" src="/img/posts/AZ-103-Study-Guide-2020/selfDoubt.png">
  </a>
</p>

I started to readjust by calculating the remaining time I had before the exam. I had roughly two weeks left. Really, I had one week because I wanted the week before my exam to be left for me to focus on practice tests. Knowing my time constraint, I went out looking for new content to study the AZ-103. I was able to find several great resources such as Exam Prep books, Udemy, and Pluralsight learning paths. The problem was the time commitment. I knew I wouldn’t be able to read a book in a week. The Pluralsight learning path was forty one hours, so that was out. Udemy had too many courses to go through to make a good call, so I nixed that. 

I then remembered a [CloudSkills podcast with Nick Colyer](https://cloudskills.fm/043). Nick’s AZ-103 course was mentioned several times in the episode so I went looking for it. Luckily for me it was listed in the aCloudGuru library, which I had a subscription for. The course duration seemed reasonable, at nine-ish hours, but what stood out to me was the layout of the course. The course has a fantastic progression to it. Another reason I chose a video course was I had only been doing hands-on labs up to this point and I knew I was reaching my attention span with that learning medium. I needed to switch it up if I was going to make it through another nine hours of content. 

After I completed a couple learning modules with Nick Colyer’s AZ-103 course, I recalculated the time until I took the exam. I didn’t have enough time to complete the course and do all the practice tests I wanted. I knew I had to reschedule the exam. According to the numbers I should have pushed it a week out, but I didn’t want all the knowledge I had gained through the hands on labs to dissipate. I decided to move the exam out four days and ramp up my studying to three hours per day for the next week. Because I waited until a week before the exam to reschedule, it cost me 20 bucks, a penalty worth paying.

---
**Tips**

* Use practice tests to discover your gaps.
* Recommit to your decision to take the exam.
* Reschedule as a last resort, but do it when necessary.
* Utilize multiple learning mediums. 

---

## The Week Of

All I did the week of the exam was take practice tests. The first thing to realize about practice tests is they are only a valid assessment of your skills on the first attempt, and I only had five. I also knew I could only retain so much from them per day. Practice tests have a much higher cognitive demand than video training or hands-on labs. 

I, however, was comfortable just spending thirty to forty minutes studying per day the week of the exam. I felt I needed to do more to address the questions missed on the practice tests, so I developed the following strategy for practice tests:

1. Take a practice test in the morning
2. Evaluate missed questions:
    * If I truly did not understand the question, open a browser tab to research it.
    * If it was a simple mistake or misunderstanding of the question, just review it.
3. Retake the practice tests by the end of the day.

It’s worth mentioning that I would periodically research missed question during the day.  After a practice exam, the last thing I wanted to do was review every single missed question. I also didn’t have that kind of time. However, grabbing one question here and another there throughout the day worked really well. 

Even with this strategy in place, I had a strong desire to cram for the exam. I just had a pretty hard week of study prior, with three hours a day and knew I needed to back off or I was going to burn out before the exam and fail, due to lack of mental energy. I decided to push back on the desire to cram and just focus on preparing for the exam with practice tests. As I started to do better on the practice tests, I felt more confident and I didn’t feel like I needed to cram anymore.

The day of the exam, I decided to do something radical, nothing. I had my exam scheduled for noon and just felt that whatever I’ve learned by now is either good enough or not good enough and soon I’ll find out. I realized the best thing I could do was to relax and let my mind sort all the stuff I had just learned. Throughout the exam and even after, I was uncertain if I had passed or not. I got some questions I knew and many I felt like I winged it on. I completed the exam thinking I had a 50/50 shot of passing. Several hours later I checked my exam status again and it was set to PASS!

<p align="center">
   <a href="https://twitter.com/joshduffney/status/1207250160692146176">
  <img width="625" height="441" src="/img/posts/AZ-103-Study-Guide-2020/howtousePracticeTests.png">
  </a>
</p>

---
**Tips**

* Scale down your study the week of the exam.
* Split up the research of missed questions. 
* Do very little the day of the exam. 

---

# Suggested Study Schedule & Resources

Below is my suggested study schedule. If you bothered to read the blog post up to this point you’ll notice that this is different that what I actually did. The materials are the same but the order is different. In retrospect, this is how I would have prepared for the exam. Feel free to change the order to fit your learning style or to mix video training with labs.

---
**Weeks 1-2:**

_Video Course:_ AZ-103 Microsoft Azure Administrator 2020 by Nick Colyer

_Duration:_ ~8 hours

_Time Commitment:_ 1 hour per day

_Resources:_

- [aCloudGuru AZ-103 Microsoft Azure Administrator 2020](https://acloud.guru/learn/az103-azure-administrator)

- [Skylines academy Microsoft AZ-103 Certification: Azure Administrator 2020](https://courses.skylinesacademy.com/p/az-100)
---

**Weeks 2-5:**

_Hands-On Labs:_ Microsoft Learning Path Modules AZ-103

_Duration:_ ~34 hours

_Time Commitment:_ 2 hours per day

_Resources:_

- [Administer infrastructure resources in Azure](https://docs.microsoft.com/en-us/learn/paths/administer-infrastructure-resources-in-azure/)

- [Manage resources in Azure](https://docs.microsoft.com/en-us/learn/paths/manage-resources-in-azure/)

- [Administer containers in Azure](https://docs.microsoft.com/en-us/learn/paths/administer-containers-in-azure/)

- [Manage users and groups in Azure Active Directory](https://docs.microsoft.com/en-us/learn/modules/manage-users-and-groups-in-aad/)

- [Secure your cloud data](https://docs.microsoft.com/en-us/learn/paths/secure-your-cloud-data/
)

---
**Weeks: 5-6**

Practice Exams: 
- [Whizlabs Microsoft Azure Exam AZ-103 Certification Practice Tests](https://www.whizlabs.com/microsoft-azure-exam-az-103-certification/)

_Duration:_ ~5-8 hours

_Time Commitment:_ ~2 hours per day

<br>

---

<div align="center">
<p>"The Most <b>Actionable</b> Newsletter to Hit Your Inbox"</p>
<iframe src="https://duffney.substack.com/embed" width="480" height="320" style="border:1px solid #EEE; background:white;" frameborder="0" scrolling="no"></iframe>
<p><i>No spam. Just the best work I'm capable of.</i></p>
</div>

<br>