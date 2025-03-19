Youtube 1.25 default speed
```// ==UserScript==
// @name         YouTube Default Playback Speed
// @namespace    https://youtube.com/
// @version      1.1
// @description  Automatically sets YouTube playback speed to 2.5x, excluding live streams.
// @author       Pepega
// @match        https://www.youtube.com/*
// @grant        none
// ==/UserScript==

(function () {
    'use strict';

    const DEFAULT_SPEED = 1.25;

    function isLiveStream() {
        // Check for the "LIVE" badge; YouTube uses a specific indicator for live streams
        const liveBadge = document.querySelector('.ytp-live-badge');
        return liveBadge && liveBadge.offsetParent !== null;
    }

    function setPlaybackSpeed(video) {
        if (video && !isLiveStream() && video.playbackRate !== DEFAULT_SPEED) {
            video.playbackRate = DEFAULT_SPEED;
            console.log(`Playback speed set to ${DEFAULT_SPEED}x`);
        }
    }

    function handleNewVideo() {
        const video = document.querySelector('video.html5-main-video');
        if (video) {
            setPlaybackSpeed(video);
        }
    }

    function initObservers() {
        // Observe DOM changes for dynamically loaded pages (YouTube's SPA behavior)
        const observer = new MutationObserver(() => {
            handleNewVideo();
        });

        observer.observe(document.body, { childList: true, subtree: true });

        // Handle YouTube's navigation events (yt-navigate-finish)
        window.addEventListener('yt-navigate-finish', () => {
            setTimeout(() => {
                handleNewVideo();
            }, 500); // Delay to ensure the video element is loaded
        });
    }

    // Initial setup for the video on page load
    window.addEventListener('load', () => {
        setTimeout(() => {
            handleNewVideo();
        }, 1000);
    });

    // Initialize observers
    initObservers();
})();
