# Google-News-Google-weather-Link-replacer
Replaces the "Google weather" Redirect with any weather link you choose. 
Replace the Link on line 20 with the weather provider of your choice. (In between " Qoutes" 

For Greasemonkey or similar extensions 
// ==UserScript==
// @name         Google News Specific Weather Link Redirect
// @namespace    http://tampermonkey.net/
// @version      1.0
// @description  Redirects the specific Google News weather link to Weather.com
// @author       Jake
// @match        https://news.google.com/*
// @grant        none
// @run-at       document-end
// ==/UserScript==

(function() {
    'use strict';

    const targetWeatherUrl = 'https://weather.com/weather/today/l/91fbf9a8c4bbb6fda244cb9d5b78c04221fd40732db9334504034f2113782172](https://weather.com/weather/today/l/c12593460c4d91961584b9b7d2f5639edbb00d5fa0462c7b039f5d4569454c61';

    function replaceWeatherLink() {
        // Target the specific weather link using the provided HTML path
        const weatherLink = document.querySelector(
            'body#yDmH0d div.T4LgNb main.IKXQhd c-wiz.XPbmfb section.XhbDsd div.UJdj6 c-wiz.yPI8Rb div.WCkv8e.xIBui div.CcMACc div.dSBEqc div#c19.yINXKd div.hJ2BFb a.Fqws6b'
        );

        if (weatherLink) {
            // Prevent Google's default behavior and redirect
            weatherLink.addEventListener('click', function(e) {
                e.preventDefault();
                window.location.href = targetWeatherUrl;
            });
            // Also set the href directly
            weatherLink.href = targetWeatherUrl;
        }
    }

    // Initial run
    replaceWeatherLink();

    // Handle dynamic content loading
    const observer = new MutationObserver(function(mutations) {
        replaceWeatherLink();
    });

    observer.observe(document.body, {
        childList: true,
        subtree: true
    });

    // Cleanup on page unload
    window.addEventListener('unload', function() {
        observer.disconnect();
    });
})();
