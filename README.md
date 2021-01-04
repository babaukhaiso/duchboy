# DuchBoy
Build AriaNG on Heroku, and upload to cloud drive when the file download completed.<br>
在 Heroku 上搭建 AriaNG ，并在文件下载完成后上传至网盘。

Using Rclone with 21vianet mod and Aria2, even UNRAR online flexibly? Try this [Heroku Rclone 21vianet](https://github.com/xinxin8816/heroku-rclone-21vianet)<br>
想更灵活的使用 Aria2、Rclone，甚至是 RAR 在线解压？试试这个 [Heroku Rclone 世纪互联版](https://github.com/xinxin8816/heroku-rclone-21vianet)

## Abuse Warning 

**Heroku are actively banning this APP now.**<br>

**Your account will be SUSPENDED in highly possible.**<br>

**This APP is no longer updated.**<br>

1. Rclone with 21vianet patch and Gclone mod. 
2. Support mount double cloud drive.
3. Improve performance of the built-in Aria2c and Rclone. 
4. Unpack(Beta) ZIP/RAR/7Z with password or sub-volume. 
5. Fix some little issues in fork source. 

## Deploy by Docker (Recommend)

FAQ: [Do I have to use Docker?](#do-i-have-to-use-docker)

### Requirement 

* [Docker](https://www.docker.com/)
* [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli)
* [git](https://git-scm.com/)
* Ability to use terminal

### Steps

1. Run `heroku login` to login, then `heroku container:login` too.
2. Clone this repository and enter it. (PS: Please run `git config --global core.autocrlf false` before `git clone` if you are using Windows.)
3. Run `heroku apps:create APP_NAME` to create it.
4. Run `heroku config:set -a APP_NAME ARIA2C_SECRET=ARIA2_SECRET` and `heroku config:set -a APP_NAME HEROKU_APP_NAME=APP_NAME`.
5. Run `heroku container:push web -a APP_NAME` and `heroku container:release web -a APP_NAME`.
6. Run `heroku open -a APP_NAME` and it will open your browser to deployed instance. 

## Deploy by One-Click

**Pay attention please: deploy by One-Click uses the Heroku built-in environment, that means your account might banned immediately.**

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

## Optional: Connect Cloud Drive 连接网盘

1. Setup Rclone by following [Rclone Docs](https://rclone.org/docs/), Chinese users can setup with 21vianet patch to connect OneDrive by 21vianet.<br> 
You can find your config from there:

```
Windows: %userprofile%\.config\rclone\rclone.conf
Linux: $HOME/.config/rclone/rclone.conf
```
Optional: Using service account setup with [Gclone](https://github.com/donwa/gclone) to break Google Drive 750GB limit, or easier connect to folder or Team Drive by destination ID. First, open your rclone config and edit `service_account_file_path = /app/accounts/` as the SA json paths.

* Deploy by Docker: Create a new folder in your local repository root path after git clone, such as `/accounts/`, copy your SA json in it.<br>
* Deploy by One-Click: Fork this repository, and create a new folder in your forked repository, such as `/accounts/`, upload your SA json in it. 

2. Your `rclone.conf` file, it should look like this:

```conf
[DRIVENAME A]
type = WHATEVER
client_id = WHATEVER
client_secret = WHATEVER
scope = WHATEVER
china_version = WHATEVER
token = WHATEVER

[DRIVENAME B]
type = WHATEVER
client_id = WHATEVER
client_secret = WHATEVER
scope = WHATEVER
china_version = WHATEVER
token = WHATEVER

others entries...
```

3. Set Rclone Config: Find the drive you want to use, and copy its `[DRIVENAME A] ...` to  `... token = ...` section, and replace all linebreaks with `\n`, and copy this TEXT.

* Deploy by Docker: Run `heroku config:set -a APP_NAME RCLONE_CONFIG=THAT_TEXT`.<br>
* Deploy by One-Click: Set `THAT_TEXT` in the `RCLONE_CONFIG` blank.

4. Set Rclone Upload Destination: `[DRIVENAME A]:[REMOVE PATH A]` means upload to `[REMOVE PATH A]` in `[DRIVENAME A]`. `[DRIVENAME A]` is the drive friendly name in rclone config, `[REMOVE PATH A]` is a remote path. 

* Deploying by Docker: Run `heroku config:set -a APP_NAME RCLONE_DESTINATION=[DRIVENAME A]:[REMOVE PATH A]`.<br>
* Deploying by One-Click: Set `[DRIVENAME A]:[REMOVE PATH A]` in the `RCLONE_DESTINATION` blank.

5. If you mount a second cloud drive, Set `RCLONE_DESTINATION_2` same as step 4.