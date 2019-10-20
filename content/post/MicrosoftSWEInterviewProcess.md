---
title: "Microsoft SWE Interview Process"
date: 2019-10-20T10:56:03-04:00
draft: false
toc: false
comments: false
categories:
- Cloud Development
tags:
- Azure Storage
---

This post details my experiences during the interview process for a Software Engineering position in Microsoft's Azure Storage team.
<!--more-->
First off, since I already work for Microsoft, this was an internal interview process, so your mileage may vary. The end result of this process was that I was offered a position as an entry-level Software Engineer (SWE)- not bad considering I only have exactly 1-year of programming experience that does not constitute as consulting and Microsoft SWEs are no-joke, hardcore programmers in my opinion.

I first reached out to the manager of the Azure Storage team because I heard they were local and developing my company's cloud would be something incredibly cool for me to do since this would benefit the world as a whole. The manager was incredibly cool and patient, and took me seriously: he immediately set up an interview for me with one of his senior developers to test my mettle.

During the interview, I was asked, "Given a string, find the longest substring in the string where the first and last characters are the same." Simple enough problem. I first found the worst solution so I had something to fall back to in case I could not find the most optimal solution. In the end, though, I was able to at least describe the most optimal solution since time had run out to finish coding it because we had a good discussion at the beginning of the interview. The optimal solution was to use a dictionary (or hashmap, but I interviewed using Python) and store the first and last positions of each character that appeared in the string; from there you subtract the last and first positions of each character to find the longest string, outputting the substring that would be the first alphabetically.

After passing that interview, I was told the recruiter would reach out to me. A few months passed and I reached out to the manager to see what was going on. He was surprised the recruiter had not reached out, as he thought I had ghosted the whole interview process. He told me he'd reach out to the interviewer again to set up the final round of interviews. After a few weeks and continuous prodding, he was finally able to get the recruiter to set up the last round.  really appreciated how much he went out of his way to get this whole interview process to occur - I am eternally grateful that the One Microsoft attitude really exists and I have always received help within the company whenever I have asked for it (and even when I haven't)!

The final round consisted of four back-to-back technical interviews; needless to say, by the end of the day I was exasperated. The first interview asked me to reverse a number. I used Python trickery to get a quick solution, but of course he wanted it the traditional way because cheese is only fresh for a few days. I was able to get the remainders of each iteration, but I was stuck on how to add the remainders up. I thought you should store each of them in an array and then calculate the reverse number when the last remainder was computed, but he hinted that I only needed to add them to one integer and multiply the remainders to the result the opposite way I calculated them. We then discussed how you would prevent overflows if the numbers were too big, etc. I can't believed I needed that much help for suck a simple problem, but the next two interviews went a lot better.

The second interviewer asked me to design a system that would allow for proper authentication measures given certain environmental conditions. After creating a sequence diagram and describing how the system would work, we discussed what working on her team would be like. I really appreciated her taking the time to discuss how my daily life on the team would be like and it made me really hope I passed the interview process.

The third interviewer was the lead programmer, a really chill dude with a lot of patience. He first asked me to do what the senior programmer in the first round asked me to do, "Given a string, find the longest substring in the string where the first and last characters are the same." I already told him that I was asked that problem and told him the solution, and he thanked me for my honesty. He then asked me to determine how to ensure that a string given only a sequence of open and closed brackets and parentheses was balanced. He then asked me another question that was on the same level of difficulty, both of which I answered in ways that satisfied him.

The final interview was kind of hard, as it was remote and the first technical question was confusing after the first part was implemented. Needless to say, she understood my thought process after I wrote out example outputs and helped me find two bugs in my code after I confusingly added the two additional features she wanted me to add. She asked another question pertaining to what the algorithm would be to use the fewest possible moves to move objects into their sequential orders, which I found the least optimal solution for but did not have enough time to think of the optimal solution. She said I did well and appreciated how I provided examples to show which parts of the first question I was confused about.

After the final interviewer, the manager thanked me and escorted back to our office's main lobby. Three days later, he told me that the team liked me and would be glad to have me if I were to take their offer. Overall, I was definitely satisfied with the interview process as a whole and I really appreciate the time and effort my coworkers put in to interview me.