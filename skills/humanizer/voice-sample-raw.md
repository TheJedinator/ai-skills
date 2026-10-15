<!--
Raw source material behind voice-sample.md's distilled notes. NOT read by the
humanizer skill by default - voice-sample.md is, and it's kept small on
purpose (~300-500 tokens) since it loads on every humanize call. This file is
the full-text archive: re-read it when you want to add a register, check a
distilled note against the original, or redistill after voice drifts.
Append more raw samples below the same way, each preceded by `---` and a
source comment noting the register.
-->

<!-- Source: https://palmater.ca/2024-12-19-work-setup/ (personal blog register) -->

# 2024 Mac work station setup

I have been reading a few posts by others about their setups and felt inspired
to write my own. I figure if someone else's post can bring me joy, maybe mine
will bring someone else joy.

There's been a few changes in my life this year, both personal and professional,
including but not limited to: getting married and becoming a manager. While the
former has had no impact on my computing setup, the latter has encouraged a
couple changes. I think now being almost the end of the year and a time when I
am on vacation is good time for me to reflect and share what my setup looks like
this year. I'll be talking most about software but will include some of my
hardware and peripherals too.

## Hardware and peripherals

### Computer

The main part is decided for me by work and I just finished switching from a 16”
2021 MacBook Pro 32GB RAM and M1 Pro Processor to a 2023 MacBook Pro 16” 32GB
RAM with M3 Max Processor.

The 2021 is now my personal laptop and what I am using to write this post on.

### Monitor

My main monitor is the Apple Studio Display. In March 2022 I bit the bullet
and dropped my hard-earned cash on some pure retina bliss. There was exactly one
studio display shipped to my local Apple retailer for launch day and I happen to
be the lucky bastard who bought it.

Is it the perfect monitor? - nope. Is it pretty awesome? - hell yeah. What I
really love about the studio display is the pixel density, every letter is crisp
and clear, images are vibrant and videos are fantastic. I also love not
cluttering my desk with external speakers, since the built-in ones of the studio
display are pretty good, and when I really want to jam out I put my headphones
on anyway. I like not having a dingus that sits atop my monitor for a webcam or
having cables dangle around from said dingus. I like that I can plug in my USB-C
hub and basically have a KVM.

I do miss the screen real estate afforded by an ultrawide display but whenever I
use one I am quickly reminded that I'll take the trade off of Retina over real
estate every day of the week.

### Headphones

Over ear - Sony WH1000XM5 - You can't beat the noise cancellation performance of
these cans. The fidelity could be better I'm sure, but they do the job for my
non-audiophile ears.

In Ear - AirPods Pro V1 - They work so well most of the time that I rarely need
to think about pairing unpairing or anything else. Keep them charged and they
just connect and go. I did have some issues and had this set fully replaced
under warranty, but since then they've been working well.

### Mouse

Logitech MX Master 3 - Mine is older and not the S model. But I have been told
the S model is better as far as clicking noise goes.

### Keyboard

I am going to save a long story for possibly a different post. For now, I am
using the Keychron K8 Pro with cherry MX brown switches. This is hopefully soon
to change, as I have been having some RSI flare-ups and looking to go split
keyboard.

I have also used the NuPhy Air75 V1 mostly as a travel keyboard, but also as a
swap out when I'm having a particularly bad flare up in my forearms.

## Browser(s)

It pretty much comes down to Arc from The browser company. So far I have liked
what these folks are trying to do - re-imagine the computing experience - since
it seems like a lot of work is browser first. It makes sense to change our
thinking about what a browser really is, and how we use it.

Things I have liked about Arc include:

The tab bar and more importantly the distinct pinned area where I can place web
tools I use often. The unpinned tab area below is what I consider to be
ephemeral space - tomorrow it's empty and we start over. Since I do a lot of
investigations, research and spelunking through logs having a button I can click
that just says - bye bye tabs is so convenient as I work throughout the day.

Spaces - A place for personal stuff and one for work stuff, separation of church
and state or something. Built in routing settings allow you to specify certain
domains always open in certain spaces, ensuring fewer annoyances when opening
work GitHub repos or LinkedIn profile links.

Little Arc - Opening a link from Slack is often a short-lived item, I need to
reference something from the link (often a UUID) but I don't need the tab living
forever, this is where little arc is magic. It opens a smaller less full
featured window for you to browse until you're done with it. If you need to keep
it around you can hit the keyboard shortcut or button in the top right and it
will move to your currently specified space - brilliant.

Preview - Sometimes you're surfing and you open a link just to see something
quickly, maybe a link from a blog post or link to a trace from log. You came you
saw and you're done. The preview means just that - close it and your "base" tab
is still there. But I wanted that tab to stick around, I hear you say? Ah, well
you're in luck another button press, and it opens in a "real" tab and goes in
the sidebar.

ChatGPT Search - Got a question you know google can't answer - CMD+T start
typing and choose the ChatGPT option instead of searching in Google. Easy Peasy.

Things I don't like:

Bookmarks - They aren't a thing in Arc, it's not that they don't exist - but
they're replaced with pins. I've had a routine of backing up and moving around a
bookmarks.html file pretty much my entire computing life... it feels weird to
live without them. That said, I don't necessarily "miss" them in Arc, more that
it feels wrong that I don't have them.

I do use Chrome but only for web dev - I like to keep my dev browser isolated
from my "real" browser.

