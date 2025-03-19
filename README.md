Youtube 1.25 default speed
```// ==UserScript==
// @name         YouTube Default 1.25x Speed (Except Live Streams)
// @namespace    http://tampermonkey.net/
// @version      1.0
// @description  Sets YouTube videos to 1.25x speed by default, but doesn't affect live streams
// @author       You
// @match        https://www.youtube.com/*
// @grant        none
// ==/UserScript==

(function() {
    'use strict';
    
    // Function to check if the current video is a live stream
    function isLiveStream() {
        const liveBadge = document.querySelector('.ytp-live-badge');
        return liveBadge !== null;
    }
    
    // Function to set playback speed if not a live stream
    function setPlaybackSpeed() {
        const video = document.querySelector('video');
        if (video && !isLiveStream()) {
            video.playbackRate = 1.25;
        }
    }
    
    // Handle navigation within YouTube (SPA behavior)
    let lastUrl = location.href;
    new MutationObserver(() => {
        if (location.href !== lastUrl) {
            lastUrl = location.href;
            // Wait for video player to load after navigation
            setTimeout(setPlaybackSpeed, 2000);
        }
    }).observe(document, {subtree: true, childList: true});
    
    // Handle initial page load and video player ready events
    document.addEventListener('yt-navigate-finish', function() {
        setTimeout(setPlaybackSpeed, 2000);
    });
    
    // Periodically check to ensure setting is applied (helps with ads, etc.)
    setInterval(setPlaybackSpeed, 5000);
})();
