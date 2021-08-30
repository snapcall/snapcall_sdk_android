# CHANGELOG

## 2.8.2
## Fixed
- fix hangup on click on refuse in notification

## 2.8.1
## Fixed
- fix a bug where remote video was sometimes not displayed
- local video rotate with device

## 2.8.0
## Added
- public getter for callID in SCCall event
- Event onUpdateUI and api updateUI to apply props change

## Fixed
- video renderer not released properly with updatUI
- set microphone active at call start
- adapt button size for small screen width, and avoid label overlaps

## 2.7.0
## Added
- event on display / destroy of UI #88
- hide and show call controller with swipe gesture #87
- Live chat auth with token #85
- event LocalVideoInfo #84

## Fixed
- fix info payload for video activation #84

## 2.6.0
### Added 
- customisation of notification

### Fixed 
- agent mail correct in event 
- reconnect for video activation only once 
- landscape pictures in pictures
- ring event added when agent receive a call


## 2.5.1
## Fixed
- api payload fix

## 2.5.0
### Added
- Partner API
- new default user interface
- video call support
- agent api 

## 2.4.0
### Fixed
- media connection issue on some network.

## 2.3.12
- timeout improvment
## 2.3.11
## fixed 
- issue about media connection in some network.

## 2.3.10
### fixed 
- fixed a null pointeur exeception in a brodcast listener.

## 2.3.9
### fixed
- Network listener small fix. 

## 2.3.8
- added a back button on default ui if needed.

## 2.3.7
- fixed crash when reconnect before event about agent.

## 2.3.6
- fixed held and transfert status during internet shutdown
- added Default ui logo can be in res folder.

## 2.3.5
- new Event send each second with the call duration.
- deleted Manifest permission.
- network event for data fixed.  

## 2.3.4
- fixed held event
- hangup do not fire an error anymore.

## 2.3.3
- receive data from your agent 

## 2.3.0
- now use java 8
- now depend on library "org.webrtc:google-webrtc:1.0.27771"
- New Event System that let you make a call UI that feet your app.
- Agent email can be received.

## 2.2.0
- zendesk format for json
- context externe is a JSON

## 2.1.0
- re-work Event listner to be more easy to use and similiar on both platform
- Timer Implementation for the application listener  like ios
- background color modifications
- uiend event

## 2.0.0
- bid call
- push call
- inter app call
- customisation
- Add CHANGELOG.md