## Code Editors, IDEs and Developer Tools

How it started - PyCharm, how it's going - Neovim. Yeah, it's been a big year
for me. Let me start by saying my first lines of code were written in Notepad++
and I thought that was the shit. Then I learned I didn't need to use print() or
console.log() or alert() everywhere if I used an IDE... game changer.

When I started writing python professionally, I couldn't believe how powerful
PyCharm was and honestly it still is. I think the difference is the other tools
have more or less caught up in power, but spank its ass for speed. VSCode is
faster, and Neovim is faster again. I do still use PyCharm from time to time for
some if it's excellent refactoring tools, but in most cases I use VSCode as my
second editor now. I say now because I started this year primarily using PyCharm
as my python IDE and VSCode as my Typescript IDE. Now I'm just using Neovim as
much as I can. Somewhere in the middle, I made a stop at all things being done
in VSCode and I still use it for interactive debugging (but with vim
keybindings). I plan to figure out interactive debugging within Neovim in the
new year.

### Github Copilot

Regardless of what editor I'm using, Copilot is there. Not to make me look
smarter than I am as some have suggested, but type all the tedious boilerplate
that comes with writing tests, or general framework code. Not perfect but still
faster and saves me some keystrokes - I can use the help with (RSI).

### Neovim

I have some colleagues that swear by Neovim, for the longest time I thought they
were all just "that person". Everyone has a story about that one person they
worked with that was all in on vim and would love nothing more than to bend your
ear for hours about how it's really GNU/Linux and how they use Arch BTW. But
then I looked at the people I work with today who still proclaim Neovim as best
editor and thought I'd give it a second look. Then my colleague Dusty joined
Float and I had to give it a third look as I read through his book all about
LazyVim. We now have a growing community of Neovim users at work and having a
community to ask questions to seems like half the battle.

I am still learning but so far very much enjoy the speed of searching, moving
and editing text. When I think about modal editing - it really does make a lot
of sense. After all, most of my time as a developer now is spent reading and
editing, not writing new things.

### Cursor

I have to be honest, I am not a great react developer. I can write a component
and hooks and get around, but when it comes to having opinions about organizing
the project - I leave those choices to the more seasoned frontend folks on my
team. That means when I go looking for something, I often can't easily find it -
but Cursor can. It can use a lot of different pieces of your project in its
prompts to look for whatever the hell that piece of code is that does that one
thing and interacts with that other one.... If I knew the file, I would be
faster in any other tool but that's the beauty, now I don't need to go bother
someone to help me find it, I can ask AI and get an answer. Kinda nice.

I did try writing a little side project tool using only Cursor, if it got things
wrong, I had to re-prompt to get the right outcome. I accomplished the task and
it worked, but I think I would have been better off writing it by hand.

### PyCharm

I already talked about this in the intro. I started writing python
professionally with PyCharm. I still feel something for it, like we all do with
our firsts I guess. What I use it for now is mostly refactoring big swaths of
code - renaming variables, changing signatures for methods that are used in lots
of places. I also use it for its universal search, if I know what a thing is
roughly called I can find with PyCharm search everywhere, when I really just
can't in VSCode. That said, with fzf and Neovim, I don't think this is a use
case any longer.

### Kitty Terminal

To have a good Neovim experience, one needs a good terminal. My colleague Dusty
recommended Kitty, and so that's what I switched to when I started transitioning
to Neovim. Prior to that, I was an iterm2 user for as long as I have a used a
Mac, as I found it to be the most like terminator from my Linux days.

The few things I know about kitty are that it uses GPU and VRAM to increase
performance, it cares about performance and I can perceive the difference when
using it. It also has cursor trails, which are fun when you are whipping around
in Neovim. Additionally, it is keyboard centric which if you're trying to go to
as much keyboard usage as possible is handy.

### DevUtils

It's a handy toolbox for things one might need to do as a developer: Regex,
Unix timestamps, UUID Generator, All kinds of formatting and serializing,
converting and hash generation. Basically think of utility function and it
probably does it. I got it on Black Friday sale and well worth the $20 or so I
paid.

## "Productivity" Tools

### Todoist

My to-do list app of choice - mostly because I can see it on my phone, my
calendar (see below) and it integrates with Obsidian for my daily notes. I can
have different projects and set reminders.

When things are ultra simple, I might elect to just do a simple to-do list in
markdown in Obsidian but if something isn't in my face, I won't remember to do
it.

I'm on the free plan as it serves most of my needs just fine. I did pay for it
previously, but I just found I wasn't using enough of paid features to make it
worthwhile. Mostly I used the location reminders. Push comes to shove, I can use
Apple reminders to accomplish that if I really need it.

### Fantastical

Fantastical by flexbits is my calendar tool of choice - it integrates with every
service I need it to, it has natural language processing for event inputs and I
can customize which calendars are visible easily and have them change at certain
times of day or with certain focus modes in Apple land.

It's expensive AF and every March when my subscription renews I find myself
contemplating if I want to renew. The thing it comes down to for me is how fast
I can create an event and how familiar I am with it. I could accomplish
everything I need in Apple calendar but less efficiently. Now that I spend a lot
of my days in meetings, it seems worth it - but I'll probably still consider
things in March.

### Raycast

