![](attachments/hhs_banner-22kwugjbkc2qquqclz5kwif4srie.webp) <a id="home"></a>

# Hobart Hackerspace wiki ([wiki.local](#home))

This is our documentation store. This is **your** place to stash records of how to use our gear and what projects/experiments  you've done. (We trust you not to stuff it up or misuse it.) 

In the spirit of our *Make -- Learn -- Inspire* motto, use it to:

- **learn** from others' experience, 
- share how you **made** something (including the gory details *and* the mistakes) 
- **inspire** others to explore and create!

Connect from within our local network at: [wiki.local](#home). From outside, it's [wiki.hobarthackerspace.org.au:7008](http://wiki.hobarthackerspace.org.au:7008) (`http` only -- no `https` yet).
Creating or altering pages from outside the Space will require you to log in. This is to prevent malicious damage. [Ask a Committee member](mailto:committee@hobarthackerspace.org.au?subject=Wiki%20access) for the password.

# Contents
(If what you're looking for isn't obvious, use the search bar above.)

## Safety
- [Basic rules](https://hobarthackerspace.org.au/safety/)
- [Remember our core terms of use](https://hobarthackerspace.org.au/terms_of_use/)
- If you see an issue that needs attention, please [report it to the Committee](mailto:committee@hobarthackerspace.org.au?subject=Safety%20issue&body=OH%26S%20Incident%20Report). Please use an Incident Report form, either paper or electronic. Paper forms are in the labelled box opposite the main entrance door. Electronic forms are either [PDF](https://hobarthackerspace.org.au/assets/wiki-assets/Incident_Report_Form.pdf) or [MS Word](https://hobarthackerspace.org.au/assets/wiki-assets/Incident_Report_Form.docx).

## Our Building
Our home is a shared space and a heritage building. Please respect these when using it.
The heritage issue means that we have to be careful how we look after the building, especially the older stone parts.

### Sharing the Space safely
[We have a set of guidelines for all those who use our Space](Sharing%20our%20Space.md)

### Leaving the building
[Here's a checklist to follow when leaving, to make sure the building is left safe & secure.](Building%20departure%20checklist.md)

## Our Equipment
- [CNC Router](CNC%20Router.md)
- [Laser Cutting](Laser%20Cutting.md)
- [3D Printing](3D_Printers.md)
- [Electronics](Electronics%20Lab.md)
- [Radio](Radio%20Station.md)


## Events
- [Open Night](https://hobarthackerspace.org.au/events/open-night/)
- [Projects Night](Projects%20Night.md)
- [Festival of Bright Ideas (FOBI)](Festival%20of%20Bright%20Ideas%20FOBI.md)

## Systems
- [Our website](https://hobarthackerspace.org.au)
	- [Website content & management](https://github.com/hobarthackerspace/hobarthackerspace.github.io)
	- [Public calendars](https://hobarthackerspace.org.au/events/calendar/)
		- We have calendars on our website showing event bookings and space bookings.
		- Committee members can add items to those calendars so that others can know what's happening when.
		- [Adding an event](Public%20calendars.md).
- This wiki
	- [Adding your content](How%20to%20use%20the%20wiki.md). The easy way to add content is to start with another page and copy its source into your new page.
	- [The wiki server and its software](The%20Wikmd%20server.md)
- Home Assistant
	- [Home assistant](https://office.hobarthackerspace.org.au:8123) gives us access to our security system - the alarms, video cameras, etc. Committee members can have access to it if they ask.
	- Details of our [Home Assistant setup & configuration](Home%20Assistant%20setup.md) are [here](Home%20Assistant%20setup.md).
- CCTV cameras
	- Most of our monitoring cameras are connected to a Hikvision NVR. Ask for permission to access that directly.
	- More recent cameras [are documented here](Video_Cameras.md).

## Administration
- These are links for Committee members access the various systems that we use to look after the Hackerspace. 
	- Committee documents are in our [OneDrive/Microsoft365 document store](Microsoft%20365%20and%20OneDrive%20file%20access.md)
	- [Management of Members' RFID tags](Members%20Door%20RFID%20Tags.md)
	- [Membership records](Membership%20records.md)
	- The [Door access controller](Door%20access%20controller.md) runs on a Rasperry Pi.
	- We use the [GnuCash accounting system](HHS%20GnuCash%20Notes.md) to manage our accounts.
		- Our books are open to all members on request. Just [ask the treasurer](mailto:treasurer@hobarthackerspace.org.au?subject=Request%20to%20look%20at%20financial%20reports&body=Please%20send%20me%20a%20link%20to%20the%20current%20financial%20reports) for access to our recent reports.
	- Networks (LAN, WAN & Internet)
		- [Internet connection](Internet%20connection%20and%20firewall.md)
		- Internal (LAN) network and WiFi
			- [Ethernet network](Ethernet_network.md)
			- [WiFi and router](WiFi7%20upgrade%202025.md)
		- [Domain Names and DNS records](Domain%20Name%20records.md)
		- [Exposed IP ports](Internet%20connection%20and%20firewall%23static-ip-assignments-and-exposed-ip-ports.md)
	- Branding assets (logo, letterhead, etc etc)
		- Logo ![](attachments/Hacky_source_7x11-kxzr34ldff7ygkwwnhxid75mnoh4.svg){width="27px"}
			- Our logo is called *Hacky* -- a friendly take on the skull & crossbones, courtesy of Shane. It's inspired, we're told, by the *Sea Shepherds'* logo. [Here's a page of usable image files.](Hacky_images.md)
		 - QR Codes
			- We're moving to set up QR codes pointing to all our equipment documentation pages. [Here's a page showing how to make one, customised for a particular page and with *Hacky* in the middle.](QR_Codes.md)
		 - Letterhead
		 	- Templates for our letterhead are available for download in both `MS Word` and `LibreOffice` formats:
				- [MS Word](attachments/Letterhead_v1.0.dotx)
				- [LibreOffice](attachments/Letterhead_v1.0.ott)
			- Please be aware that the letterhead templates are intended only for use on Hackerspace business. 
			  Any other use will be treated as the crime of fraud.
	- Building issues
		- Maintenance
			- If there are urgent major issues, contact the Health Department site manager:
				- Mike Rowley
				- ‭p: 0417 316 173‬
				- e: [michael.rowley@dhhs.tas.gov.au](mailto:michael.rowley@dhhs.tas.gov.au)
			- In an after hours emergency at Hackerspace we need to call the RHH switchboard (6166 8308) and ask for Facilities On Call
		- Electricity distribution
			- [Here is a plan of the power outlets and circuits](power_distribution.md)

# This wiki
Our wiki runs on Wikmd software. It’s a file-based wiki that aims for simplicity. 
The documents are completely written in Markdown -- a markup language you're probably already 
familiar with (or can learn very quickly if you're not). 
Details on how to use it are in the [How to use this wiki](How%20to%20use%20the%20wiki.md) page. 

The base system is maintained by [Brian Marriott](mailto:brian.marriott@hobarthackerspace.org.au) -- 
but it's up to all members to provide the content! Just click the ![](attachments/Wiki_New_button_sml-len6lxa3gbd62fhxfwpss3jk35ko.png) button above.
If you have suggestions for improvement, 
[email Brian](mailto:brian.marriott@hobarthackerspace.org.au) at the moment -- 
in future you'll be able to add them to a Github tickets page.

The source repository of the Wikmd software is at [github.com/Linbreux/wikmd](https://github.com/Linbreux/wikmd) and the documentation is at [linbreux.github.io/wikmd](https://linbreux.github.io/wikmd/).

*BWM*

- Transcoded from WikMD on Tue Nov 25 12:13:31 2025
	- 114 lines written, 28 changed.
	- Lines: 29, 32, 35, 36, 37, 38, 39, 44, 45, 53, 55, 56, 59, 62, 66, 67, 68, 69, 70, 73, 75, 76, 77, 78, 81, 83, 98, 104
