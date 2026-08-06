1. Download the Setup Script File Natively

Instead of piping directly into bash, download the installer locally first. This avoids environment flag issues and allows you to inspect the script. _(Note: Replace `22.x` with `20.x` or `24.x` depending on your required major version):_ [[1](https://github.com/nodesource/distributions/blob/master/DEV_README.md)]

bash

```
curl -fsSL https://deb.nodesource.com/setup_22.x -o nodesource_setup.sh
```

Use code with caution.

2. Execute the Script with Elevated Privileges

Run the local file directly using your standard `sudo` command without the `-E` flag: [[1](https://www.rosehosting.com/blog/install-node-js-on-ubuntu-26-04/?srsltid=AfmBOoplTwWdOSM1wqozJzqgPaO12jvoeuo7LTc3xgUFXdnTZ2Jr_00t)]

bash

```
sudo bash nodesource_setup.sh
```

Use code with caution.

3. Install Node.js

Once the installer configures your package lists successfully, deploy Node.js via your native package manager: [[1](https://linuxize.com/post/how-to-install-node-js-on-ubuntu-26-04/), [2](https://github.com/nodesource/distributions/blob/master/scripts/deb/setup_22.x)]

bash

```
sudo apt-get install -y nodejs
```

Use code with caution.

4. Verify Your Work

Confirm both binaries are successfully resolved by checking their compiled system versions: [[1](https://linuxize.com/post/how-to-install-node-js-on-ubuntu-26-04/)]

bash

```
node -v
npm -v
```

Use code with caution.