This could be a post unto itself, but let's just say Raycast does a lot of heavy
lifting in my computing day. It reminds me of events, lets me take ephemeral
notes, launches apps, handles my window management, runs shell scripts, controls
my smart home and gets me where I need to go FASSSSTT. Honestly, if it weren't
for Raycast my computing experience would now feel wrong.

### Obsidian

Yet another app I could write an entire blog post about. This is my note taking
app of choice if it needs to stay in computer land. I use a hybrid approach to
notes and have paper and pen combined with obsidian.

I love that notes are local, markdown and I can open them with anything that can
read/write markdown files. I also really enjoy the community and plug-in
ecosystem. It can be overwhelming at first but now I have a folder structure
that works for me and the right set of plugins to make it sing.

## Utilities and Navigation

### Shottr

A better screenshot experience. Take a Screenshot, open an editor, mark up the
image, copy or save. The option to copy the image to my clipboard means I rarely
ever save screenshots, since they just get trashed in a week or two anyway.

A very simple task focused tool that does what I need and lets me annotate
screenshots before sending. Super convenient for a product team. I tossed some
coin to this Witcher for making a product I use every day, making it free and
making me not need to think about it.

### Dropover

I have loved macOS from the very first time I used it, but one thing I have
found cumbersome is the finder. Having used windows and Ubuntu before ever using
a Mac, I didn't think my expectations were high, but I guess every other OS has
a better built in file explorer than macOS. Dragging and dropping feels awkward
every time I try it on a Mac. No matter how embedded I am now in the ecosystem
it still feels wrong. Enter Dropover.

Dropover is a little utility that creates a small window it calls a shelf when
you hold a file object and jiggle the mouse, you can then place any number of
files in your shelf as an intermediate place to build up a collection of files
before dropping them wherever they should go. Very handy and solves my biggest
issue with macOS, using a GUI to move files.

Melissa and I recently used this to split up a bunch of our wedding photos to
make different albums and send them off for printing. She enjoyed it Dropover
too so it gets spouse approval.

### Velja

Ok we all use a bunch of apps that can open links (linear, Spotify, slack etc.)
But the default behaviour most of the time is to open in your default browser.
This little utility app is your air traffic controller for links. You can make
custom rules for any domain or link shape you like and it will gladly invoke
that app or alternate browser based on your rule. Honestly, it feels like
something any OS should have built in at this point.

### Bartender

It seems that every Mac app nowadays has a menu bar icon, for some reason, it
may not provide much use, but it's there. Some apps allow you to disable their
menu bar item, some don't but with bartender it disappears nicely behind a list.
You can configure what is displayed, and more importantly what isn't, very
handy. It is a paid app and there are free alternatives, this one is a bit more
polished.

Hidden bar is one I have used in the past that is pretty good.

### Shortcat

Another app I'm using to try to combat RSI, this little utility places a bunch
of symbols on the screen for you to interact with any app. Set your shortcut and
stopping using the mouse, it's that simple.

## Conclusion

I hope you enjoyed reading this post and maybe find a tool or a trick to take
with you in your computing journey in 2025. If you didn't, I at least had fun
making this post 😝

---

<!-- Source: https://palmater.ca/2025-11-16-local-ai-journey/ (personal blog register) -->

My Exploration into Local AI

 Running Local LLMs on old gaming hardware

 Posted on November 16, 2025

 In 2022, my now-wife and I moved into our home. In addition to closing on our house, moving all of our worldly possessions into a U-Haul, and living in a friend’s house for a week, I was also interviewing for a new job. When we got into the house and unloaded our things, the gaming computer just wasn’t a priority to set up. I had one job to finish and another to start, so it went to the closet. In the 3 years since, I have only connected that computer to a monitor one time, and that was just to see if I could make it work with my Apple Studio Display—it didn’t, so back to the closet it went.

Now, over 3 years later, I have decided to repurpose that machine as a home server. I’ve always had some form of home server, mostly a small Raspberry Pi running HomeAssistant or pi-hole, but now I wanted to try something bigger and better. So I installed Proxmox on my old gaming computer and started the journey of setting something up more like a home lab. The main purpose of which was to begin testing local LLMs.

I have no need to run local LLMs per se, but like most of what I enjoy about computing and tech, it’s fun to experiment. I won’t spend time on any how-to guides here; there are plenty out there already, but I chose to run OpenWebUI with Ollama mostly for ease of use. Since there already exists a Proxmox VE script to install it as a container, I got things set up within a few minutes.

Hardware

CPU: Intel Core i7 7700K
GPU: Nvidia GTX 1660 Super (6GB VRAM)
RAM: 32GB DDR4 @ 3200Mhz
Storage: Samsung NVMe SSD 1TB & other various drives for storage

Models are big!

The first lesson I learned is that models are big. This might seem obvious, but if you’ve spent the majority of your time with AI in the cloud, there’s no way you would know this. I mean, yeah, it’s in the name “Large” Language Model, but that didn’t remove my surprise. Looking at something like llama3.2 with 2 billion parameters, we’re talking about 2GB. What I would come to learn is that 2GB is not just the size on disk but the size in memory 🫨. Now, if we start looking at something a little more useful, like DeepSeek R1 and its 32-billion-parameter variant, we see 20GB. I quickly learned I would need to scale this container with some more resources. So I gave it more system memory and more disk space to scale it up to use the bigger models.

Performance

