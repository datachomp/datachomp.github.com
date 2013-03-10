---
title: My subversion ignore list for Visual Studio / Anhksvn
author: Rob
layout: post
permalink: /archives/my-subversion-ignore-list-for-visual-studio-anhksvn/
dsq_thread_id:
  - 888404938
categories:
  - AppDev
  - Tools
---
# 

Disclaimer: I didn't write/create these exclusions...I copied them off [Stackoverflow.com][1] Sadly, I didn't bookmark the link and upon quick search, there were too many svn links for me to filter through.

 [1]: http://stackoverflow.com "StackOverflow.com"

**UPDATE:** Jim motivated me to actually spend the extra time to track down the actual links I used.  [Link1][2] and  [Link2][3]

 [2]: http://stackoverflow.com/questions/85353/best-general-svn-ignore-pattern "Link1"
 [3]: http://stackoverflow.com/questions/4375971/tortoise-svn-global-ignore-pattern "Link2"

I use  [AhnkSVN][4] as my visual studio source control plugin.  I right click on the solution,  select 'subversion',  select 'solution properties'.  That brings up a box and you will want to click 'add'.  From there,  select  svn.Ignore  and copy/paste the below:

 [4]: http://ankhsvn.open.collab.net/ "AnkhSVN"

Solution Level:

*.csproj.user Bin obj Obj Release debug Debug release

Note: I don't use Reshaper, if you do, there are other exclusions you will want to add

In [Tortoisesvn][5],  my exclusion list is :

 [5]: http://tortoisesvn.tigris.org/ "TortoiseSVN"

*.o *.lo *.la *.al .libs *.so \*.so.[0-9]\* *.a *.pyc *.pyo *.rej *~ #*# .#* .*.swp .DS_Store *.csproj.user Bin obj Obj Release debug Debug release *.suo

When you go into your TortoiseSVN settings,  on the 'general' tab,  copy/paste that into the  'Global Ignore Pattern:' box.