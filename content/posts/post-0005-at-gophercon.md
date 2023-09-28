---
categories: [ ]
date: "2023-09-28T16:00:51-07:00"
draft: false
images: [ ]
tags: [ ]
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
