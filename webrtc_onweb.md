# Get stream from browser

=> we have to get the video and audio streams from the browser

const localStream = await navigator.mediaDevices.getUserMedia({video:true,audio:true}) // for the both video and audio

=> to stop or resume audio

localStream.getAudioTracks()[0].enabled=true

=> to stop or resume video

localStream.getVideoTracks()[0].enabled=true
