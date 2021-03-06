```
    ____           ____
   / __ \____     / __ \___  _________  ____
  / / / / __ \   / /_/ / _ \/ ___/ __ \/ __ \
 / /_/ / /_/ /  / _, _/  __/ /__/ /_/ / / / /
/_____/\____/  /_/ |_|\___/\___/\____/_/ /_/
                    made with <3 by @jesgvn
```

# Do Recon

Automated recon script that spins up a DigitalOcean VPS and sets the `user-data` option to a recon script for a specific domain(s). Initializing script (`vps-init.sh`) installs popular recon tools and runs a basic recon assessment with subfinder, Amass and nuclei. Supports notification on completion by Telegram. Default is to not run recon commands, you can enable by passing `-r` flag as shown below.

### Usage:

```sh
TELEGRAM_BOT_ID=123 TELEGRAM_CHAT_ID=asdf ./dorecon -r domain1.com domain2.com
```

### Go tools installed:

* [Amass](https://github.com/OWASP/Amass)
* [subfinder](https://github.com/projectdiscovery/subfinder)
* [httpx](https://github.com/projectdiscovery/httpx)
* [nuclei](https://github.com/projectdiscovery/nuclei)
* [httprobe](https://github.com/tomnomnom/httprobe)
* [ffuf](https://github.com/ffuf/ffuf)
* [hakrawler](https://github.com/hakluke/hakrawler)

### Python tools installed (with flag)

* [dirsearch](https://github.com/maurosoria/dirsearch)

### Requirements:

* [DigitalOcean](https://m.do.co/c/b3ccbe8742ef) account (referral link)
* Create and attach an [SSH key](https://www.digitalocean.com/docs/droplets/how-to/add-ssh-keys/) to your account (in order to connect to VPS later)
* Create an [API token](https://www.digitalocean.com/docs/apis-clis/doctl/how-to/install/#step-2-create-an-api-token)
* Install and configure [doctl](https://github.com/digitalocean/doctl) using the created API token
* Set the correct SSH key in the doctl [configuration file](https://github.com/digitalocean/doctl#configuring-default-values), this will allow you to SSH to the VPS
* Optional: Telegram message sent upon recon completion

### Configuration:

**Flags:**

| Flag | Description | Example |
|------|-------------|---------|
| -r   | Run recon commands | ./dorecon -r domain1.com |
| -p   | Install python tools | ./dorecon -p |

**Environment variables:**

| Name | Description | Example |
|------|-------------|---------|
| REGION | Region in which to create droplet (optional, default = `sfo2`) `doctl compute region list` to view available) | REGION=nyc3 |
| SIZE | Size of the droplet (optional, default = `s-1vcpu-2gb`) `doctl compute size list` to view available options) | SIZE=s-3vcpu-1gb |
| TELEGRAM_BOT_ID | Telegram bot id (optional) | TELEGRAM_BOT_ID=123123:asdfasdfasdf |
| TELEGRAM_CHAT_ID | Telegram chat id (optional) | TELEGRAM_CHAT_ID=123123 |

### Initial recon:

The initial recon consists of:

* Running subfinder and Amass on target domain
* Piping found domains to httpx and later to nuclei

### Reports:

Reports are written to `/root/recon/reports/$TIMESTAMP/$DOMAIN`.

You can view the status of the script with `tail -f /var/log/cloud-init-output.log`.

### After:

Since the rest of the tools are installed on the VPS using them is as simple as just SSH'ing to the droplet.

