---
layout: post
title: "Helping Asset Pipeline Open Up A Little"
date: 2013-10-26 14:42
comments: true
categories: 
---
A working Asset Pipeline can be a thing of beauty. A broken asset pipeline can be a horribly mean shut-in. One of my favorite ways for it to break in my Rails 3 app is the:
	undefined method `directory?' for nil:NilClass
	rake aborted

Thanks AP, for breaking and basically telling me nothing. Luckily, it doesn't have to be this way. If we take a moment to stop and get to know AP, we can get it to open up and be more helpful to us. We start by setting our bundler editor in .bashrc by adding the following line:
	export BUNDLER_EDITOR=subl   #I set mine to sublimetext

Next up, we hop into terminal and go for an in home visit right into the code itself:
	bundle open sprockets  #sprockets is AP's birth name

Navigate to lib->sprockets->base.rb

find your way to the "def each_entry(root, &block)" section of code and you'll likely see a lack of error handling there. To fix it, I wrap a rescue around the "directory?" check like so:
	begin
        if stat(path).directory?
          each_entry(path) do |subpath|
            paths << subpath
          end
        end
        rescue
          puts "Hey friend, I have an issue at: #{path}"
        end
    end

This doesn't fix all of AP's problems, none of us are perfect, but it does coax the gem into letting us know where the problem is. Once we know where the problem is, we can work on it together rather than just throwing our arms up in the air and quitting.