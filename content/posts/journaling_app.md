---
title: "Journaling App. Pet project."
date: 2023-11-06
slug: "journaling_app"
description: "An Expo App for iOS, that records your voice and transcribes it using OpenAI to the text into the Google Doc"
keywords: ["python", "react native", "GCP", "WhisperAI"]
draft: false
tags: ["Python", "React Native", "GCP", "WhisperAI", "OpenAI"]
summary: An Expo App for iOS, that records your voice and transcribes it using OpenAI to the text into the Google Doc.
toc: true
---

## Summary:
This is the story of how I built a voice-to-text journaling app. Starting from a need to keep up with my thoughts, this project pushed my limits. I tackled tech choices, solved tricky problems, and learned loads – especially about keeping things simple and user-focused. Big learning curve, bigger rewards!

## Tools, I leaned on:
Expo, OpenAI, GCP.

The code for this project is [here](https://github.com/mariavyso/journaling_app)!
_____________________________________________________________________________________________
Have you ever felt like your thoughts are racing faster than your hands can write them down? As a Junior Software Engineer and a self-proclaimed journaling enthusiast, this was a constant struggle for me. That’s where my journey with this voice-to-text journaling app began.

It all started with a simple yet intriguing challenge from my therapist: to explore journaling as a tool for self-discovery. In 2023, pen and paper seem archaic, but there’s something about writing down your thoughts that feels deeply personal. However, when my hand was cramped from trying to keep pace with my thoughts, I knew there had to be a better way. So I had an idea to create an app that could transcribe my monologue into written text and send it to the document with a timestamp, so I could go back and re-read it if I wanted.

I was aware of existing apps that offered similar features, but as someone with a product management background, the passion for crafting something tailored to my needs was irresistible. Plus, it was the perfect project to improve my skills and learn something new.

## Little Roadmap
I decided to break the task into 4 phases:
1. Outline the application’s core functions (1 Day)
2. Research of the technologies (3 Day)
3. Build an MVP (5 days)
4. Finalizing the product (5 days)

To manage myself more effectively I added the approximate number of days that I planned to spend on each step, given that I have a full-time job.
<div style="width:70%;height:0;padding-bottom:57%;position:relative;">
    <img src="https://media.giphy.com/media/toXKzaJP3WIgM/source.gif" width="100%" height="100%" style="position:absolute" frameborder="0">
</div>
via GIPHY

## Phase 1. Core functions of the product
I was thinking about something simple, yet effective and decided on these functions:

- Frontend: A button to start/stop audio recording and a timer.
- Backend: Audio recording, transcription, and text integration into a document.

## Phase 2. Research of the technologies
I started my research with the frontend part since my experience in interface programming was limited to the Chrome extension (insert link to post). Fortunately, I have friends who have experience in mobile development and after a couple of interviews and googling, I decided to write a naive application, using the open-source platform Expo.

For the backend part, I found two options for easy audio transcription:

1. Google Cloud Speech-to-text API
2. WhisperAI API from OpenAI
After testing, WhisperAI emerged as the winner for its accuracy and cost-effectiveness.

Since the project was intended for personal use only, for the final step, I decided to use a Google Documents.

At this step, I tried to run a quick test of the product, starting with the backend, since I have more experience in this area. Everything went quite successfully here. I recorded a couple of audio to test the script, transcribed them using the WhisperAI API, and sent the text to Google document.

Testing the frontend part was also quite easy - I had a button to turn the audio recording on and off and I could use Expo to record and then I could play this audio on my device.

At this step, it seemed to me that everything was quite simple and I needed a couple more days to assemble a full-fledged MVP.


## Phase 3. The MVP Challenge
This phase was a rollercoaster.

Inspired by the success of those quick tests, I started with the frontend and wrote the code for the button, that turns the audio recording on and off and added the timer so I could see the duration of my record on the screen. (It was not fast, after this experience I no longer joke about the frontend) For the backend, I decided that it would be great to store my audio recordings in the Google Storage Bucket and wrote a Google function that would be triggered by the new audio, that appeared in the bucket.

But here I met an issue with the audio format and it turned out that Expo records audio in CAF format by default, and even if I specify MP3 in the recording settings, the WhisperAI API does not understand this format. So I added the ffmpeg library for audio reformatting. This helped, but a new issue arose.

As it turns out, to send audio from Expo to the Google bucket, I need to either generate a token manually on the Google side and constantly update it, or register the application and configure it as a real app, which was not part of my plans either for the MVP nor for the real product. I was okay with having the app available through Expo Go and not having it in the Apple store ($99 per year and ongoing support of the app for other people wasn’t on my wishlist).

I left this question for step 4, which means that for now, I needed to copy the token and paste it to the Expo manually. The rest of the app worked well, the Google function triggered as soon as new audio appeared in the bucket, the WhisperAI API transcribed the text, and the text was inserted into the document, I was happy.


## Phase 4. Crucial pivot and finalizing the product
To solve the problem of generating a token, I turned to my mentor, who, by luck, is a senior software engineer. He suggested two options for solving the problem:

1. Rewrite the code so that audio is sent from Expo to the Google function via an HTTP request.
2. Try to generate a new JWT token inside of the Expo.

Of course, I didn’t want to destroy my setup in Google Cloud, which I considered a genius and met my requirements, so I chose option 2 - generating a token and authentication inside Expo.

As it turned out, this was challenging, because we have a ton of libraries in Python and JS for JWT signature generation, but somehow JS libraries didn’t work in my case.
My mentor became interested in solving this issue and offered some help with this and wrote a code that works for Expo and Google and I was over the moon because it finally worked.

After a couple of days of using the application for its intended purpose, I realized that after each click on the stop recording button I used to go and check the Google document to see if the audio transcript had been loaded into it. This was not convenient and I decided that I needed to display a message on the screen as soon as new text was loaded.

But for incorporating this feature, It turned out that I needed one more authentication, and guess what I had to do…

<div style="width:70%;height:0;padding-bottom:57%;position:relative;">
    <img src="https://media.giphy.com/media/GZLf5Njk1KGwU/source.gif" width="100%" height="100%" style="position:absolute" frameborder="0">
</div>
via GIPHY

</br>
</br>

This led to a crucial pivot: instead of storing audio, I decided to send it directly to the backend for processing. A challenging decision, but it streamlined the app and reduced complexity. Well, that is, I returned to the first option that my mentor suggested.

This brought enormous benefits, because now I know that I need to listen to adults, I need to think in advance about the functionality that would be nice to have, and review other people’s projects to understand what I need to bring into my own one.

After several iterations and incorporating a feature to notify users once their journal entry is ready, the app transformed from a personal tool into something I believe could benefit others. Here’s a sneak peek at the app in action:

**Video of using the app is still in production, but it will be there soon.**

_____________________________________________________________

## Aftermath
**Throughout this journey, the learning curve was steep, but the personal and professional growth I experienced was immeasurable. I can say, that I became better at problem-solving, and broadened my technical skill set. Working with my mentor and seeking advice from peers in mobile development were crucial in allowing me to see my project from a different perspective. I had many hours of frustration and doubt, but overcoming these barriers was fulfilling.**