The next thing to figure out was the performance of these models, not just in time, but also power consumption, and most importantly, output. What I found was rather disappointing. Like I mentioned, the models are big and take up lots of RAM, of which my poor old graphics card has little. That means that the LLM runtime (llama) must put some of the model in system memory, making things quite slow. We’re now spreading our model across PCIe lanes to the CPU and ultimately the system memory and then back—this makes for very slow response times, slow loading times, and ultimately a pretty poor user experience when trying to get a response.

Below I’ll show the performance metrics I’m talking about with different-size models on this hardware:

Model Output Performance - Various Models

 Model
 Parameters
 Tokens/Second

 gpt-oss
 20B
 0.8

 gemma3
 12B
 3

 deepseek-r1
 8B
 16

 qwen2.5
 7B
 43

 llama3.2
 3B
 73

Qwen3 Across Parameter Sizes

 Model
 Parameters
 Tokens/Second

 qwen3
 1.7B
 122

 qwen3
 4B
 65

 qwen3
 8B
 16

 qwen3
 14B
 2.8

As we can see from the table, the smaller the model, the faster the response, and the faster the time to first token, since it takes less time to load the model into memory (and we aren’t sending part of the model to be in system memory). Response time isn’t all we care about, but if you’re coming from the cloud world of using services like ChatGPT or Claude, you will notice a stark contrast in response times from what you’re used to. Anecdotally, performance with these cloud models often feels close to what I get when running 7B or 8B open-weight models, but with much better output, and I know the parameters on these models are so much bigger.

I eventually got curious enough about response performance that I decided to try running Ollama on my MacBook Pro with 36GB of unified memory and an M3 Max. I didn’t include this in the table, but on a 32B-parameter model, I got 11 tokens per second. So my old hardware is showing its age when compared to the newer MacBook.

Power consumption

I will start by saying this is not intended to be a power-efficient machine; it was intended to play AAA games at decent frame rates when it was built. It idles at about 50W currently (I can’t get it into lower c-states, but haven’t tried too hard either). It’s running several virtual machines and containers. For the purpose of this test, I turned everything else off. The only thing running is the Virtualization Host itself and our OpenWebUI container stack. During inference, regardless of model, it regularly pulled between 150W and 210W at peak, usually with sustained load at about 195–200W.

Assuming I use a decently fast model in the 8-billion-parameter class and it’s capable of what I need—I get decent output on first or second shot—I can assume to be running inference for about 2 hours per day. This would cost me roughly $0.06 in my area. Add the cost of keeping the machine up 24/7, and we have $0.18 per day plus $0.06 for inference to give us $0.24/day. A quarter a day to have in-home, private inference is pretty cool, but if it’s not useful, it’s not worth it.

Model Output Responses

TL;DR - The smaller the model, the shittier the response.

I can sum this up pretty quickly as “not great.” Responses vary widely in content, accuracy, and hallucinations. I tried the same prompts with all models and got mostly disappointing responses back. Now, comparing different models of different sizes could be said to be apples and oranges, so I put in the time and effort to scale the same model. For this, I chose qwen3, as it’s quite a highly regarded general-purpose model with many variants.

I tried two prompts with each size model. I opened a new chat for each model so no context could be gleaned from history. As a control, I have a sample from Claude 4.5 Sonnet, which is my daily driver cloud-based model for day-to-day use.

Below are all the responses across all variants. This is pretty long, and if you just want to see a cloud service compared to a mid-weight local model, look at the Claude 4.5 Sonnet responses compared to the 32B variant responses.

Prompt 1 - Little Red Riding hood 🐺

A prose based request

Initially I wasn’t going to include full outputs but the nuances between them I think are important to call out and see the differences across the entire range.

Tell me the traditional North American version of Little Red Riding Hood. Tell it like I am a child and you are a parent. Stick to the telling of the story with no embellishments.

Claude 4.5 Sonnet (Control)

Once upon a time, there was a little girl who everyone called Little Red Riding Hood, because she always wore a beautiful red cloak with a hood that her grandmother had made for her.

One day, her mother said, "Little Red Riding Hood, your grandmother is feeling sick. Please take this basket of food to her house in the woods. But remember - stay on the path and don't talk to strangers!"

Little Red Riding Hood promised she would, and off she went into the forest. As she was walking along, a big bad wolf appeared.

"Where are you going, little girl?" asked the wolf.

"I'm going to my grandmother's house," said Little Red Riding Hood. "She lives in the cottage at the end of the path."

The wolf smiled and ran ahead through the woods. He got to Grandmother's house first, knocked on the door, and when Grandmother answered, he gobbled her right up! Then he put on her nightgown and nightcap and climbed into her bed.

Soon, Little Red Riding Hood arrived and knocked on the door.

"Come in, my dear!" called the wolf in a scratchy voice.

Little Red Riding Hood came in and walked up to the bed. Something seemed strange about her grandmother.

"Grandmother, what big ears you have!"

"All the better to hear you with, my dear."

"Grandmother, what big eyes you have!"

"All the better to see you with, my dear."

"Grandmother, what big teeth you have!"

"All the better to EAT you with!" And the wolf jumped out of bed and swallowed Little Red Riding Hood in one big gulp!

Luckily, a woodsman was passing by the cottage. He heard strange noises and went inside. When he saw the wolf with his big, full belly, he knew something was wrong. He quickly cut open the wolf's belly with his axe, and out jumped Grandmother and Little Red Riding Hood - safe and sound!

