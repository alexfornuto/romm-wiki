## IGDB

To access the IGDB API you'll need a Twitch account and a valid phone number for 2FA verification. Up-to-date instructions are available in the [IGDB API documentation](https://api-docs.igdb.com/#account-creation). When registering your application in the Twitch Developer Portal, fill out the form like so:

* Name: Something **unique or random** like `correct-horse-battery-staple` or `KVV8NDXMSRFJ2MRNPNRSL7GQT`
* OAuth Redirect URLs: `localhost`
* Category: `Application Integration`
* Client Type: Confidential

> [!IMPORTANT]  
> **The name you pick has to be unique! Picking an existing name will fail silently, with no error messages.**

Note the client ID and secret that appear on screen, and use them to set `IGDB_CLIENT_ID` and `IGDB_CLIENT_SECRET` in your environment variables.

## Mobygames

To access the Mobygames API, [create a MobyGames account](https://www.mobygames.com/user/register/) and then visit your profile page. Click the 'API' link under your user name to sign up for an API key. Copy the key shown and use it to set `MOBYGAMES_API_KEY`.