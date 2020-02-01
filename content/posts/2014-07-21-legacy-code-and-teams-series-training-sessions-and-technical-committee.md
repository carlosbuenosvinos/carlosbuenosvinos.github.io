---
title: 'Legacy Code and Teams Series: Training Sessions and Technical Committee'
author: Carlos Buenosvinos
type: post
date: 2014-07-21T07:49:58+00:00
url: /legacy-code-and-teams-series-training-sessions-and-technical-committee/
categories:
  - 'Agile &amp; Craftsmanship'
  - PHP
  - Talks
tags:
  - kpi
  - legacy code
  - legacy teams
  - technical committee
  - training sessions

---
(Next: <a title="Legacy Code and Teams Series: Composer" href="http://carlosbuenosvinos.com/legacy-code-and-teams-series-composer/" target="_blank">Legacy Code and Teams Series: Composer</a>)

When talking with some friends about how we are improving Atrápalo legacy code and development process, they asked for creating a presentation and give a talk about the steps we are following, hows, whys, and so on. It sound to me interesting so I would like to help sharing my experiences, not just the Atrápalo ones because when working at Emagister we did something quite similar with Christian, Eber, Dario, Lluís, Jordi, Jose Luís and the rest of the team with great results.

Before jumping into a talk, I would like to start a serie of posts about my way dealing with legacy projects. I&#8217;m really interested in listening from you and your comments in order to enrich the final result. So, any suggestion or experience is really welcomed.

<!--more-->

In this posts, I would like to talk about how to properly use composer, hexagonal arquitecture, TDD, continuous integrations, framework migration strategies, deployment, etc. in a legacy project without pain. Today, I want to share **Training Sessions** and **Technical Committee** practices.

### **Training Sessions**

As CTO, you have a technical roadmap in your mind in order to improve code quality, development speed, etc. However, you need to **make it happen**, the most difficult part. In order to make it happen, you need to define a policy about coding, deploying, infrastructure, specific libraries, etc., communicate them and guarantee that every developer has the proper skills to do it right.

How can it be done? I use **training sessions**. At Atrápalo, every Friday, from 13:00 to 15:00, no excuses, we train all of the team about the next technical challenges, changes, libraries, methodologies or processes we are introducing to improve quality and speed. Those training sessions are purely based on the roadmap except for those on holidays with most of the team is on holidays. When that&#8217;s the case, we teach about other technologies just for fun. If a change it&#8217;s a bit difficult, we spend more than one session.

If a specific change that may affect everyone has been introduced during the week, the owner explains it to the rest of the mates before the official training session starts.

So, my first recommendation to deal with legacy code, it&#8217;s not about code. It&#8217;s about people and their skills. Luckily, after I started doing this exercise at Emagister around 4 years ago, I have heard about more PHP teams practicing it at Barcelona.

### **KPIs and Technical Committee**

&#8220;You cannot improve what it&#8217;s not measured&#8221;. So, let&#8217;s have some KPIs to keep in track for checking that all the improvements added to the team and the code work well. Create a Google Spreadsheet with KPIs such as performance, bugs, slow queries, etc., share it between all the technical committee members and review them weekly. If you want to focus in some aspect, just add a KPI, when everything is under control, you can just removed. There are some KPIs just for control, such as traffic or number of servers. You can find it there is a problem based in traffic getting higher.

Each KPI, it&#8217;s assigned to a different member and its responsibility is to fill the numbers, find why is degrading and identify ways to improve it. Technical Committee also reviews the technical roadmap adding new tasks or changing the order based on the team and company interests. If during the day-by-day development something is not working properly (architecture, branching methodology, etc.) any developer can send an email to the committee to discuss the problem, propose a solution and execute it.

Choose all the members between people in the team that has skills and knowledge to improve the roadmap and run some tasks. Switch members every quarter in order to make everyone participate in the process.

Members take tasks from the technical roadmap and execute them during their sprints. If tasks are too big, we plan them as a User Story in any of the Backlogs of the teams.

The goal of this practice is keeping the technical debt under control and execute actions to make your team perform even more removing blockers in the methodology, development process or tools. Think about it as a technical retrospective.

### Sum up

Build your technical committee, decide and assign your KPIs, create tasks to improve them, bring everyone on board, make the tasks happen and train your team weekly to become better than they already are.