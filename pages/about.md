---
layout: page
title: About
permalink: /about/
weight: 3
---

# **About Me**
<h4>Hi, I am <strong>{{ site.author.name }}</strong> :wave:,</h4>
<p></p>
<h5 class="text-justify">I am a second year Master's student studying Computer Science at The Ohio State University, currently looking for full-time opportunities as a Software Developer. I previously worked at Nutanix for 2 years where I expanded my knowledge about compute, storage and network infrastructure. I believe that if you never stop learning and communicate your knowledge effectively, the world is your oyster. </h5>

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
