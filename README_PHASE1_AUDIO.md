# ProxiSpeak Phase 1 — Spatial Audio

## What changed
- Added `src/audio/AudioEngine.js` to route remote WebRTC audio elements through Web Audio.
- Added `src/audio/attenuation.js` for smooth distance-based gain and horizontal stereo pan.
- Added `src/audio/attenuation.test.js` for calculation tests.
- Updated `App.jsx` to attach remote audio elements to the engine, update gain/pan when avatar positions change, and clean up audio nodes when peers disconnect.
- Existing Socket.IO signaling and WebRTC peer negotiation remain in place.

## Important behavior
- Distance attenuation is smoothstep: full volume at 0px, zero at/above 100px.
- Stereo pan maps horizontal avatar displacement into [-1, 1].
- Web Audio is applied to REMOTE audio playback. The local microphone stream is still sent through the existing WebRTC peer connections.
- If StereoPannerNode is unavailable, gain attenuation still works.
- Browser audio contexts may require a user gesture; the existing microphone button resumes the context.

## Install
Copy `client/src/App.jsx`, `client/src/main.jsx`, `client/src/styles.css`, and the `client/src/audio/` folder into your existing project. Keep `server/` unchanged.

## Test calculation helpers
From `client`:
```bash
node --test src/audio/attenuation.test.js
```

## Run app
Terminal 1:
```bash
cd server
npm install
npm start
```
Terminal 2:
```bash
cd client
npm install
npm run dev
```

Use two browser sessions in the same room, enable microphones, and move avatars left/right and closer/farther apart.