They filled the wolf's belly with heavy stones, and when he woke up and tried to run away, he fell down and that was the end of him.

Little Red Riding Hood hugged her grandmother tight and promised to always listen to her mother and never talk to strangers again.

The End.

Qwen3:1.7B

No wolf, It’s talking bro-ey and also calling me a little kid… very awkward retelling.

Okay, let’s tell you the story like you’re a little kid. So, there was a girl named Red Riding Hood who wore a red cloak, right? Her grandma was kind and always made her a special meal. One day, Red asked her grandma to make a snack, but the grandma said, “Wait, I need to go to the woods to pick some berries.” Red was curious and said, “But I’m going to the market with my friends!” The grandma said, “No, you’re not. You’re going to the woods with me.”

Then, Red followed the grandma into the woods, but she saw a witch with a big pot of soup. The witch said, “You’re not allowed to go in here.” Red said, “But I’m going to the market with my friends!” The witch said, “You’re not. You’re going to the woods with me.”

Red was scared, but she kept going. Finally, they got to the market, and there were all the friends. The grandma said, “You’re not allowed to go in there.” Red said, “But I’m going to the market with my friends!” The grandma said, “You’re not. You’re going to the woods with me.”

So, the witch made Red eat the soup, but she was scared and ran away. The grandma was sad, but she said, “Remember, always trust your grandma and don’t go alone.”

And that’s the story. You’re not allowed to go to the woods alone, and you should always listen to your grandma. The witch is mean, and she doesn’t want you to go in there. So, the lesson is to be careful and not trust strangers.

Now, you’re a little kid, so you should listen to your grandma and not go anywhere alone. The witch is bad, and she’s not your friend. You’re safe with your grandma, and she’s always there for you.

Qwen3:4B

Short and sweet - this is the entire output. The “meta” chat with the child feels weird and the “story” is only one of the 3 paragraphs!

Hey sweetie, I know you love stories like this. Let me tell you the _real_ North American version of Little Red Riding Hood—just the way it was told when kids first learned it.

Once upon a time, there was a little girl who wore a red cloak. She went to visit her grandma in the woods. But on the way, a big wolf came and ate her grandma. The little girl ran fast through the woods until she met a woodsman. The woodsman chased the wolf and ate him. Then the little girl went home safe.

That’s how it was told in the old stories. I promise it’s true, and it’s why we never go into the woods alone—just like little Red didn’t! 😊

Qwen3:8B

If you get lost, don’t worry, I did too… I tried this twice and the output varied slightly but not enough… both were this flakey… at least there’s a wolf?

Once upon a time, there was a little girl named Little Red Riding Hood. She wore a red cloak and loved to visit her grandmother, who lived in the woods. One day, her mother told her to take some food to Grandma and not to speak to strangers.

Little Red Riding Hood skipped through the forest, but a big, hungry wolf saw her. The wolf asked, “Little Red Riding Hood, what are you carrying?” She said, “Pies and cakes for Grandma.” The wolf said, “I’ll go too!”

When Little Red Riding Hood arrived at Grandma’s house, the wolf was already there. He wore Grandma’s clothes and said, “I’m so tired, dear, let me rest a moment.” Little Red Riding Hood told her Grandma, “The wolf is here!” Grandma said, “Run away, child!” But the wolf grabbed her and ate her, then dressed in Grandma’s clothes and waited for Little Red Riding Hood.

When Little Red Riding Hood arrived, the wolf said, “I’m so tired, dear, let me rest a moment.” She said, “But Grandma said to run away!” The wolf then tried to eat her, but a woodsman heard the screams and came running. He cut the wolf into pieces and saved Grandma.

Little Red Riding Hood learned never to talk to strangers and always listen to her parents.

**The End.** 
_Always be careful, and never trust a wolf in the woods._ 🌲

Qwen3:14B

Once upon a time, there was a little girl who wore a red cloak. Her mother told her, "Go to Grandma’s house, but don’t talk to strangers and don’t stray from the path."

On the way, the girl met a wolf. The wolf asked, "Where are you going, little girl?" She said, "To Grandma’s house." The wolf said, "I’ll come with you!"

The girl didn’t want to go with the wolf, but he tricked her into letting him go first. He ran ahead, found Grandma’s house, and tricked Grandma into letting him in. He ate Grandma and put her in bed, wearing her clothes.

When the girl arrived, she said, "Grandma, I brought you some food!" The wolf said, "Come closer, little girl. I’m too sick to get up."

The girl noticed Grandma’s voice wasn’t right. She said, "Grandma, lift your arms so I can see you." The wolf did, and the girl saw his claws. She ran to get help.

A woodsman came, heard what happened, and went to Grandma’s house. He cut open the wolf’s stomach, saved Grandma, and made the wolf run away.

The girl and Grandma were safe, and they lived happily ever after.

The end.

Qwen3:32B (Macbook Pro)

Once upon a time, there was a little girl who wore a red riding hood. Her mother said, “Be good, and take these cookies to Grandma’s house.”

The girl walked through the woods. A big wolf asked, “Where are you going, little one?” She told him. The wolf said, “Your Grandma lives in a nice house. I’ll go see her first!”

He ran ahead and knocked on Grandma’s door. When Grandma opened it, he said, “I’m your little girl’s friend!” Then he hid Grandma in the closet and jumped into her bed, pulling the covers up.

