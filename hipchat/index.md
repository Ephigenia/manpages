# Adding New Users to HipChat

You’re received an invitation email to your @foobugs account. After that follow these instructions:

1. 	Edit User Profile

	After you’ve chosen your password and stuff please update your `@metion` tag and avatar image on the "account" page:
	https://foobugs.hipchat.com/account

2.	OSX: Use Messages App with HipChat

	Hipchat provides apps for iOS, Android, Windows Phones and pads and also a native client. If you don’t want to have an additional software you can use the native "Messages" or "Nachrichten" app with HipChat over the Jabber protocol.

	# OSX

	- Open Message app and Go to Preferences and click on the "+" Icon
	- Use the credentials from here: https://foobugs.hipchat.com/account/xmpp

	There’s a more detailed version: http://help.hipchat.com/knowledgebase/articles/64437-how-to-connect-using-messages-app-ichat

	# PC: Miranda

	http://help.hipchat.com/knowledgebase/articles/64438-how-to-connect-using-miranda

	# PC: Trillian

	http://help.hipchat.com/knowledgebase/articles/64440-how-to-connect-using-trillian

3. 	Joining Rooms

	On the xmpp info page (https://foobugs.hipchat.com/account/xmpp) there’s
	also a list rooms you can join. Use the XMPP/Jabber name column values to
	create shortcuts in the Messages App:

	OSX: 

	- Switch to Messages App
	- Main Menu "Ablage" - "Chatraum betreten" or Press `CMD+R`. 
	- There you can add room favorites by pasting the room’s name into "Raumname" and then on the "+" on the lower bottom of the window.

	After you’ve added all Rooms click on any of them to join.

4.	Say hello in every room!


# Adding GitHub Hook

1.	Create an API Key if you don’t have one allready on HipChat:
	https://foobugs.hipchat.com/admin/api

1.	Go to github project’s admin page - Hooks - Hipchat.
	Add the API Key from the HipChat API page.

2.  Get the room id from here: https://foobugs.hipchat.com/rooms/ids