# Agora React & TS Web App Demo

This tutorial explains my process of going through the Quickstart for the React & TS Agora Web App and adding some extra functionality. For live demo, click [here](https://jolortiz.github.io/agora-video-react/).

### Quick Start

I began by cloning AgoraIO's [Basic Video Call repo](https://github.com/AgoraIO/Basic-Video-Call) and navigating to [Web Tutorial For React & TS - 1to1](https://github.com/AgoraIO/Basic-Video-Call/tree/master/One-to-One-Video/Agora-Web-Tutorial-1to1-React).

I then followed the instructions in the README:
1. Create a developer account at [agora.io](https://dashboard.agora.io/signin/).
2. Create a new project in the Dashboard, generate a token, and come up with a channel name.
3. Install the required packages for the project using npm.
4. Use npm to run dev environment and build for production.

After following these instructions I have the sample app up and running! To use the video call system take the App ID, Channel Name, and Temp Token and input them into the app to join the channel.
---

### Added Functionality

Now that I have my dev environment set up, It was time to look over the code and add some additional functionality. For this project I added Mute/Unmute capabilities as well as Enable/Disable Video capabilities. I implemented these in the exact same way so I will only cover Mute/Unmute here.

I began by adding a button component that I inserted into the header. This component is a Material-UI component and you can see the documenatation for it [here](https://material-ui.com/api/button/). This button reflects changes made to the `isMuted` variable that I added to the state. Dependent upon this variable, the button will call a function to mute or unmute the stream when it is clicked.
```javascript
const MuteUnmuteBtn = () => {
  return (
    <Button
      className={classes.buttonTop}
      color={isMuted ? "primary" : "secondary"} // themed colors
      onClick={isMuted ? unmute : mute}
      variant="contained"
      disabled={!isJoined || isLoading} // disable button if stream is not live
    >
      {isMuted ? "Unmute" : "Mute"}
    </Button>
  );
}
```

I then consulted the [documentation](https://docs.agora.io/en/Video/API%20Reference/web/index.html) to find out what methods I needed to mute and unmute the stream. I navigated to the `Stream` section, since I knew I would be manipulating the `Stream` object. I found the right methods and added them to my functions making sure to also update the state to reflect the changes.
```javascript
const mute = () => {
  try {
    if (localStream) {
      localStream.muteAudio(); // mute audio
      setisMuted(true); // update state
    }
  } catch(err) {
    enqueueSnackbar(`Failed to mute, ${err}`, { variant: "error" });
  }
}

const unmute = () => {
  try {
    if (localStream) {
      localStream.unmuteAudio(); // unmute audio
      setisMuted(false); // update state
    }
  } catch(err) {
    enqueueSnackbar(`Failed to unmute, ${err}`, { variant: "error" });
  }
}
```

`mute` and `unmute` were now functional and I could implement disable/enable audio in exactly the same way.
---

### Version Control & Deployment
I uploaded the source code to this repository, and deployed the app on GitHub Pages. My process is detailed in the steps below:
1. Navigate to your project folder and install GitHub Pages package as a dev-dependancy.
```shell
$ npm install gh-pages --save-dev
```
2. Open `package.json` and add a new property `homepage` with the URL for your project. The URL scheme will be as follows `"http://{your-username}.github.io/{repo-name}"`.
```
"homepage": "https://jolortiz.github.io/agora-video-react/",
```
3. Now add `predeploy` and `deploy` to the `scripts` property.
```
"scripts": {
  "predeploy": "npm run build",
  "deploy": "gh-pages -d build"
  // ...
}
```
4. Create a Git repository, intialize locally, and add it as a remote.
```shell
$ git remote add origin git@github.com:username/new_repo
```
5. You can now finally deploy!
```shell
$ npm run deploy
```
---
