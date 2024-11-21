A karaoke app, play karaoke video from youtube, with voice and  camera calling through Agora  
**problem**: streaming that youtube video 
- i manage to get the youtube id  
- i can download the video but if not streaming from backend, it will not sync 
# Video Streaming problems  
- DASH or HLS protocol is best for streaming video, but i use django websocket 
-> im lazy to switch 
### solutions 
- ffmpeg is best to create stream but doesnt support websocket :) 

- jsmpeg is alternative and support websocket 
	1. connect jsmpeg (in client) to websocket
	2. server sends out mpeg-ts data  
	3. client receive and render video  
- use http (django fileresponse)
- (DASH or HLS)
	










# haloclub
--- 
# Refererences 




2023 07 02 20:25
#literature  [[backend]] [[python]] [[computer science]] [[networking]] [[tcp ip and protocols]]