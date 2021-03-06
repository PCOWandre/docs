# Sync Settings

## Sync / Import

#### Username Field

Which field will be used as username for new users. Usually `sAMAccountName` or `uid`.
Leave empty to let the user pick their own Rocket.Chat username.
You can use template tags too, for example:

	#{givenName}.#{sn}

#### Unique Identifier Field

Which field will be used to link the LDAP user and the Rocket.Chat user. You can inform multiple values separated by comma to try to get the value from LDAP record.

#### Default Domain

If provided the Default Domain will be used to create an unique email for users where email was not imported from LDAP. The email will be mounted as `username@default_domain` or `unique_id@default_domain`.

#### Merge Existing Users

**Caution!** When importing a user from LDAP and an user with same username already exists the LDAP info and password will be set into the existing user. This will let LDAP users take over password accounts with the same username.

#### Sync User Data

Keep user data in sync with server on **login** or on **background sync** (eg: name, email and custom fields).

#### User Data Field Map

Configure how user account fields (like email) are populated from a record in LDAP (once found).

As an example, `{"cn":"name", "mail":"email"}` will choose a person's human readable name from the cn attribute, and their email from the mail attribute.

Additionally it is possible to use variables, for example, the following object will use a combination of the user's first name and last name for the rocket chat.

	{
		"#{givenName} #{sn}": "name",
		"mail": "email"
	}

#### Sync LDAP Groups

Enable this setting to activate role mapping from user groups on the community edition of Rocket.Chat.

#### Auto Remove User Roles

Enable this setting to automatically remove roles from LDAP users that don't have the corresponding group. This will only remove roles automatically that are set under the user data group map below.

#### User Group Filter

The LDAP search filter used to check if a user is in a group.

#### LDAP Group BaseDN

The LDAP BaseDN used to lookup if users are in a group.

#### User Data Group Map

The mapping of LDAP groups to Rocket.Chat roles, in JSON format.

As an example, the following object will map the rocket-admin LDAP group to Rocket.Chat's "admin" role and the "tech-support" group to the "support" role.

	{
		"rocket-admin":"admin",
		"tech-support":"support"
	}


#### Auto Sync LDAP Groups to Channels

Enable this feature to automatically add users to a channel based on their LDAP group.


#### Channel Admin

When the above setting cause a channel to be created automatically during an user sync, this setting will determine what user will become the admin of the channel.


#### LDAP Group Channel Map

The map of LDAP groups to Rocket.Chat channels, in JSON format.
As an example, the following object will add any user in the LDAP group "employee" to the general channel on Rocket.Chat.


	{
		"employee":"general"
	}

#### Auto Remove Users from Channels

Enabling this will remove any users in a channel that do not have the corresponding LDAP group! This will happen in every login and background sync, so removing a group on LDAP will not instantly remove access to channels on Rocket.Chat.


#### Sync User Avatar

Toggle avatar syncing on or off.

#### User avatar field

What LDAP field will be used as **avatar** for users. Leave empty to use `thumbnailPhoto` first and `jpegPhoto` as fallback.

#### Background Sync

Enable periodic background sync

#### Background Sync interval

The interval between synchronizations, using the [Cron Text](https://bunkat.github.io/later/parsers.html#text) format,

#### Background Sync Import New Users

Will import all users (based on your filter criteria) that exists in LDAP and does not exists in Rocket.Chat

#### Background Sync Update Existing Users

Will sync the avatar, fields, username, etc (based on your configuration) of all users already imported from LDAP on every **Sync Interval**

#### Execute Synchronization Now

Will execute the Background Sync now rather than wait the Sync Interval even if Background Sync is False.
This Action is asynchronous, please see the logs for more information about the process.

## Advanced Sync

#### Sync User Active State

Determine if users should be enabled or disabled on Rocket.Chat based on the LDAP status. The 'pwdAccountLockedTime' attribute will be used to determine if the user is disabled. This setting is not yet compatible with all LDAP Servers, so if you don't use the 'pwdAccountLockedTime' attribute, you may want to disable it completely.

## Role Mapping Settings (Enterprise only)

#### Role mapping from LDAP to Rocket.Chat.

Use this setting to map LDAP groups into Rocket.Chat roles.

You need to use an object format where the object key must be the LDAP group and the object value must be an array of RC roles. Example:

	{
		'ldapRole': [
			'rcRole',
			'anotherRCRole'
		]
	}

#### Validate mapping for each login

If the validation should occur for each login.

Be careful with this setting because it will overwrite the user roles in each login, otherwise this will be validated only at the moment of user creation.

#### Default role to user

The default RC role to be applied to user if the user has some LDAP group that is not mapped.

#### LDAP query to get user groups

LDAP query to get the LDAP groups that the user is part of.

