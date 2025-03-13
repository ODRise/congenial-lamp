Youtube 1.25 default speed
```// ==UserScript==
// @name         YouTube Auto Playback Speed 1.25
// @namespace    http://tampermonkey.net/
// @version      1.0
// @description  Automatically sets YouTube playback speed to 1.25x
// @author       Your Name
// @match        https://www.youtube.com/*
// @match        https://youtube.com/*
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    function setPlaybackSpeed(video) {
        if (video.playbackRate !== 1.25) {
            video.playbackRate = 1.25;
        }
    }

    function checkAndSetSpeed() {
        const video = document.querySelector('video');
        if (video) {
            setPlaybackSpeed(video);

            // If playback speed changes (by YouTube), reapply the speed
            video.addEventListener('ratechange', () => {
                setPlaybackSpeed(video);
            });
        }
    }

    // Observe page changes (for YouTube navigation without full page reload)
    const observer = new MutationObserver(checkAndSetSpeed);
    observer.observe(document.body, { childList: true, subtree: true });

    // Initial check
    checkAndSetSpeed();

})();
