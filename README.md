# Containers for performing utility functions

Sometimes we need to run a service, but installing it on our host systems means a lot of baggage/dependencies and they
don't always work as intended, especially on Mac and Windows.

Containers to the rescue! Package up a specific function into its own environment and run it on demand, without messing
up your own host!

In this release we have an SMB Server (Impacket), webdav, NGINX server and even an HTTP request receiver.

I can't take credit for this, I found an amazing blog over here: https://blog.ropnop.com/docker-for-pentesters/

# Usage

Make sure you have Docker installed of course.

Go into the specific container folder and run docker build. E.g.

`docker build -t {your-docker-account}/impacket:latest .`

Then chuck it up to your Docker Hub account:

`docker push {your-docker-account}/impacket:latest`

Then run them! To streamline your flow, set shell aliases. This is how it looks in my ~/.zshrc file (dont forget to
restart your shell or better, run e.g. `source ~/.zshrc`

`smbservehere() {
    local sharename
    [[ -z $1 ]] && sharename="SHARE" || sharename=$1
    docker run --rm -it -p 445:445 -v "${PWD}:/tmp/serve" {your-docker-account}/impacket smbserver.py -smb2support $sharename /tmp/serve
}

alias nginxhere='docker run --rm -it -p 80:80 -p 443:443 -v "${PWD}:/srv/data" {your-docker-account}/nginxhere:v1'
alias webdavhere='docker run --rm -it -p 80:80 -v "${PWD}:/srv/data/share" {your-docker-account}/webdavhere:v1'
alias metasploitports='docker run --rm -it -v "${HOME}/.msf4:/home/msf/.msf4" -p 6666-6699:6666-6699 metasploitframework/metasploit-framework ./msf
alias reqdump='docker run --rm -it -p 80:3000 {your-docker-account}/reqdump:v1'
`

# Bonus Points

This is of course local to your machine. If you want to expose these to the interwebz, use the awesome ngrok tool.

For HTTP/S use `ngrok http 80` / `ngrok http 443`

For TCP use e.g. `ngrok tcp 445`

Google and ngrok's website are your friend :-)

Have fun!
