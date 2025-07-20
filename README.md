# youssef7rouz1-DownUnderCTF-OSINT-Zer0C00l-WRITEUP

When listening to the audio, I recognized two parts:
1.	The first part is Dual Tone Multi Frequency signaling, or DTMF—the same sounds a telephone makes when dialing a number.
2.	The second part I didn’t recognize (being born in 2002), so I searched the internet for modem sounds and came across this YouTube video: https://www.youtube.com/watch?v=gsNaR6FRuO0. 
The sounds after the DTMF tones looked identical, so I dug into the description of that video and found a wonderful post by Oona Räisänen: https://www.windytan.com/2012/11/the-sound-of-dialup-pictured.html.
 Now I know this is the handshake that initiates a telephone conversation between two modems: the first part is the dialing tones, and the second part is the data exchange handshake to negotiate the protocol.

Coming back to our MP3 file: in the first part (modem dials) we can hear seven digits. If we open the MP3 in Audacity (or any other audio editing software), we can clearly see seven distinct dial tone pulses. 
 Consulting the Wikipedia page on former Australian dialling codes (https://en.wikipedia.org/wiki/Former_Australian_dialling_codes), we confirm that up until 1996, Sydney subscriber numbers were seven digits (xxx xxxx) after the “02” area code.

 
 
 
Next, we use ffmpeg (a powerful command line tool for processing audio and video) to extract the first few seconds of the file ( 2 seconds ), isolating just the dialing segment.

  ```bash
ffmpeg -i Zer0C00l.mp3  -t 2 dialing.raw
```
We then decode those tones with multimon ng (a command line utility for decoding various digital modem signals, including DTMF). This yields the sequence 369 3244, which we format in full international style as 61-2-369-3244.

 ```bash
multimon-ng -t wav -a DTMF -i dialing.wav
```

A quick search for “61-2-369-3244 Sydney” leads to a RetroArchive BBS listing at http://annex.retroarchive.org/cdrom/nightowl-016/002A/GBBS9504/GBBS9504.TXT. Searching for “3244” in that file returns the entry:
NSW Waverley Hotline 61 2 369 3244 GeSo. . . . . 3:712/941
We clearly see “NSW” for New South Wales and the number 61 2 369 3244. All roads lead to “Hotline” (Nick Harvey’s Hotline).

A FidoNet lookup of node 3:712/941 confirms the same, https://fido.de/archive/lookup/aka/3%3A712%2F941
 
and a Reddit pointer to FidoNet archives (https://www.reddit.com/r/bbs/comments/u9zx3e/fidonet_archives/) led me to https://kuehlbox.wtf/bbs,files/fidohist. Navigating to the 1995 nodelists, downloading any file, opening it as text, and searching for “61-2-369-3244” again returns that exact line.
So indeed, all roads lead to “Hotline.”
Flag: DUCTF{Hotline}
