+++
date = '2025-01-02T22:41:34+07:00'
draft = false
title = 'Smb_share'
+++

## First start
So basically, Windows requires you to config the "Sharing" and "Security" for shared it.Just create a folder and go to Properties, go to Sharing tabs edit in Advanced Sharing. Next, go to security and add permissions for it. Thrid, if you don't enable network share at first go to Control Panel -> Network and Internet -> Network and Sharing Center -> Advanced sharing settings and configures it (Recommend keep the password on).
- If you setup it properly the files you shared should show on \\\127.0.0.1 (Win + R).
## Now the best part
### Install zerotier for VLAN
By default how zerotier works kindly smart, when outside lan network it will act like a VLAN and transfer files with peak rates is your connection speed. See [fast.com](https://fast.com). On the other side, if it detect you and the server you connected with are in the same network [(LAN)](https://en.wikipedia.org/wiki/Local_area_network), the peak download rates will be how fast the router can handle (or limit by cable or wirelesss).
1. Connected the device you want to share on the same zerotier network.
2. If it worked properly, use `` \\<ip addresses> `` which is the ip for the computer contain share folder
## Enchance for more privacy
If you use the default settings, theoretically it still have some security issuses like if someone know the password of your computer and you two are on the same network, they can secretly access your data because the leaks account/password and the network you are using.
- In control panel, selected only share on private network. Move zerotier to there and remember to choose all other kinds of network is Public network (Maybe affect the local transfer rates because of **not testing yet**)
- Create a dummy account, open cmd by administrator``net user dummy password /add``, choose whatever ``username`` and ``password`` you like, i like to keep it as dummy and just change the password. Store the account somewhere god will not reach it, too.
- (Optional) User regedit and changing the value in ``HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon``
	1. Right-click Winlogon, select New > Key, and name it SpecialAccounts.
	2. Inside the SpecialAccounts key, create another new key named UserList.
	3. Select the UserList key, right-click in the right pane, and select New > DWORD (32-bit) Value.
	4. Name the DWORD value the same as the username (``dummy``) and set its value to 0.
- This will hide the user from the login screen.
- Now on the shared folder change the sharing options to just ``dummy`` accounts. Make sure the security tabs have Authenticated user.
---
- Base on [CIA triad](https://www.techtarget.com/whatis/definition/Confidentiality-integrity-and-availability-CIA), this will make sure your data dont fall into wrong hands, but keep in mind that nothing will be safed when connect to the Internet. Even the smart bulb can be use for DDOS attack, LOL. 