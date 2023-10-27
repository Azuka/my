---
categories: [ ]
date: "2023-09-28T16:00:51-07:00"
draft: false
images: [ ]
tags: ["gophercon"]
title: At GopherCon
---

"Sorry, I'll have to drop in 8 minutes" I typed into a dedicated Slack channel at work for a group bug fixing session.

I was at [GopherCon](https://www.gophercon.com/), not quite bright-eyed from a night of wrestling with my hotel's
internet connection, but looking forward to a Keynote talk
about [proposed changes to the encoding/json package](https://www.gophercon.com/agenda/session/1160910).

I had my badge flipped, because I didn't want to talk to anyone and was in that little zone where I counted the time
down before I had to head back upstairs to the Keynote ballroom.

"Are you Azuka?"

I looked up to see someone approaching. He didn't look familiar, and for a brief moment I wondered if he was on
the [buf](https://buf.build/) team. I'd had a pretty interesting chat the day before about my issues
getting [connect-es](https://github.com/connectrpc/connect-es) to run in Firebase functions
using fastify, as well as some typing issues converting
typescript [PlainMessage](https://github.com/bufbuild/protobuf-es/blob/main/docs/runtime_api.md#plainmessage)
representations to Firestore-compatible models. Turns out I was talking to an ex-Googler who worked on Firestore, but I
digress.

"Yes I am," I replied, not quite able to place him.

"Why did you attack the CTF server on Tuesday?"

That came completely out of left-field, because the only response I could muster was "huh?"

"The Capture The Flag event server" he continued. "You attacked the server and took down the community internet on
Tuesday."

Whoa. That was a lot to unpack. Surely this was some red team exercise, a prank, or just some guy who was off his
rocker, right?

Right?

Well, no.

"What were you doing between 1:30 and 3* Tuesday afternoon?" he continued.

"Well, I'm currently trying to get a containerized application to run on Google Cloud," I explained. "I've been trying
to get it to build but having issues on my Mac M1 due to some of the python scripts somehow not working locally."

"I'm not a python guy," at this point I was rambling because his expression told me he didn't believe a word I was
saying. "It was set up using something called poetry which was taking a long time to solve dependencies and pull pip
packages. I eventually started running and troubleshooting on Google Cloud build. Would you like to see?"

He did.

I showed him the cloud build timestamps, my git history as well as my local history (thanks Jetbrains / Webstorm). He
also collected the host names of my personal and work MacBooks as well as the ifconfig outputs, possibly to match my MAC
address. He asked if I had another MacBook, but I told him I didn't.

"Are you in the Gophers Slack?" he asked.

"Yes" I replied.

"Why didn't you respond to the email from H* yesterday?" he followed up with another question.

"I'm not sure I got it," was my response. Who checks work email when not at work?

"She sent it to your work email address."

I looked in my inbox, and there it was in the non-focused section:

"READ ME: GC23 | Need to Speak to You"

Hardly a title that doesn't scream: recruiter.

"Oh, it wasn't in the focused section in Outlook," I explained.

"Okay," he gave me a non-committal answer. I asked his name, still convinced this wasn't something serious, just so I
could look him up. He walked away, I joined the keynote, and that was it.

Or not.

I looked up both H and our mystery man who I'll call B, and confirmed they were both legitimately associated with the
conference, so I replied H via email that I'd be happy to talk if she still wanted to. When I looked in the gophers
Slack channel, it turned out she had sent me a direct message there as well, so I told her I'd replied her email and
would be available if she still wanted to talk.

"Yes please. Come see me at the tech table after this talk."

"Okay."

Right after the talk, I did. I found B there, and together with H and another guy who looked vaguely familiar, but who
I'll call E, we found a corner to talk.

What it boiled down to was that they knew what I'd "done", and the docker stuff I said I was doing didn't match the
network traffic they saw ("I"'d apparently hit a peak of somewhere around 300mb/s) which drove the internet bill
somewhere
into the hundreds of thousands of dollars.

Then came the usual platitudes of "everybody makes mistakes", "some people have done this before and had to be
prosecuted", and "you could be banned from attending any other event."

I was flummoxed, and starting to get _very_ angry. The problem is when I get angry, I get very calm, and the incredulous
looks on their faces told me they were annoyed at how good an actor I was.

E told me so, and B brandished a printed page with the hostname "`Azukas-MBP`" (my hostname is `Azukas-Macbook-Pro`) and
a
MAC address as evidence. According to them , they found me Thursday morning because they could track my MacBook based
on proximity, and it wasn't a mistake that they'd tracked me down. H said she wasn't a technical person, but obviously
trusted them, and after going back and forth pretty much told me to "stay in my lane" and just not cause trouble for the
rest of the conference.

At some point, I asked how I could help (network and other logs from my computer, anything) and got the scoffing
response that none of them had the time to spend on investigating this. I also remembered a prolonged network blip
during the
[Service Weaver workshop](https://www.gophercon.com/agenda/session/1200016) within that timeframe where everybody (
including me) got stuck downloading go modules (`go mod tidy`) but given the
attitude I was being given, I wasn't sure about volunteering that information.

I asked what would happen when or if they eventually discovered I wasn't responsible.

"We would of course tender an apology, but we're all in the tech space. These things happen, and it could be a mistake,
or you were just playing around, but we just need you to come clean. When you look at the fact you didn't reply your
email or the direct Slack message until now, it's hard not to be suspicious."

It was a "you know we know that you did it" situation. The only person who was absolutely sure I knew nothing about this
was me, and at 3:1 the odds were just not in my favor.

After another reminder from H to "stay in my lane", we broke up and went back in.

I enjoyed the encoding/json v2 proposal talk, but when the keynotes ended, I had some time to reflect on everything
that transpired and was completely soured on the rest of the conference.

I sent a quick message to my manager at work just in case, and reached out to a few friends.

At this point, I have a lot of questions and no answers.

- Why were they absolutely sure I was responsible for this?
- Who actually did it?
- How was anyone able to generate that kind of traffic?

I understand running a conference is hard, but I hope some time is actually taken to look into this and someone reaches
out to me.

I'll wait.

*I may have these times wrong.

## Post-mortem (update: 2023-10-26)

It's been almost a month since this happened. I wrote this down immediately after, because writing has always been my
outlet,
but also because I wanted to be sure when I looked back on this in hindsight that my feelings, especially the anger I felt
were justified.

In the meantime, I did some thinking, as well as some snooping. One thing H kept repeating was that by signing up for the conference, I agreed to
abide by the code of conduct at GopherCon. I felt blindsided,
since this was three people (from my perspective) starting off with accusing me, and attempting to railroad me into admitting to attacking a network server. 

At no point was any attempt made to hear my side of the story, other than the beginning accusation by B. I told him about the containers I was building because understandably, 
that was the only thing I was working on that I believed could increase traffic usage on the network.

First, I went looking for an arbitration process in
the [GopherCon Code of Conduct](https://www.gophercon.com/page/2482900/code-of-conduct), only to find that it was
exactly a verbatim copy of the
[Go Community Code of Conduct](https://go.dev/conduct), including the email for the Google Open Source Programs Office.
I do not believe that Google runs GopherCon so this seemed like there was nothing in place to handle incidents like this, especially give the very unprofessional way I was approached and talked down to.

Looking in the #gophercon-ctf channel in the Gophers Slack workspace, I saw an exchange:

>B
> 
>2:37 PM
>Some is attacking `https://{redacted}.zip` again
>
>2:37
> 
>It's down again
>
>
>B
> 
>2:43 PM
>Back up
> 
>2:43
> 
>If anyone identifies who is attacking `https://{redacted}.zip` you get 1000 points

So somewhere in there, someone "investigated" and got 1000 points.

Additionally, B visited my LinkedIn profile multiple times, although I'm not sure why.

The next day, I sent a Slack message to a group chat with B, H, and E, because I wanted to see if there was a neutral
third party who would take a look at what happened, and **_listen_** to me.

> Azuka Okuleye
> 11:47 AM
>
>Hello all.
>
>I understand running an event is very stressful, but I gave myself some time to sleep on and think over what happened
> yesterday.
>
>This was my very first GopherCon, and I'm still very upset at being accused of attacking a server. I hope you have some
> time now with everything over, because this is a very serious allegation and I refuse to let this be swept under the
> rug.
>
>I would like to send an email and get this looked into VERY thoroughly because a community is driven not just by
> successes, but how failures and oversights are handled, and I consider this both. Please let me know who to include.
>
>Thank you.

Very quickly, I got back a reply:

> H
> 12:05 PM
>
>The email would be to us and our stance hasn't changed. We allowed you to stay despite tracking the device to you both
> days.

which pretty much told me I was not going to get anywhere with them, so I replied with:

> Azuka Okuleye
> 12:05 PM
>
> Thank you. That's all I needed to know.

E followed up with:

> E
> 12:33 PM
>
> Azuka, I wholeheartedly agree about the community aspect. That includes following rules that are in place to not
> degrade the experience of others.
>
> You weren’t chosen at random. Logs were reviewed, we worked with the hotel’s networking staff as well. The host name
> of the machine was your machine. We tracked the MAC address to you. As an IT professional, I’m sure you can agree that
> is sufficient evidence to reasonably consider someone a suspect.
>
> You suffered no harm. We approached you, asked you about it, informed you that those types of things won’t be
> tolerated, and let you know the harm that it cost us and the significant networking expenses that we’d be charged. It
> also damages our reputation with the venue. They may not let us come back if issues like this are common. Nothing was
> publicized, you weren’t asked to leave, it was a warning.
>
> You’re asking for a “VERY thorough” investigation, are you sure you know what you’re asking for? That means working
> with the hotel’s network team further, hiring a third-party security team so that it’s not a conflict, then subpoenaing
> your devices to have them forensically audited. That will come at a significant cost to us both financially and in time.
> If that’s the route we’re taking, it’s because we’re involving the authorities and handling it the legal route.
> After-all a crime was committed.

which just left me dumbfounded. I suffered no harm, and I was asked, not harangued about it, where the only acceptable
answer to them would be me admitting to something I didn't do?!

I followed up with:

> Azuka Okuleye
> 12:57 PM
> E, I'm sure you have a lot of information on your side, and that it's my word against what you have found so far.
>
> I would very likely be inclined to trust it if I were in your position, but I looked in the ctf channel and I know,
> despite you not believing me, that I did not attack your systems. If, and I know that seems farfetched to you, but if
> you were in my position and had absolutely no idea what was going on, would you take the word of people who have
> determined already that you were responsible? I have been on the other side exactly once in my life, and finding out is
> still one of my lowest moments.
>
> Is there even a miniscule chance that I've been impersonated by someone else? As an IT professional, I'm not a
> networking person, but even I know that I wouldn't advertise myself using a machine with my name, and that MAC addresses
> can be spoofed.
>
>Please remember you are dealing with a person here. I left the conference yesterday around 3 after getting off the
> network as soon as we had our talk, but I stayed in San Diego until late hoping against hope that new information would
> come up, and this would all turn out to be a mistake.
>
>I offered to help yesterday, and I still stand by that. There's surely some middle ground because my experience here is
> I answered and provided the information B asked for, and would be happy to provide any additional info.
>
>If the answer is that you "let" me stay because it would take considerable effort to look into this, I really hope the
> next person this happens to gets a better experience than I've had.

So far, that conversation has gotten no response, which just confirms my gut feeling that all the parties involved at the other end are interested in is for this to just go away. 

This has been handled very poorly, and from what I see, there's little to no accountability here. I think I've gone above and beyond,
and maybe shows that underneath the veneer of "community", some people are more equal than others.

I've gotten a few suggestions, but I've never liked being the center of attention, so "trial by social media" is unfortunately not where I want this to go. 

If I've shared this with you, please respect that.
