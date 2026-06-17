# System Requirements Specification

## Content
This SRS follows the template defined by the IEEE standard [[1]](#ref-1). Accordingly, it contains the following chapters:
1. Introduction and Purpose
2. Business Requirements
3. Roles
4. Feature List
5. Use Cases
6. User Requirements
7. Functional Requirements
8. Nonfunctional Requirements
9. Interface Requirements
10. Performance Requirements
11. Security Requirements
12. Design and Implementation Constraints
13. External System Requirements
14. Quality Assurance Requirements
15. Documentation Requirements
16. Bibliography

## 1. Introduction and Purpose
### 1.1 Project Background
This project is developed in collaboration with CV de Trikvögel and WAVE Entertainment. WAVE Entertainment is a company that provides audio and visual support for events [[2]](#ref-2). The company also provides these services for the carnival group “de Trikvögel.” During the carnival season in the southern part of Limburg, a province in the Netherlands, this carnival group participates in multiple carnival parades with about 75 members. Each year, the members of the group build a carnival wagon based on a specific theme. The wagon is then equipped with audio and visual equipment from WAVE Entertainment. During the parades, the wagon is displayed to the public and helps create atmosphere for the members of the carnival group, who follow the wagon on foot. Furthermore, this project will focus on the audio aspects of the setup on the carnival wagon.
 
The wagon's audio system consists of multiple festival-grade speakers with separate bass units, a microphone, and an amplifier, all connected to a laptop that controls the entire audio system. This laptop runs Spotify using a Spotify Premium account [[3]](#ref-3) and is provided with internet access through a smartphone cellular hotspot. Although Spotify is a relatively simple music player, it does provide access to a very large collection of songs. As one of the most widely used and popular music platforms, it also includes the newest and most popular songs. This is important because members of the carnival group often request currently popular songs. The DJ from WAVE Entertainment, who is also a member of CV de Trikvögel and joins the carnival parades, can control the music from the Spotify app on his smartphone. This is an essential aspect of the audio setup, as the DJ also walks behind the wagon during the parades. Through the Spotify app on his smartphone, he can instantly play specific songs, create a queue for upcoming songs, and skip songs when necessary. Although dedicated DJ software would offer far more features, it would complicate access to specific new songs that may be requested by other members of the carnival group, and not all DJ software supports remotely controlling the music from a smartphone at a distance.
 
### 1.2 Problem Statement
After participating in numerous parades, the DJ and several members of CV de Trikvögel identified a missing feature in the current audio setup. Most DJ software includes a soundboard for playing custom sound effects, but unfortunately Spotify does not. A soundboard would enable the wagon to play short, themed sound effects during the parade which would add more atmosphere and entertainment. For example, the 2025 theme was based on Shaun the Sheep. For this specific theme the wagon’s audio system could play a short sheep sound effect at specific moments during the parade. The DJ and several group members also thought it would be fun to include common phrases from the Limburg dialect, which are traditionally associated with Limburgs carnival.
 
No existing application fully addresses this need. Three candidate solutions — Sound Show [[4]](#ref-4), Voicemod [[5]](#ref-5), and Podcast Soundboard [[6]](#ref-6) — were analyzed and found insufficient for several of the following reasons:
 
- **Limited remote control.** Some solutions do not allow the DJ to truly control sound effects from his smartphone while the sounds play on the laptop connected to the audio system.
- **Inconsistent cross-platform support.** The laptop, connected to the audio system, may be either a Windows machine or an Apple MacBook, and the smartphone may be either an Android device or an iPhone. Some of the analyzed solutions do not support all combinations of these platforms.
- **No on-demand sound retrieval.** None of the solutions allow the DJ to directly search the internet from his smartphone remote to retrieve sound effects that are spontaneously requested by group members during a parade. The analyzed solutions only support sound effects that are first uploaded to the system, or support collections of general common sound effects. Sound effects in the Limburgs dialects are of course not included in these collections.
- **Cost and uncertainty.** The treasurer of the carnival group was reluctant to allocate part of the group's tight budget to paid licences when the suitability of these solutions for this specific use case had not been proven.

### 1.3 Proposed Solution
To address the stated problem, this project will deliver a custom Remote Soundboard system consisting of two synchronized instances:
 
- A **soundboard remote controller** running on the DJ's smartphone or on a Windows or macOS desktop computer, used to trigger and manage sound effects. Running the controller on a desktop or laptop provides a more convenient interface for uploading and organising sound effect files before a parade.
- A **soundboard audio host** running on the laptop connected to the audio system, which receives commands from the remote controller and plays the corresponding sound effects through the speakers.

The remote controller allows the DJ to directly play and control sound effects on the audio host, while walking behind the carnival wagon. The remote controller additionally supports retrieving and downloading new sound effects to the remote controller directly from the internet. This retrieval feature is implemented using agentic Artificial Intelligence (AI) workflows, which have recently demonstrated the ability to query and process data from the internet effectively [[7]](#ref-7). Because Spotify is used as the music player on the laptop, the system also integrates with Spotify to further control the audio system. The audio system should for example automatically lower the music volume while a sound effect is playing.

### 1.4 Document overview
The remainder of this document is structured as follows. Chapter 2 establishes the business requirements, describing the high-level goals and constraints that the Remote Soundboard system must satisfy from the perspective of CV de Trikvögel, WAVE Entertainment and the developer for this system. Chapter 3 defines the roles of all stakeholders involved, such as the DJ and the carnival group members, whose needs drive the requirements in subsequent chapters.

Chapter 4 provides a feature list that summarizes the capabilities of the Remote Soundboard system at a high level. Chapter 5 elaborates on these features through use cases, describing concrete interaction scenarios between the roles defined in Chapter 3 and the system. Chapter 6 then captures the user requirements derived from these use cases.

Chapters 7 through 11 specify the detailed requirements for the Remote Soundboard system. Chapter 7 defines the functional requirements describing what the system must do, while Chapters 8 through 11 address nonfunctional concerns: general quality attributes, interface specifications, performance constraints, and security requirements respectively.

Chapters 12 and 13 describe constraints imposed on the design and implementation, and the requirements on external systems the solution depends on. Finally, Chapters 14 and 15 address quality assurance and documentation requirements. The bibliography is provided in Chapter 16.

## 2. Business Requirements
### 2.1 CV de Trikvögel
- **BR1:** The Remote Soundboard system must enable the DJ to play sound effects on the audio system during carnival parades at any moment.
- **BR2:** The Remote Soundboard system must enable the DJ to play previously downloaded sound effects and custom recorded sound effects.
- **BR3:** The Remote Soundboard system must enable the DJ to quickly retrieve sound effects from the internet based on spontaneous requests from group members during a parade.
- **BR4:** The Remote Soundboard system must play sound effects through the audio system with a latency of at most 1000 milliseconds after the DJ triggers the sound effect.
- **BR5:** The cost of a single agentic sound retrieval search in the Remote Soundboard system must be at most €0.50.

### 2.2 WAVE Entertainment
- **BR6:** The Remote Soundboard system must not require technical expertise to operate.
- **BR7:** The soundboard remote controller must be operated from a smartphone while walking behind the carnival wagon during parades.
- **BR8:** The soundboard remote controller must be compatible with both Android and iOS smartphones, as well as with Windows and macOS desktop computers and laptops, to facilitate easier management of sound effect files.
- **BR9:** The soundboard audio host must be compatible with both Windows and macOS.
- **BR10:** The Remote Soundboard system must integrate with Spotify to control the music playback.

### 2.3 Developper
- **BR11:** The Remote Soundboard system will use European developed and hosted AI services whenever possible.
- **BR12:** The Remote Soundboard system will be developed for an open audience, but will be primarily designed for the specific use case of CV de Trikvögel and WAVE Entertainment.
- **BR13:** The operational costs of the Remote Soundboard system must be at most €0.5000 per year, which includes any costs for maintaining the domain, hosting a website and cloud services.

## 3. Roles
### 3.1 Members of CV de Trikvögel
CV de Trikvögel has 75 members with different roles and responsibilities within the carnival group. To start with, several members of the group have formed a building group. This building group is responsible for designing and building the carnival wagon aimed towards a specific theme each year. Based on this theme, the building group can come up with ideas for sound effects that would fit the theme and facilitate the atmosphere during the parades. These sound effected can be downloaded from the internet or recorded before the start of the parades. The Remote Soundboard system should then enable the DJ to play the sound effects on the audio system at specific moments during the parades.

Fortunately, there currently are enough members in the building group. The majority of the members of the carnival group are however not involved in the building process. These members only participate in the parades by walking behind the wagon and create the festive atmosphere. During the parades, these members will often request specific songs that match or will inhance the atmosphere. With the Remote Soundboard system, the members could also request specific sound effects that would fit the atmosphere.

Finally, some members of the carnival group have an organisational role. One of these members is the treasurer, who is responsible for managing the budget of the carnival group. The treasurer needs to make sure that all of the costs for the entire carnival group stay within the allocated budget. The treasurer has therefore drawn up a maximum budget for the operating costs of the Remote Soundboard system. 

### 3.2 DJ from WAVE Entertainment
The DJ from WAVE Entertainment is also a member of CV de Trikvögel and therefore also participates in the parades. The main responsibility of the DJ is to control the music that is played on the audio system on the carnival wagon during the parades. Because the DJ also walks behind the wagon during the parades, he controls the music from his smartphone using the Spotify app. The Remote Soundboard system should enable the DJ to also control sound effects from his smartphone while walking behind the wagon. When other members of the carnival group request specific sound effects during the parades, the DJ should be able to quickly retrieve these sound effects from the internet and play them on the audio system. Finally, the DJ should be able to control the music playback on Spotify from the Remote Soundboard system, for example by automatically lowering the music volume while a sound effect is playing.

## 4. Feature List
The Remote Soundboard system should include the following most important high-level features:
- **Remote Soundboard Control:** The DJ can trigger and manage sound effects on the audio system from his smartphone while walking behind the carnival wagon during parades, or from a desktop computer/laptop before the parade.
- **Upload Sound Effects:** The DJ can upload sound effects to the soundboard remote controller.
- **Agentic Sound Retrieval:** The DJ can use natural language to request specific sound effects from an agentic AI workflow. This agentic workflow will then retrieve the requested sound effect from the internet and download it to the soundboard remote controller.
- **Cross-Platform Compatibility:** The soundboard remote controller is compatible with both Android and iOS smartphones, and with Windows and macOS desktop computers and laptops. The soundboard audio host is compatible with both Windows and macOS.
- **Spotify Integration:** The Remote Soundboard system integrates with Spotify to control the music playback on the audio system, for example by automatically lowering the music volume while a sound effect is playing.

## 5. Use Cases
### 5.1 Playing prepared sound effects during a parade
The main use case for the Remote Soundboard system comes from the building group of CV de Trikvögel. As this building group is responsible for designing and building the carnival wagon, they also come up with ideas for sound effects that would fit the theme of the wagon and enhance the atmosphere during the parades. For example, the 2025 theme was based on Shaun the Sheep. For this specific theme the wagon’s audio system could play a short sheep sound effect at specific moments during the parade. Some members of the building group could then record or download the desired sound effects before the start of the parades. With the Remote Soundboard system, the DJ can then play these sound effects on the audio system at specific moments during the parades. This can be at places where a jury is positioned or at places where parts of the parades are recorded for television. In this way, the theme of the wagon can be more interactively expressed trough the wagon.

### 5.2 Playing spontaneously requested sound effects during a parade
While the majority of the members of CV de Trikvögel are not part of the building group, they do participate in the parades by walking behind the wagon and create the festive atmosphere. During the parades, these members are often very enthusiastic and want to participate in expressing the yearly theme of the wagon. They already often request specific songs that further enhance the atmosphere. With the Remote Soundboard system, these members can also spontaneously request specific sound effects for desired moments during the parades. As the DJ also walks behind the wagon during the parades, it is difficult for him to download the requested sound effects from the internet on his smartphone while walking and managing the music. The Remote Soundboard system therefore offloads this task to an agentic AI workflow. The DJ can simply describe the requested sound effect in natural language while the agentic AI workflow will then retrieve the requested sound effect from the internet and download it to the soundboard remote controller. The DJ can then play the requested sound effect on the audio system at the desired moment during the parade.
 
### 5.3 Managing sound effects from a desktop computer/laptop before a parade
Before the start of a carnival parade, the DJ wants to prepare the sound effects that are requested by the building group or sound effects that he wants to play during the parade. This preparation process is easier to do on a desktop computer or laptop than on a smartphone. Firstly, a desktop computer or laptop have a more accessible file system and provide more options for downloading the desired sound effects from the internet. Secondly, the larger screen and the more precise control on a desktop computer or laptop also make it easier to organise and manage the saved sound effects on the controller. The controller then allows the DJ to create easily recognisable names for the sound effects and color code them so he can always quickly find the desired sound effect during the parade. Once the parade begins, the DJ switches to the smartphone-based remote controller to trigger sound effects while walking behind the wagon.

## 6. User Requirements
### 6.1 Sound Effect Playback
- **UR1:** The DJ must be able to play a sound effect on the audio system by performing a single interaction on the soundboard remote controller.
- **UR2:** The DJ must be able to stop a currently playing sound effect from the soundboard remote controller.
- **UR3:** The DJ must be able to adjust the playback volume of a sound effect from the soundboard remote controller.
- **UR4:** The DJ must be able to see which sound effects are currently playing on the soundboard remote controller.

### 6.2 Sound Effect Management
- **UR5:** The DJ must be able to view all available sound effects on the soundboard remote controller.
- **UR6:** The DJ must be able to add a sound effect file from his device to the soundboard remote controller.
- **UR7:** The DJ must be able to record a custom sound effect using his device's microphone and add it to the soundboard remote controller.
- **UR8:** The DJ must be able to rename a sound effect on the soundboard.
- **UR9:** The DJ must be able to delete a sound effect from the soundboard.
- **UR10:** Sound effects added to the soundboard must remain available across sessions without requiring re-upload.
- **UR11:** The DJ must be able to assign a color to each sound effect button on the soundboard remote controller.
- **UR12:** The DJ must be able to assign a color to a sound effect group, which serves as the default color for all buttons in that group that do not have an individually assigned color.

### 6.3 Agentic Sound Retrieval
- **UR13:** The DJ must be able to describe a desired sound effect in natural language from the soundboard remote controller.
- **UR14:** The system must automatically search the internet and retrieve a sound effect matching the DJ's description without requiring any further manual action from the DJ.
- **UR15:** The DJ must be notified when the retrieved sound effect is ready for use.
- **UR16:** The DJ must be able to preview a retrieved sound effect before adding it to the soundboard.
- **UR17:** The DJ must be able to accept or discard a retrieved sound effect.

### 6.4 Spotify Integration
- **UR18:** The music volume must automatically decrease when a sound effect starts playing and return to its original level when the sound effect finishes.
- **UR19:** The DJ must be able to enable or disable the automatic volume reduction during sound effect playback.
- **UR20:** The DJ must be able to configure the degree of volume reduction applied during sound effect playback.

### 6.5 System Setup and Connection
- **UR21:** The DJ must be able to connect the soundboard remote controller to the soundboard audio host without requiring technical expertise.
- **UR22:** The DJ must be able to see whether the soundboard remote controller is connected to the soundboard audio host.
- **UR23:** The DJ must be able to sign in to the soundboard remote controller using their Google account.
- **UR24:** The DJ must be able to enter and save their personal API key for the AI service in the soundboard remote controller settings.

## 7. Functional Requirements
### 7.1 Sound Effect Playback
- **FR1:** The soundboard remote controller shall send a play command to the soundboard audio host when the DJ triggers a sound effect.
- **FR2:** The soundboard audio host shall begin playing the specified sound effect upon receiving a play command from an authenticated remote controller.
- **FR3:** The soundboard remote controller shall send a stop command to the soundboard audio host when the DJ stops a sound effect.
- **FR4:** The soundboard audio host shall immediately stop playing the specified sound effect upon receiving a stop command.
- **FR5:** The soundboard remote controller shall allow the DJ to adjust the playback volume of a sound effect and shall communicate this volume setting to the soundboard audio host.
- **FR6:** The soundboard remote controller shall visually indicate which sound effects are currently playing.
- **FR7:** The soundboard audio host shall support at minimum the MP3 audio file format.

### 7.2 Sound Effect Management
- **FR8:** The soundboard remote controller shall display all available sound effects as individually labelled, color-coded interactive elements in a grid or list layout.
- **FR9:** The soundboard remote controller shall allow the DJ to import a sound effect file from the device's local storage.
- **FR10:** The soundboard remote controller shall allow the DJ to record audio using the device's microphone and save the recording as a sound effect.
- **FR11:** The soundboard remote controller shall allow the DJ to rename any sound effect.
- **FR12:** The soundboard remote controller shall allow the DJ to delete any sound effect.
- **FR13:** The soundboard remote controller shall synchronise all sound effect files and metadata to the soundboard audio host after any addition, modification, or deletion.
- **FR14:** The soundboard remote controller shall persist all sound effects and their associated metadata between application sessions.

### 7.3 Agentic Sound Retrieval
- **FR15:** The soundboard remote controller shall provide a natural language input interface through which the DJ can describe a desired sound effect.
- **FR16:** Upon submission of a natural language description, the system shall invoke a multi-agent AI workflow to search the internet for a matching sound effect.
- **FR17:** The agentic AI workflow shall download the retrieved sound effect to the soundboard remote controller.
- **FR18:** The soundboard remote controller shall notify the DJ when the agentic AI workflow has completed retrieval of a sound effect.
- **FR19:** The soundboard remote controller shall allow the DJ to preview a retrieved sound effect before adding it to the soundboard.
- **FR20:** The soundboard remote controller shall allow the DJ to accept a retrieved sound effect, which shall then add it to the soundboard, or to discard it.
- **FR21:** The agentic AI workflow shall use European-developed and hosted AI services wherever technically feasible, in accordance with BR11.
- **FR22:** The soundboard remote controller shall provide a settings interface through which the DJ can enter and save their personal API key for the AI service used by the agentic sound retrieval feature.

### 7.4 Spotify Integration
- **FR23:** The soundboard audio host shall interface with the Spotify application running on the same laptop to control its playback volume.
- **FR24:** The soundboard audio host shall automatically reduce the Spotify playback volume when a sound effect begins playing.
- **FR25:** The soundboard audio host shall automatically restore the Spotify playback volume to its level prior to the reduction when all active sound effects have finished playing.
- **FR26:** The soundboard remote controller shall allow the DJ to configure the target volume level to which Spotify is reduced during sound effect playback.
- **FR27:** The soundboard remote controller shall allow the DJ to enable or disable the automatic Spotify volume reduction feature.

### 7.5 Connection and Communication
- **FR28:** The soundboard remote controller and soundboard audio host shall establish a persistent connection over the network for the exchange of commands, status updates, and sound effect files.
- **FR29:** The soundboard remote controller shall display the current connection status with the soundboard audio host.
- **FR30:** The system shall automatically attempt to re-establish the connection between the soundboard remote controller and the soundboard audio host if the connection is interrupted.
- **FR31:** The soundboard audio host shall reject commands from unauthenticated sources.

### 7.6 Sound Effect Color-Coding
- **FR32:** The soundboard remote controller shall allow the DJ to assign a color to each sound effect from a predefined color palette.
- **FR33:** The soundboard remote controller shall render each sound effect button in its assigned color.
- **FR34:** The soundboard remote controller shall allow the DJ to assign a color to a sound effect group; this color shall serve as the default for all sound effect buttons within the group that do not have an individually assigned color.

### 7.7 Authentication
- **FR35:** The soundboard remote controller shall require the DJ to sign in using a Google account before accessing any soundboard functionality.
- **FR36:** The soundboard remote controller shall maintain the authenticated session persistently, so that the DJ does not need to sign in again on subsequent application launches on the same device.

## 8. Nonfunctional Requirements
### 8.1 Usability
- **NFR1:** The soundboard remote controller must be operable without technical expertise, in accordance with BR6.
- **NFR2:** All text, labels, and interactive elements on the soundboard remote controller must remain legible under outdoor daylight conditions.
- **NFR3:** Error messages and system notifications displayed to the DJ must be written in non-technical language.
- **NFR4:** The colors available in the sound effect color palette must remain visually distinguishable from one another under outdoor daylight conditions.

### 8.2 Reliability
- **NFR5:** The Remote Soundboard system must remain continuously operational for the full duration of a carnival parade, which typically lasts between four and eight hours.
- **NFR6:** The soundboard audio host must recover from unexpected application errors without requiring a manual restart during a parade.
- **NFR7:** The soundboard remote controller must not lose any stored sound effects or configuration data as a result of an application crash or unexpected shutdown.

### 8.3 Maintainability
- **NFR8:** The system must be maintainable by the developer without requiring specialised hardware beyond a standard smartphone and a laptop.

## 9. Interface Requirements
### 9.1 User Interface
- **IR1:** The soundboard remote controller shall present a main screen with a grid-based layout of labelled, color-coded buttons, each representing one sound effect.
- **IR2:** The soundboard remote controller shall provide a dedicated interface for the agentic sound retrieval feature, clearly separated from the main soundboard.
- **IR3:** The soundboard remote controller shall provide a settings screen for configuring the connection to the soundboard audio host, the Spotify integration, and other preferences.
- **IR4:** The soundboard audio host shall display the current connection status and the name of any sound effect currently playing.

### 9.2 Hardware Interface
- **IR5:** The soundboard audio host shall output audio through the operating system's default audio output device, which is assumed to be connected to the carnival wagon's audio system.
- **IR6:** The soundboard remote controller shall use the device's wireless network interface to communicate with the soundboard audio host.
- **IR7:** The soundboard remote controller shall use the device's microphone hardware to support the custom sound effect recording feature described in FR10.

### 9.3 Software Interface
- **IR8:** The soundboard audio host shall interface with the Spotify desktop application using the Spotify Web API [[8]](#ref-8) to read and set playback volume.
- **IR9:** The soundboard remote controller and soundboard audio host shall communicate using a documented application-level protocol over HTTP or WebSocket.
- **IR10:** The agentic sound retrieval feature shall interface with a third-party AI service API that supports internet search and audio file retrieval capabilities.

## 10. Performance Requirements
- **PR1:** The soundboard audio host shall begin playing a sound effect within 1000 milliseconds of the DJ triggering it on the soundboard remote controller, measured from the moment the DJ's interaction is registered to the moment audio output begins, in accordance with BR4.
- **PR2:** The soundboard remote controller shall acknowledge a play or stop action to the DJ within 200 milliseconds of the interaction being registered.
- **PR3:** The agentic sound retrieval workflow shall provide a status update to the DJ within 5 seconds of the DJ submitting a retrieval request, confirming that the request has been received and is being processed.
- **PR4:** The cost of a single agentic sound retrieval request, including all AI API calls made during that workflow, shall not exceed €0.50, in accordance with BR5.
- **PR5:** The total annual operational cost of the Remote Soundboard system, including any domain, hosting, and cloud service costs, shall not exceed €0.5000, in accordance with BR13.

## 11. Security Requirements
- **SR1:** The soundboard audio host shall only accept and execute commands from a soundboard remote controller that has been authenticated using a shared secret or equivalent mechanism.
- **SR2:** API keys and credentials used by the Remote Soundboard system, including those for the Spotify API and the AI service API, shall be stored in a secure location on the respective host device and shall not be exposed through any user interface or transmitted in plaintext.
- **SR3:** User authentication in the soundboard remote controller shall be implemented using Google OAuth 2.0; no alternative authentication mechanism shall be accepted.

## 12. Design and Implementation Constraints
- **DC1:** The soundboard remote controller must be implemented as an application that runs on both Android and iOS smartphones and on Windows and macOS desktop computers and laptops, in accordance with BR8.
- **DC2:** The soundboard audio host must be implemented as a desktop application that runs on both Windows and macOS, in accordance with BR9.
- **DC3:** The system must use European-developed and hosted AI services wherever technically feasible, in accordance with BR11.
- **DC4:** The soundboard audio host must integrate with Spotify using the Spotify Web API, which requires an active Spotify Premium subscription on the laptop, in accordance with BR10.
- **DC5:** The system must be operable over the network created by the smartphone's cellular hotspot connection, which is also used to provide internet access to the laptop.
- **DC6:** The soundboard audio host must produce audio output through the operating system's default audio device without requiring any modifications to the existing audio hardware setup on the carnival wagon.
- **DC7:** The system must not depend on paid third-party services beyond the Spotify Premium subscription already in use and the budgeted AI service costs, in accordance with BR5 and BR13.
- **DC8:** User authentication in the soundboard remote controller must be implemented using Google Sign-In (Google OAuth 2.0), in accordance with SR3.
- **DC9:** The agentic sound retrieval feature must not include a built-in or shared API key for the AI service; each user must supply and configure their own API key, in accordance with FR22.

## 13. External System Requirements
- **ESR1:** The Spotify desktop application must be installed and running on the laptop connected to the audio system for the Spotify integration features described in FR23 through FR27 to function.
- **ESR2:** An active Spotify Premium account must be logged in to the Spotify application on the laptop, as the Spotify Web API requires a Premium subscription to control playback volume programmatically.
- **ESR3:** The device running the soundboard remote controller must have an active cellular data connection or access to a Wi-Fi network with internet connectivity for the agentic sound retrieval feature described in FR16 and FR17 to function.
- **ESR4:** The laptop running the soundboard audio host must be accessible over the same network as the device running the soundboard remote controller, either through the device's cellular hotspot or another shared network.
- **ESR5:** The AI service used for the agentic sound retrieval workflow must provide a publicly accessible API, must support internet search and audio file retrieval, must operate within the cost constraints of BR5, and must be preferably developed and hosted within Europe in accordance with BR11.
- **ESR6:** The AI service used for the agentic sound retrieval workflow must provide transparent per-request pricing to enable monitoring of costs against the limit defined in BR5.
- **ESR7:** The DJ must obtain and configure their own API key from the AI service provider before the agentic sound retrieval feature described in FR16 can be used.

## 14. Quality Assurance Requirements
- **QAR1:** The developer shall write and maintain automated unit tests covering the core logic of the soundboard remote controller and the soundboard audio host.
- **QAR2:** The developer shall perform integration tests verifying that the soundboard remote controller correctly triggers sound playback on the soundboard audio host across a network connection.
- **QAR3:** The developer shall perform end-to-end tests of the agentic sound retrieval workflow to verify that sound effects can be retrieved from the internet and added to the soundboard.
- **QAR4:** The soundboard remote controller shall be tested on at least one physical Android device, at least one physical iOS device, at least one Windows machine, and at least one macOS machine to verify the cross-platform compatibility required by DC1.
- **QAR5:** The soundboard audio host shall be tested on at least one Windows machine and at least one macOS machine to verify the cross-platform compatibility required by DC2.
- **QAR6:** The developer shall measure and document the end-to-end latency from triggering a sound effect on the soundboard remote controller to the start of audio output on the soundboard audio host under realistic network conditions, and shall verify that this latency does not exceed the limit defined in PR1.
- **QAR7:** Before the first carnival parade, the developer shall conduct a usability test with the DJ and at least one other member of CV de Trikvögel to verify that the system meets the usability requirements defined in NFR1 through NFR4.

## 15. Documentation Requirements
- **DR1:** The developer shall provide a user manual for the DJ describing how to use the soundboard remote controller, covering at minimum: triggering sound effects, managing sound effects, using the agentic sound retrieval feature, and configuring the Spotify integration.
- **DR2:** The developer shall provide an installation and configuration guide for setting up the soundboard audio host on the laptop.
- **DR3:** The developer shall provide a setup guide for configuring the connection between the soundboard remote controller and the soundboard audio host.
- **DR4:** All user-facing documentation shall be written in language accessible to non-technical users, in accordance with NFR3.
- **DR5:** The developer shall maintain technical documentation describing the system architecture, the communication protocol between the remote controller and audio host, and the integration points with external systems, to support future maintenance of the system.
 
## 16. Bibliography
<a id="ref-1"></a>[1]: IEEE Computer Society. *Software requirements specifications*.
      from https://www.computer.org/resources/software-requirements-specifications
 
<a id="ref-2"></a>[2]: WAVE Entertainment. *Home*.
     from https://wave-entertainment.nl/
 
<a id="ref-3"></a>[3]: Spotify. *Premium*.
     from https://www.spotify.com/nl/premium/
 
<a id="ref-4"></a>[4]: Herbin, L. *Sound Show — Free Soundboard Software*.
     from https://soundshow.app/
 
<a id="ref-5"></a>[5]: Voicemod Inc. *Voicemod: Free PC Meme Soundboard App*.
     from https://www.voicemod.net/en/soundboard/
 
<a id="ref-6"></a>[6]: Paterson, A. *Podcast Soundboard*.
     from https://podcastsoundboard.com/
 
<a id="ref-7"></a>[7]: Plaat, A., van Duijn, M., van Stein, N., Preuss, M., van der Putten, P., & Batenburg, K. J. *Agentic Large Language Models, a Survey*. arXiv preprint arXiv:2503.23037, 2025.
     from https://arxiv.org/abs/2503.23037
 
<a id="ref-8"></a>[8]: Spotify AB. *Spotify Web API Reference*.
     from https://developer.spotify.com/documentation/web-api