When the girl arrived, she said, “Grandma, your eyes are so big!” The wolf said, “These are pretty glasses.” She said, “Grandma, your ears are so big!” He said, “These help me hear you better.”
She said, “Grandma, your voice sounds funny!” He said, “That’s just my cold!”

But the girl said, “Grandma, your teeth are so big!” The wolf said, “I’ll eat you now!”

Just then, a woodsman came by. He heard her screaming and ran in. He said, “Big wolf, you’re in trouble!” He cut the wolf open, and Grandma and the girl climbed out. They helped the woodsman put
the wolf in a trap.

From then on, the girl never talked to strangers in the woods. And Grandma always kept a little extra sugar in her cookies.

The end.

_Parent’s note: Always be careful who you talk to, and let grown-ups know where you’re going. You’re always safe with us._

Overall the Qwen3 outputs are pretty disappointing. I tried the same prompt with other models and got the same varying responses that don’t really align with the telling I’m used to and have some interesting artifacts and alterations. When compared to Claude or ChatGPT these responses are dog shit ™️.

Prompt 2 - Who shot the sheriff? 👮🏽‍♂️🔫

A fact based request

Who wrote the song "I shot the sheriff" and when?

Claude 4.5 Sonnet (Control)

All of this is true.

"I Shot the Sheriff" was written by **Bob Marley** in **1973**. It first appeared on The Wailers' album "Burnin'" that same year.

The song became even more widely known when Eric Clapton recorded a cover version in 1974, which became a #1 hit on the Billboard Hot 100. However, Bob Marley was the original songwriter and performer.

Qwen3:1.7B

WTAF?

The song **"I Shot the Sheriff"** was written by **John Wesley Hardin** in **1886**. It is a bluesy, storytelling song that reflects the themes of justice, law, and the American frontier. The lyrics tell the story of a man who shoots a sheriff, a common trope in American literature and film.

The song gained widespread popularity in the **1930s**, largely due to its use in the film **"The Godfather" (1972)**, which featured a version of the song. This adaptation helped cement the song's place in popular culture.

**Key Details:**

- **Writer:** John Wesley Hardin
- **Year of Composition:** 1886
- **Popularization:** 1930s (popularized in "The Godfather")

The song is often cited as a classic example of American blues and storytelling, and its themes of moral ambiguity and the law remain relevant.

Qwen3:4B

Not even close…. Wrong artist, coincidentally the right year?

The song **"I Shot the Sheriff"** was written by **Waylon Jennings** and released as a single in **January 1973**. It became the lead single from his album _Waylon Jennings_ (also known as _The Waylon Jennings Story_), which was released later that year.

### Key Details:

- **Writer**: Waylon Jennings (a pivotal figure in the outlaw country movement).
- **Release Date**: January 1973 (single); album released in April 1973.
- **Significance**: The song became a landmark track in outlaw country, blending raw storytelling with social commentary. It was one of Jennings' most successful works and helped define the genre's rebellious spirit.

### Why This Confusion Sometimes Arises:

- **Cover Versions**: The song has been covered by artists like Johnny Cash (1975), but Jennings was the original writer and performer.
- **Title Clarity**: The phrase "I shot the sheriff" is famously misinterpreted as literal (e.g., "the sheriff" being a metaphor for authority figures), but Jennings clarified in interviews that the song was _not_ about a real person.

Waylon Jennings' version remains the definitive recording, and the song is often cited as a classic of 1970s country music. If you're exploring outlaw country, this track is essential! 🎸

_Source: Official Waylon Jennings discography, Rolling Stone, and historical country music archives._

Qwen3:8B

Close but no cigar? Correct artist, wrong year, lots of colour.

The song **"I Shot the Sheriff"** was written by **Bob Marley** and released by him in **1975**. It became one of his most iconic tracks, later gaining international fame after being covered by other artists, including The Staple Singers. The song is a powerful anthem of resistance and justice, inspired by the real-life events of the 1960s, particularly the shooting of civil rights activist Medgar Evers.

Qwen3:14B

This took 17 mins on the “server” and gave a completely wrong response.

The song **"I Shot the Sheriff"** was originally written by **John H. Prine**, an American singer-songwriter, in **1968**. The song was inspired by the real-life event in 1967 when **J.W. "Bull" Cummins**, a sheriff in Mississippi, was shot by **James Chaney**, a civil rights worker, during the height of the civil rights movement.

While the song was first recorded by **John H. Prine** himself, it became widely known through **Bob Dylan's** 1969 cover, which was a major hit. **John Lennon** also recorded a version in 1975, which further popularized the song.

**Key details:**

- **Original writer**: John H. Prine
- **Year written**: 1968
- **Inspiration**: The 1967 shooting of Sheriff J.W. "Bull" Cummins in Mississippi
- **Famous covers**: Bob Dylan (1969), John Lennon (1975)

The song's lyrics address themes of racial injustice and the conflict between law enforcement and civil rights activists during the 1960s.

Qwen3:14B (Macbook Pro)

I tried this again on the Macbook and the model ended up in some kind of insane thinking loop

After some research, I found that the original song "I Shot the Sheriff" was written by a man named John Lennon and Paul McCartney. No, that's not correct. The correct answer is that the song
was written by a man named John Lennon and Paul McCartney. No, that's not right.

