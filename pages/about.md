---
layout: page
title: About
permalink: /about/
weight: 3
---

# **About Me**
<h4>Hi, I am <strong>{{ site.author.name }},</strong></h4>
<p></p>
I am a second year Master's student studying Computer Science at The Ohio State University, currently looking for full-time opportunities as a Software Developer.<br><br>
My current research is focused around optimizing the performance of GPU to GPU data transfers over Infiniband in Supercomputers to speed up high performance deep learning applications.<br><br>
I previously worked at Nutanix for 2 years where I gained expertise working with compute, storage and network infrastructure on Nutanix and VMware platforms.<br><br> 
I believe that the secret to a happy career is to never stop learning, to share your knowledge with your peers and to make time for family, friends and travel.

<div class="row">
{% include about/skills.html title="Programming Skills" source=site.data.programming-skills %}
{% include about/skills.html title="" source=site.data.other-skills %}
</div>

<div class="row">
{% include about/timeline.html title="Education" source=site.data.education-timeline %}
</div>

<div class="row">
{% include about/timeline.html title="Experience" source=site.data.experience-timeline %}
</div>

<div class="row">
<h2 class="mb-3"><a href="/projects">Check out my Projects</a></h2>
</div>
