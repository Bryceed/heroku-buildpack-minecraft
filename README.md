# Heroku Minecraft Buildpack

This is a [Heroku Buildpack](https://devcenter.heroku.com/articles/buildpacks)
for running a Minecraft server in a [dyno](https://devcenter.heroku.com/articles/dynos). This fork removes the Amazon S3 requirement for those who don't need persistent storage. Because of this, **Heroku will automatically revert all files generated at runtime**. This means that **after all server connections are closed, it will be as if all files had just been uploaded to Heroku.** **Make any permanent changes to the server _locally_**.
[![Deploy to Heroku](https://www.herokucdn.com/deploy/button.png)](https://heroku.com/deploy)

## Usage

Create a [free ngrok account](https://ngrok.com/) and copy your Auth token. Then create a new Git project with a `eula.txt` file:

```sh-session
$ echo 'eula=true' > eula.txt
$ git init
$ git add eula.txt
$ git commit -m "first commit"
```

Then, install the [Heroku toolbelt](https://toolbelt.heroku.com/).
Create a Heroku app, set your ngrok token, and push:

```sh-session
$ heroku create --buildpack https://github.com/Gerzer/heroku-buildpack-minecraft
$ heroku config:set NGROK_API_TOKEN="xxxxx"
$ git push heroku master
```

Finally, open the app:

```sh-session
$ heroku open
```

This will display the ngrok logs, which will contain the name of the server
(really it's a proxy, but whatever):

```
[03/11/15 02:06:21] [INFO] [client] Tunnel established at tcp://ngrok.com:45010
```

Copy the `ngrok.com:45010` part, and paste it into your local Minecraft app
as the server name.

## Customizing

### ngrok

You can customize ngrok by setting the `NGROK_OPTS` config variable. For example:

```
$ heroku config:set NGROK_OPTS="-subdomain=my-subdomain"
```

### Minecraft

You can choose the Minecraft version by setting the MINECRAFT_VERSION like so:

```
$ heroku config:set MINECRAFT_VERSION="1.8.3"
```

You can also configure the server properties by creating a `server.properties`
file in your project and adding it to Git. This is how you would set things like
Creative mode and Hardcore difficulty. The various options available are
described on the [Minecraft Wiki](http://minecraft.gamepedia.com/Server.properties).

You can add files such as `banned-players.json`, `banned-ips.json`, `ops.json`,
`whitelist.json` to your Git repository and the Minecraft server will pick them up.