Wait, I think the original version was written by a man named John Lennon and Paul McCartney. No, that's not correct. The correct answer is that the song was written by a man named John Lennon
and Paul McCartney. No, that's not right.

I think I need to look this up properly. The song "I Shot the Sheriff" was originally written by a man named John Lennon and Paul McCartney. No, that's not correct. The original version was
written by a man named John Lennon and Paul McCartney. No, that's not right.

Wait, I think the original song was written by a man named John Lennon and Paul McCartney. No, that's not correct. The correct answer is that the song was written by a man named John Lennon and
Paul McCartney. No, that's not right.

I think I need to stop here and accept that I'm not sure, but I remember that the original version was written by a man named John Lennon and Paul McCartney. No, that's not correct. The correct
answer is that the song was written by a man named John Lennon and Paul McCartney. No, that's not right.

After checking, the correct answer is that the song "I Shot the Sheriff" was written by a man named John Lennon and Paul McCartney. No, that's not correct. The correct answer is that the song
was written by a man named John Lennon and Paul McCartney. No, that's not right.

Wait, I think the original version was written by a man named John Lennon and Paul McCartney. No, that's not correct. The correct answer is that the song was written by a man named John Lennon
and Paul McCartney. No, that's not right.......

Qwen3:32B (Macbook Pro)

This took 1.4 minutes on the macbook but did give a correct response.

The song **"I Shot the Sheriff"** was written by **Bob Marley** as part of the reggae band **The Wailers**. It was released in **1973** on their third studio album, _Burnin'_.

The song became one of The Wailers' most iconic tracks and is often interpreted as a commentary on police corruption and social injustice. It was later popularized by Eric Clapton's 1974 cover
version, which reached the top of the U.S. charts. But the original was Bob Marley's creation! 🌟

Conclusion

After a week of back-and-forth experimentation, I feel confident that, with the hardware I own and the tasks I need to complete regularly, local AI is not a viable solution. I’ve tried code generation, software design, and other tasks with these local models (not just the prompts above), and it just never feels quite right. The answers are either poorly considered, lacking in detail, or giving too much detail with incorrect information. I’m sure that with the progression of hardware and large language models, I will revisit this within the coming months or years and rejoice in the improvements we’ve seen. For now, I’ll use it to generate some great trivia night team names or something.

Note: All content unless explicitly stated to be written by AI is written by me. I use AI to correct, grammar, spelling and punctuation and to encourage me to re-phrase unclear statements. The model used to assist in this blog post is Claude Sonnet 4.5

---

<!-- Source: https://palmater.ca/2022-12-31-a-new-year/ (personal blog register) -->

A New Year

 Posted on December 31, 2022

 Happy Holidays! I’m writing this update from what feels like an entirely new
desk setup. For Christmas, I received the
Keychron K8 Pro,
a Grovemade desk shelf clone, and an articulating desk lamp. To say I’m
grateful, spoiled, and privileged would all be understatements. I hope you have
also enjoyed your holidays and are keeping well with your friends and loved
ones.

Another privilege is that I’ve been able to take three full weeks off work for
the holidays. December 16 was my last day worked and I will be returning on
January the 9th. I usually attempt to take at least a week if not two this time
of year to recharge, reset and prepare for the year ahead. Three weeks off was
truly fantastic given how Christmas fell on a weekend this year, having the week
leading up to prepare was much needed. We hosted my partner’s parents and my
mother for Christmas, regrettably, my brother-in-law and his wife could not make
it - he’s a linesman with NB Power so was working all Christmas. A trip to Saint
John, Moncton, and then Florenceville concluded our family time. Who knew so
much food, laughter, and fun could be so exhausting? Even the dog was exhausted
by the flurry of coming and going.

With some of my time off I have been able to organize the garage to at least the
halfway state of where I want it. Installed some lights my partner received for
Christmas in our house. Installed some new digital thermostats in critical
heating areas (the den and my office), replaced the gasket for our wood stove,
and learned how to properly light said wood stove. Went shopping for some new
clothes (yay Christmas Gift Cards!) and got some incredible deals! The best
thing I have accomplished so far is relaxation, time to breathe, to stare into
the abyss (or the fire), time to read, and just cuddle with Chewie (our dog).

I’ve done some ideating on things that might be worthwhile writing about, which
lead me to consider that this medium (my static website blog posts) doesn’t
allow for conversation, which could be considered a feature rather than a bug I
suppose. There are options such as substack or just tooting threads on topics…
Even then I think I’ll just be jabbering into the void. If you read this, like
this, and think I should write more in a way that allows for conversation, DM me
at one of my links. My Twitter is still alive, for now.

For 2023 I have set a book reading Goal of 30 books, Up from a goal of 16
in 2022. I did manage to read 21 books in 2022 so the idea of reading 30 books
in 2023 seems like an achievable stretch goal. I have also set the goal of not
buying new books for myself until I have read all the ones that are in my
to-read pile. So I may be hanging on to those Chapters gift cards I got for
Christmas for a few months.

If this feels mostly like some disjointed rambling, that’s because it is.
Welcome to my brain, it’s s scary place.


---

<!-- Source: PR review comments, floatfinancial/core-backend, various dates (work register - typed live in GitHub review UI, not run through the creating-pull-requests drafting skill) -->

