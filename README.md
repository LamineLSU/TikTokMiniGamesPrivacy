# TikTokPrivacyCostofFun

## Android App Traffic Analysis with Burp Suite
```
This guide outlines the step-by-step process for setting up Burp Suite with an Android emulator (Genymotion) to capture and analyze network traffic from mobile applications—specifically in the context of studying WebView-based games and privacy behaviors.
```
### Requirements
- `-- Burp Suite Professional (Professional Edition)`
- `-- JS Miner plugin for Burp Suite`
- `-- Genymotion Android Emulator`
- `-- Internet-connected desktop PC and Genymotion emulator, on the same Wi-Fi network`

### Step-by-Step Setup
#### Burp Suite Configuration
- `-- Install and launch Burp Suite Professional on your desktop`
- `-- Enable the Proxy > Intercept > Intercept is on option`
- `-- Go to Proxy > Options > Proxy Listeners and ensure it's listening on the desired interface (e.g., 127.0.0.1:8080)`
- `-- Install and activate the JS Miner plugin if analyzing JavaScript resources is required`
#### Configure Genymotion Android Emulator
- `-- Launch your Genymotion device`
- `-- Go to Wi-Fi settings > hold on the connected network > tap Modify network`
- `-- Set the proxy to Manual:`
    - ` -- Proxy hostname: IP address of the host PC running Burp Suite`
    - ` -- Proxy port: 8080 (or your Burp listener port)`
### Install Burp Certificate on Genymotion
- ` -- In Burp, visit http://burp in the Genymotion browser`
- ` -- Download the CA certificate (named cacert.der)`
- ` -- Rename the certificate to cacert.cer`
- ` -- In Genymotion, go to Settings > Security > Install from SD card, and install the certificate`
- ` -- Use Frida to bypass SSL pinning for modern apps`
### Traffic Monitoring
- ` -- Launch the TikTok app , Begin gameplay and interactions`
- ` -- Burp Suite will now capture: HTTP/HTTPS requests and responses, WebSocket messages, Redirects and external resource loads, API calls`


## Frida SSL Pinning Bypass Setup for Android Apps (In our case TikTok)
### Requirements
- `-- Rooted Android device(Genymotion emulator)`
- `-- Frida & Python installed on host machine`
- `-- Platform-tools (ADB)`
- `-- Burp Suite (Pro)`
- ` -- Target Android APK or app installed on the emulator`
- ` -- CA Certificate from Burp`

### Step-by-Step Setup
####  Install & Configure Emulator
####  Install Python Tools (Install Python and required Frida packages)
```
$ pip install frida frida-tools objection
```
#### Install Platform Tools
#### Download SSL Pinning Bypass Script (Save this Frida Script as fridascript.js)
####  Connect Device via ADB
```
adb connect <device_ip>:5555
adb devices

```
#### Setup Frida Server
```
Find device architecture : adb shell getprop ro.product.cpu.abi
```
#### Push and run server
```
adb push frida-server /data/local/tmp
adb shell chmod 777 /data/local/tmp/frida-server
adb shell /data/local/tmp/frida-server &
```
#### Push burp certificate to device
```
adb push cacert.der /data/local/tmp/cert-der.crt
```
#### Inject Frida Script
```
adb push fridascript.js /data/local/tmp
```
#### Find target app’s package name
```
frida-ps -U
```
#### Launch app with Frida script injection
```
frida -U -f com.zhiliaoapp.musically -l /data/local/tmp/fridascript.js --no-pause
```

## GPT Prompt Engineering for Privacy Analysis in Mini-Games
#### Designing, Refining, and Validating Prompts for ChatGPT-4o in Gameplay Privacy Investigations

```ChatGPT-4o was integrated to semantically analyze: Gameplay interactions, Network trace logs (via Burp Suite), JavaScript/JSON behavior (via JS Miner)```
#### How We Designed Our Prompts
```
Identify Target Insight
- Each prompt was anchored to a specific analytical goal, such as:
    - Comparing policy promises vs real data collection
    - Summarizing privacy implications if user consent was missing
    - Isolating sensitive endpoints
```
#### Start with Open-Ended Drafts
```
We began with broad questions like: What are the discrepancies found in the network trace vs privacy policy document?
Identify the data collection practice related to mini-games in TikTok's Privacy Policy.
N:B These early drafts often resulted in vague or overly general responses.
```
#### Iterative Refinement
```
Prompts were reworded with explicit structure and classification expectations.
- Intial prompt : Identify the user data collected during gameplay.
- Refined Prompt: Clearly identify which user data types were:
- Clearly disclosed
- Ambiguous
- Omitted
- Incorrectly stated
Base your answer on the comparison between gameplay behavior and TikTok’s Privacy Policy.
N:B Improvement: This added structure improved consistency and reduced illusions
```
#### Connect with JS miner 
```
We paired GPT with Burp Suite’s JS Miner extension to extract:
- Inline JavaScript behaviors
- JSON data usage
After JS Miner surfaced possible data points, ChatGPT was prompted:
- What data is collected during gameplay that isn’t mentioned in the privacy policy?
- Are there any sensitive APIs in the network traffic during gameplay compared to non-gameplay?
- What privacy risks could arise if certain information isn’t disclosed?
```
#### Validation Process
```
Manual review : Cross-checking with actual policy text ( Only responses matching real network data and policy statements were accepted)
```


