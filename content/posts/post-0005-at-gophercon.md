---
categories: []
date: "2023-09-28T16:00:51-07:00"
draft: false
images: []
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

"What were you doing between 1:30 and 3\* Tuesday afternoon?" he continued.

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

"Why didn't you respond to the email from H\* yesterday?" he followed up with another question.

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

\*I may have these times wrong.

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

> B
>
> 2:37 PM
> Some is attacking `https://{redacted}.zip` again
>
> 2:37
>
> It's down again
>
> B
>
> 2:43 PM
> Back up
>
> 2:43
>
> If anyone identifies who is attacking `https://{redacted}.zip` you get 1000 points

So somewhere in there, someone "investigated" and got 1000 points.

Additionally, B visited my LinkedIn profile multiple times, although I'm not sure why.

The next day, I sent a Slack message to a group chat with B, H, and E, because I wanted to see if there was a neutral
third party who would take a look at what happened, and **_listen_** to me.

> Azuka Okuleye
> 11:47 AM
>
> Hello all.
>
> I understand running an event is very stressful, but I gave myself some time to sleep on and think over what happened
> yesterday.
>
> This was my very first GopherCon, and I'm still very upset at being accused of attacking a server. I hope you have some
> time now with everything over, because this is a very serious allegation and I refuse to let this be swept under the
> rug.
>
> I would like to send an email and get this looked into VERY thoroughly because a community is driven not just by
> successes, but how failures and oversights are handled, and I consider this both. Please let me know who to include.
>
> Thank you.

Very quickly, I got back a reply:

> H
> 12:05 PM
>
> The email would be to us and our stance hasn't changed. We allowed you to stay despite tracking the device to you both
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
> Please remember you are dealing with a person here. I left the conference yesterday around 3 after getting off the
> network as soon as we had our talk, but I stayed in San Diego until late hoping against hope that new information would
> come up, and this would all turn out to be a mistake.
>
> I offered to help yesterday, and I still stand by that. There's surely some middle ground because my experience here is
> I answered and provided the information B asked for, and would be happy to provide any additional info.
>
> If the answer is that you "let" me stay because it would take considerable effort to look into this, I really hope the
> next person this happens to gets a better experience than I've had.

So far, that conversation has gotten no response, which just confirms my gut feeling that all the parties involved at the other end are interested in is for this to just go away.

This has been handled very poorly, and from what I see, there's little to no accountability here. I think I've gone above and beyond,
and maybe shows that underneath the veneer of "community", some people are more equal than others.

I've gotten a few suggestions, but I've never liked being the center of attention, so "trial by social media" is unfortunately not where I want this to go.

If I've shared this with you, please respect that.

## Update and some closure (2024-05-05)