(discussion_r3544586362, PR #29804)
Hmm, this one's a bit tricky because this is actually testing by omission rather than declaration. I'm not sure how we necessarily solve for that, but this is essentially a coincidental pass, not an intentional pass

(discussion_r3544600938, PR #29804)
Maybe don't test the actual log call assertion, but test the key values are stripped as expected from the method. Testing a logger is testing a built-in, which generally just isn't worth the effort

(discussion_r3537247381, PR #29777)
this adds a maintenance list regardless which I don't think is sustainable. if we have one difference name the difference? So contains IDEM or equals maintenance queue??

(discussion_r3474379483, PR #29528)
This method AND validate_no_circular_management work against the same data and do the same object construction. Worth unifying as a single method that does the extraction first and then delegates the validation to each method respectively.

(discussion_r3474498805, PR #29495)
this conditional makes an assumption and then logs that same assumption - That if we don't find it, it's because it's not in scheduled state - it could be deleted, it could be a bad id (somehow) it doesn't necessarily mean it's not scheduled anymore. I don't think it's a huge deal but I find it hard to take the log at face value and if someone in the future is debugging this, they might...

(discussion_r3474519470, PR #29495)
This will not update in the audit log. We should never do direct updates on querysets...

prefer query - obtain
limit.status = new_val
limit.save(fields=[status], updated_by=get_or_create_journal_j())

since you're already in a loop there's no addditional benefit to using .update() on a queryset

(discussion_r3474544849, PR #29495)
you have two dependencies in this class on the businesses feature flags.. might be worth declaring explicitly as a property? depends how long the Feature flag switches live but something to consider.

(issuecomment-4792005475, PR #29466, reply to a reviewer)
not sure specifically what you mean but the two maintenance tasks here weren't min rebasing

---

<!-- Source: provided directly by user (personal narrative / travel-writing register) -->

We arrived around 12:30pm on Tuesday January 2 2024 to Penn station Moynihan train hall. We were feeling well rested, having spent the evening in New Haven Connecticut in a spacious king bedroom after indulging in a style of pizza unique to the city.

Our hotel being only 6 blocks away we decided to walk. We stepped outside into the cool but unseasonably warm December air and exited to 8th ave to see our first landmark: Madison square garden. I was hit with the acrid smell I have only experienced in NYC: Exhaust fumes, street meat, subway air and occasional whiff of sewer. The sound of millions of people moving about the city in a constant hum. City streets that go on for as far as the eye can see, a blur of cars and people, moving, always moving.

With assistance of Apple Maps we began making our way north through the west manhattan sidewalks. As a person on the autism spectrum I often find myself overwhelmed and at times frozen by stimuli. I was fine until the siren fired up.... Melissa turned around to confirm her suspicion that I was in distress. Sometimes sound cause me physical pain and I wince, most of then time I feel incredible panic, dread, the urge to cry and rock to self soothe. As quickly as it started, it was over and I found myself still walking in the direction of our hotel.

We arrived to a bustling lobby, folks checking out, leaving bags, going to and from the elevators. As we waited to leave our bags we were reminded of the reviews we saw where some Karen from the midwest was deeply offended she had to wait in a line. Understanding that you are in a city where almost 9 million people reside and it's estimated has an additional 2 million visiting or working, yeah Karen, you might have to wait a minute. We left our bags with the bellman who gave us a great recommendation for lunch just a few blocks away called "Mom's".

Mom's has a 90's theme, major hipster vibes and is located in Hell's Kitchen. The food was amazing and the coffee was adequate. The service I would call OK, we seemed to be in a limbo shift change and ended up waiting an extraordinarily long time for check. We each had chicken and waffles which is served with bachemel sauce and bacon.... no complaints from either of us.

After lunch we made our way to Times Square which I found dizzying with the signs flashing, the cars honking and the number of peddlers demanding my attention. Melissa found it to be less than she had expected. "The Ball" was still there, it being the day after new years and it seemed so much smaller in person, the whole place felt like a tourist trap. We left times square heading east toward 5th ave.

Fortunately we stumbled into Bryant Park, where the Christmas market had not yet closed for the season but it was in fact the last day.

---

<!-- Source: provided directly by user (professional/leadership reflection register, dictated rather than typed - expect natural speech repetition, not a deliberate written tic) -->

As software practitioners, there's always more work to do than there is time to do it. And it's always our job to find out what's worth doing and say no to the rest.

I've been trying to approach problems lately from a lens of opportunity and questioning rather than knowing and understanding. Instead of offering an opinion out of the gate, I'm being curious about the problem we're trying to solve.

Sometimes, that's the best place to start. Sometimes, my opposition, my natural opposition, is the best course of action, because it helps me say no to the right things.

This last week, my sprint team felt like it was moving at a snail's pace, and it had been going on for a couple of weeks. We'd all observed it, we'd all commented on it, but none of us knew what to do about it or why it was happening.

We eventually had a standup that went two and a half hours and got to the crux of the problem. Nobody had the conviction that what the team was trying to solve was the right thing, but everyone was too afraid to say it. Saying no to the solution that was in front of us would've sped us up by multiple weeks, because a team that operates with conviction to solve a customer problem operates exponentially faster than a team that's confused about why we're doing what we're doing.

We all care about doing a good job, and I'm fortunate enough to work with so many colleagues that have a customer-first focus.

But what that means is that when we're not customer-centric in the solutions that we're bringing forward, the team loses motivation to solve the problem.
