# Project Configuration
In order for this project to be functional in editor and on device there is some initial setup that needs to be done.

## Application Configuration
In order to run the project and use the platform services we need to create an application on the [Meta Quest Developer Center](https://developers.meta.com/horizon/).

To run on device you will need a Quest application, and to run in editor you will need a Rift application. The following sections will describe the configuration required for the application to run.

### Data Use Checkup
To use the features from the Platform we need to request which kind of data is required for the application. This can be found in the _Data Use Checkup_ section of the application.

![data use checkup](./Media/dashboard/datausecheckup.png "Data use Checkup")

And configure the required Data Usage:
* **User Id**: Avatars, Destinations, Multiplayer, Oculus Username, Friends Invites, User Invite Others
* **In-App Purchases**: IAP
* **User Profile**: Avatars
* **Avatars**: Avatars
* **Deep Linking**: Destinations
* **Friends**: Multiplayer
* **Blocked Users**: Other - we use the Blocking API
* **Invites**: Multiplayer, Friends Invite, User Invite Others

### Add-ons
This application integrates in-app purchases (IAP) to demonstrate how to integrate durable and consumable purchases. Here are the configurations expected for this application.

First we need to open the Add-ons from the Platform Services:

![Platform Services Add-ons](./Media/dashboard/dashboard_addons_platformservices.png "Platform Services Add-ons")

Then we need to setup the different Add-ons.

![Add-ons](./Media/dashboard/dashboard_addons.png "Add-ons")

We created 3 durable SKUs, meaning we can only purchase them once, for the icons.
And 1 consumable SKU, meaning we can purchase it multiple time, for the pet cat.

### Destinations
This application uses Destination configuration to enable users to invite friends in the same arenas and launch the application together.

First we need to open the Destinations from the Platform Services:

![Platform Services Destinations](./Media/dashboard/dashboard_destinations_platformservices.png "Platform Services Destinations")

Then we need to setup the different destinations.

![Destinations](./Media/dashboard/dashboard_destinations.png "Destinations")

#### Main Menu
First off we have the MainMenu destination, which is a destination unique to the user where no other player can join them. It is setup like this:

![Destination Main Menu](./Media/dashboard/dashboard_destination_mainmenu.png "Destination Main Menu")

You will notice that the deeplink type is set to DISABLED, which prevents users from joining this destination together.

#### Arenas
Then we have different Arenas for each region:

![Destination NA](./Media/dashboard/dashboard_destination_na.png "Destination NA")

This is an example for the North American region. The deeplink type is set to ENABLE and we use the data of the deeplink to specify the Photon region to use. The format of the deeplink message is specific to our project. You can also note that the Group Launch capacity is set. As we have a maximum of 6 players in an Arena, the maximum is set to 6. (Note that the maximum value must be set to enable group launch.)

Here is a table for destinations settings:
<div style="margin: auto; padding: 10pt;">
<table>
<tr>
    <th>Destination</th>
    <th>API Name</th>
    <th>Deeplink Message</th>
</tr>
<tr>
    <td><b>Main Menu</b></td>
	<td>MainMenu</td>
	<td><i>N/A</i></td>
</tr>
<tr>
	<td><b>North America</b></td>
	<td>Arena</td>
	<td>{"Region":"usw"}</td>
</tr>
<tr>
    <td><b>South America</b></td>
	<td>ArenaSA</td>
	<td>{"Region":"sa"}</td>
</tr>
<tr>
	<td><b>Japan</b></td>
	<td>ArenaJP</td>
	<td>{"Region":"jp"}</td>
</tr>
<tr>
    <td><b>Europe</b></td>
	<td>ArenaEU</td>
	<td>{"Region":"eu"}</td>
</tr>
<tr>
	<td><b>Australia</b></td>
	<td>ArenaAU</td>
	<td>{"Region":"au"}</td>
</tr>
<tr>
    <td><b>Asia</b></td>
	<td>ArenaAsia</td>
	<td>{"Region":"asia"}</td>
</tr>
</table>
</div>

### Set the Application ID
We then need to the set the application ID in our project in Unity.

The identifier (__App ID__) can be found in the _API_ section.

![Application API](./Media/dashboard/dashboard_api.png "Application API")

Then it needs to be placed in the [Assets/Resources/OculusPlatformSettings.asset](Assets/Resources/OculusPlatformSettings.asset)

![Oculus Platform Settings Menu](./Media/editor/oculusplatformsettings_menu.png "Oculus Platform Settings Menu")

![Oculus Platform Settings](./Media/editor/oculusplatformsettings.png "Oculus Platform Settings")

## Photon Configuration

To get the sample working, you will need to configure Photon with your own account and applications. The Photon base plan is free.
- Visit [photonengine.com](https://www.photonengine.com) and [create an account](https://doc.photonengine.com/en-us/realtime/current/getting-started/obtain-your-app-id)
- From your Photon dashboard, click "Create A New App"
  - We will create 2 apps, "Realtime" and "Voice"
- First fill out the form making sure to set type to "Photon Realtime". Then click Create.
- Second fill out the form making sure to set type to "Photon Voice". Then click Create.

Your new app will now show on your Photon dashboard. Click the App ID to reveal the full string and copy the value for each app.

Open your unity project and paste your Realtime App ID in [Assets/Photon/Resources/PhotonAppSettings](Assets/Photon/Resources/PhotonAppSettings.asset).

![Photon App Settings Location](./Media/editor/photonappsettings_location.png "Photon App Settings Location")

![Photon App Settings](./Media/editor/photonappsettings.png "Photon App Settings")

Set the Voice App Id on the VoiceRecorder prefab:

![Photon Voice Recorder Location](./Media/editor/photonvoicerecorder_location.png "Photon Voice Recorder Location")

![Photon Voice Settings](./Media/editor/photonvoicesetting.png "Photon Voice Settings")

### Additional Setup for Realtime
In the case of our project we wanted players to be able to play in different regions with reduced latency, therefore we used the different regions that Photon provided. But in order to make the project simpler we limited the number of regions we would use. To do so we need to whitelist which regions are used by the application.

![Photon Whitelist](./Media/photon_whitelist.png "Photon Whitelist")

The Photon Realtime transport should now work. You can check the dashboard in your Photon account to verify there is network traffic.

## Upload to release channel
To use the platform features, you will first need to upload an initial build to a release channel.
For instructions you can go to the [developer center](https://developers.meta.com/horizon/resources/publish-release-channels-upload/). Then to be able to test with other users you will need to add them to the channel, more information in the [Add Users to Release Channel](https://developers.meta.com/horizon/resources/publish-release-channels-add-users/) topic.

Once the initial build is uploaded you will be able to use any development build with the same application Id, no need to upload every build to test local changes.