A day after I posted the post-mortem update above, I sent an email to the addresses on the [Go Community Code of Conduct](https://go.dev/conduct) page.

> Date: Fri, 27 Oct 2023 21:53:54 -0700\
> Subject: GopherCon Complaint\
> From: Azuka Okuleye &lt;redacted&gt;\
> To: <span>conduct@golang.org, opensource@google.com</span>
>
> Hello,
>
> I found these emails on the Gophercon code of conduct page:
> https://www.gophercon.com/page/2482900/code-of-conduct.
>
> Are these the correct places to report a complaint?
>
> Thank you,\
> Azuka Okuleye

Having received no reply, I sent out another email a month later.

> Date: Sat, 25 Nov 2023 09:30:58 -0800\
> Subject: Re: GopherCon Complaint\
> From: Azuka Okuleye &lt;redacted&gt;\
> To: conduct@golang.org, opensource@google.com
>
> Good day,
>
> I wanted to follow up on this email I sent a month ago.
>
> Before I share the details, I wanted confirmation that I'm reaching out to
> the right channel (s).
>
> Please advise if otherwise.
>
> Thank you.
>
> Azuka Okuleye

At this point, with yet no reply or confirmation, I went with a formal complaint to the Golang Stewards:

> Date: Thu, 30 Nov 2023 07:59:13 -0800\
> Subject: Formal Complaint about GopherCON San Diego October 28 2023\
> From: Azuka Okuleye &lt;redacted&gt;\
> To: conduct@golang.org
>
> Good day,
>
> I am reaching out to make a complaint about how I was treated at the above
> event.
>
> On two occasions, I was approached, first by Benji Vesterby, then by
> Heather Sullivan, Erik St. Martin and Benji Vesterby and accused of
> attacking a server used for the Capture the Flag event at the conference. I
> had no idea about any of this, and I believe I was cooperative in providing
> any information asked of me.
>
> Instead, I was subjected to a very disrespectful huddle where I was talked
> _at_, not to, and the only thing they wanted from me was a confession to a
> crime I did not commit. Heather in particular kept repeating that I had
> agreed to the code of conduct by registering for the conference. I believe
> that code of conduct goes both ways, especially when the organizers of a
> conference can make accusations in impunity and refuse any pretense of
> involving a neutral third party to investigate their claims.
>
> Benji was outright hostile from the very beginning when he walked up to me
> and accused me of attacking the server, and I'm not sure of Erik's
> motivations when he threatens me with either a criminal investigation, or
> sweeping this under the rug.
>
> I wrote an account of what happened on my blog an hour later, to ensure I
> recollected correctly, which you can find replicated below at the end of
> this email: https://azuka.github.io/posts/post-0005-at-gophercon/. I have
> attached relevant screenshots from Slack, including when I reached out to
> all three people mentioned above.
>
> I would greatly appreciate a confirmation that this email has been
> received, even if no immediate action is taken.
>
> Sincerely,
>
> Azuka Okuleye
>
> xxx-xxx-xxxx (text-only please, if you need to reach me).
>
> https://www.linkedin.com/in/azuka
>
> ---
>
> &lt; everything below here was just copy pasting my entire post before this section &gt;

Even that got me no reply, so I forwarded to the Google Open Source email address

> Date: Thu, 30 Nov 2023 08:01:17 -0800\
> Subject: Fwd: Formal Complaint about GopherCON San Diego October 28 2023\
> From: Azuka Okuleye &lt;redacted&gt;\
> To: opensource@google.com
> 
> Good day,
> 
> I hope this finds you well.
> 
> I submitted a formal complaint to the Golang Stewards, which you can find
> below.
> 
> Sincerely,
> 
> Azuka Okuleye
> 
> ---------- Forwarded message ---------\
> From: Azuka Okuleye &lt;redacted&gt;\
> Date: Thu, Nov 30, 2023, 7:59 AM\
> Subject: Formal Complaint about GopherCON San Diego October 28 2023\
> To: <conduct@golang.org>
> 
> &lt; etc, etc &gt;

This time I got a response:

> From: Russ Cox &lt;redacted_google.com&gt;\
> Date: Mon, 4 Dec 2023 12:43:08 -0500\
> Subject: conduct report re GopherCon\
> To: &lt;redacted&gt;
> 
> Hi Azuka,
> 
> Thank you for reaching out to both conduct@golang.org and
> opensource@google.com. I wanted to let you know that we received your mail.
> There was in fact a problem with the conduct@golang.org alias forwarding
> that we've now resolved, so thanks for pinging opensource@ too. I'm not
> sure whether I'll be able to say anything more, but we did receive the
> message, and I intend to talk to the GopherCon organizers to learn more
> about the situation. Thanks again.
> 
> Best,\
> Russ

to which I sent a reply.

> Date: Mon, 4 Dec 2023 09:45:39 -0800\
> Subject: Re: conduct report re GopherCon\
> From: Azuka Okuleye &lt;redacted&gt;\
> To: Russ Cox &lt;redacted_google.com&gt;
> 
> Hi Russ,
> 
> Thank you very much for getting back to me and I appreciate your looking
> into this.
> 
> Best,\
> Azuka

Then I waited.

And waited.

And waited...

One thing I knew would happen is the GopherCon code of conduct had to change. There was no way Google, or the Golang
stewards would
want people bringing complaints about a conference they did not directly control to them.
I checked weekly, sometimes twice a week, and it did change, so I got optimistic.

I waited three months after my complaint without hearing anything back. In the meantime, I looked at the email on the code of conduct,
which pointed to Gopher Academy. Given that
the [registration](https://search.sunbiz.org/Inquiry/corporationsearch/SearchResultDetail?inquirytype=EntityName&directionType=ForwardList&searchNameOrder=GOPHERACADEMY%20L130000709560&aggregateId=flal-l13000070956-b1f58cf5-8538-4293-9c3c-f040ef19524e&searchTerm=GOPALA%20INC.&listNameOrder=GOPHERACADEMY%20L130000709560),
pointed to Erik St. Martin who also (presumably) owns
the [gopheracademy GitHub organization](https://github.com/gopheracademy), I had zero confidence actually following that
would yield any useful results.

On March 1st, I sent a follow-up email.

> Date: Fri, 1 Mar 2024 13:57:09 -0800\
> Subject: Re: Formal Complaint about GopherCON San Diego October 28 2023\
> From: Azuka Okuleye &lt;redacted&gt;\
> To: opensource@google.com, conduct@golang.org
> 
> Hello,
> 
> I would like to follow up on the status of my complaint.
> 
> I understand you may not be at liberty to share the details, but could you
> let me know if this is still in progress or if you've come to a resolution?
> 
> Thank you,
> 
> Azuka Okuleye

Which got me nothing.

On March 20, I sent a follow-up email to Russ Cox, and got an out of office auto-reply stating he was out until March 26th.

> Date: Wed, 20 Mar 2024 08:04:21 -0700\
> Subject: Re: conduct report re GopherCon\
> From: Azuka Okuleye &lt;redacted&gt;\
> To: Russ Cox &lt;redacted_google.com&gt;
>
> Hi Russ,
>
> Sorry if I'm bugging you but I just wanted to check back in with you.
>
> I sent a follow-up email to both conduct@golang.org and
> opensource@google.com but didn't hear back and wanted to confirm if this is
> still ongoing or if there's been a conclusion: understanding, of course
> that it may not be shared with me.
>
> Thank you,\
> Azuka

April came around and nothing happened. I had traveled to Chicago for work, and part of that involved containerizing an
application while working out dependencies to ensure it could run locally on my work Mac M1s, as well as the Linux AMD64 machines our Kubernetes nodes used.
This was almost exactly the same kind of problem I was working on when I got accused.

I was done being patient, and I wanted this done and gone, so I sent an email.

> Date: Fri, 19 Apr 2024 08:05:44 -0700\
> Subject: Re: Formal Complaint about GopherCON San Diego October 28 2023\
> From: Azuka Okuleye &lt;redacted&gt;\
> To: opensource@google.com, conduct@golang.org
> 
> Good day,
> 
> I'm pretty frustrated that I've heard nothing back after the initial
> acknowledgement from Russ Cox.
> 
> Are these emails working? What do I have to do to get a reply?
> 
> Azuka Okuleye

My reply came in almost an hour later, which I didn't get to reply until I was on the plane headed home.

> From: Russ Cox &lt;redacted_google.com&gt;\
> Date: Fri, 19 Apr 2024 12:10:24 -0400\
> Subject: Re: [conduct] Re: Formal Complaint about GopherCON San Diego October
> 28 2023\
> To: Azuka Okuleye &lt;redacted&gt;\
> Cc: opensource@google.com, conduct@golang.org
> 
> Hi Azuka,
> 
> Apologies for not getting back to you earlier. I started to reply a few
> times but always got interrupted.
> 
> I checked in with the organizers, and they shared their point of view and
> how the events unfolded from their point of view during the conference. In
> general these kinds of situations are difficult to handle well, and it's
> not uncommon for both sides to feel frustrated with the other side. The
> organizers said that they don't have definitive proof that the attack came
> from your laptop, only the traffic stats from the hotel's wifi system,
> which showed very significantly more traffic from your laptop than anyone
> else's at the conference. That's why they spoke directly to you but did not
> ask you to leave the conference and did not discuss the incident with
> anyone else. I can also see that from your point of view it feels like an
> attack to even be accused.
> 
> Ultimately I think it is an unfortunate situation but with little more to
> be done. The organizers confirmed that you are welcome to attend again.
> 
> I would suggest installing a macOS network traffic monitor such as Little
> Snitch so that you have more visibility into what your machine is doing on
> the network. It is possible there was a runaway program doing something
> that caused all the trouble. More visibility is always a good thing.
> 
> Best,\
> Russ

Honestly, I was very disappointed by the reply. There's a _very_ big difference between "We noticed a lot of traffic
coming from your laptop" and "You attacked our server", especially when I shared willingly that I'd been trying over and
over to build an almost 20gb Dockerfile locally, and running into issues.

I had a very sarcastic reply typed up almost immediately in Google Keep (for carthasis, I guess), but ultimately sent this out:

> Date: Fri, 19 Apr 2024 18:08:44 -0500\
> Subject: Re: [conduct] Re: Formal Complaint about GopherCON San Diego October
> 28 2023\
> From: Azuka Okuleye &lt;redacted&gt;\
> To: Russ Cox &lt;redacted_google.com&gt;\
> Cc: opensource@google.com, conduct@golang.org
> 
> Hi Russ,
> 
> Thanks for getting back to me. I'm currently in the middle of traveling so
> I may be brief.
> 
> I speak very confidently when I say there were better ways to handle this.
> 
> From the very beginning when Benji walked up to me with an accusation, to
> when I later met with Erik and Heather, I was completely on the defensive.
> 
> I dare say any one else in my situation would have frozen up (which I did).
> I was given no proof or any information: just told to admit to what I did.
> The implication was that since I did not reply my work email (if only they
> stopped to consider I was at a conference and away from work) or gopher
> Slack (which I'm not active on in the first place), then I had something to
> hide. I was cooperative, I told them what I'd been working on, even though
> it's side work that could have gotten me in trouble at my job, I offered my
> laptop to do any additional digging.
> 
> Nobody's a villain in their own story, and I'm sure we all believe we did
> the best under the circumstances.
> 
> Thank you again for bringing this to a conclusion, although not the one I
> wanted, but you don't always get what you want in life. An apology (not
> from you) would have sufficed.
> 
> I don't think I'll be returning to gopherCon unless I'm mandated to at
> work, because as I've mentioned before, the handling of this by the
> organizers and the way I was spoken to will not make me comfortable
> attending.
> 
> All the best.
> 
> Azuka

Well, this is where it all ends, and all these people, as well as GopherCon, can stop living rent-free in my head.

Oh, and here's the docker file that "attacked" the precious CTF server, because I'm still not buying that nonsense:

```dockerfile
# <Azuka>: Where I got stuck because it was initially running on a machine with a gpu
#FROM tensorflow/tensorflow:2.4.1-gpu
FROM tensorflow/tensorflow:2.13.0
#FROM --platform=linux/x86_64 tensorflow/tensorflow:2.13.0

WORKDIR /app

VOLUME /database
VOLUME /input
VOLUME /logs


# <Azuka>: GPG problems when building the image from scratch
# https://github.com/tensorflow/tensorflow/issues/56085
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/3bf863cc.pub

# Install system requirements.
RUN apt-get update && apt-get upgrade -y && apt-get install -y \
    git \
    libfontconfig \
    fontconfig \
    libjpeg-turbo8 \
    libxrender1 \
    xfonts-75dpi \
    xvfb \
    wget \
    && rm -rf /var/lib/apt/lists/*

# We build wkhtmltox ourselves to include patched QT.
COPY backend/deps ./deps
RUN dpkg -i ./deps/wkhtmltox_0.12.5-1.bionic_amd64.deb

# Copy large binary file requirements early to take advantage of Docker layer
# caching.
COPY backend/bin ./bin


# <Azuka>: Everything here before dumb-init was me fighting python deps. I'm not a python guy
# Install poetry
RUN pip install --upgrade pip
RUN python3 -m pip install --upgrade pip
RUN pip install poetry==1.1.15
RUN poetry config virtualenvs.create false
COPY backend/pyproject.toml ./pyproject.toml
RUN rm -rfv $(poetry config virtualenvs.path)/*
#RUN poetry export --no-dev -f requirements.txt --output requirements.txt
#RUN pip install -r requirements.txt
RUN poetry install --no-dev -vvv
RUN poetry run pip install --find-links=deps torchvision
#RUN python3 -m spacy download en_core_web_trf
#RUN python3 -m spacy download en_core_web_sm

# # Install python requirements with pipenv
# #RUN pip install setuptools --upgrade
# RUN pip install --upgrade pip
# RUN pip install pipenv
# #COPY backend/pyproject.toml ./pyproject.toml
# COPY backend/Pipfile .
# # We reuse tensorflow already installed in the base Docker image.
# RUN sed -i '/tensorflow\ =/d' ./Pipfile
# RUN pipenv install
# #RUN python3 -m pipenv sync --deploy --system
# RUN pip install --find-links=deps torchvision
RUN python3 -m spacy download en_core_web_trf
#RUN python3 -m spacy download en_core_web_sm
RUN pip install psycopg2-binary pyparsing

RUN wget -O /usr/local/bin/dumb-init https://github.com/Yelp/dumb-init/releases/download/v1.2.5/dumb-init_1.2.5_x86_64
RUN chmod +x /usr/local/bin/dumb-init

ENTRYPOINT ["/usr/local/bin/dumb-init", "-v", "--"]

# We copy individual folders to preserve directory structure.
COPY backend/README.md ./README.md
COPY backend/report_templates ./report_templates
COPY backend/runtime ./runtime
#COPY backend/bin ./bin
COPY backend/lib ./lib
COPY backend/config ./config
#COPY backend/rad_parser ./rad_parser
COPY backend/app.py .

COPY types/app-report.schema.json ../types/app-report.schema.json

ENV ENV="dev-docker"
#ENTRYPOINT ["python3", "app.py"]

CMD ["python", "app.py"]
#CMD ["python3", "-m", "pipenv", "run", "python", "app.py"]
#CMD ["python3", "-m", "trace", "--trace", "app.py"]

```
