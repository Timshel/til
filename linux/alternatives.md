# Setup multiple NodeJs using alternatives

Put the multiple version in `/usr/lib/nodejs/`.

Add them to alternative (higher priority is better).

    sudo update-alternatives --install /usr/lib/nodejs/current nodejs /usr/lib/nodejs/node-v0.12.12-linux-x64 200
    sudo update-alternatives --install /usr/lib/nodejs/current nodejs /usr/lib/nodejs/node-v4.4.0-linux-x64 100

Then create the symlink

    sudo ln -s /usr/lib/nodejs/current/bin/node /usr/bin/nodejs
    sudo ln -s /usr/lib/nodejs/current/bin/npm /usr/bin/npm
    sudo ln -s /usr/bin/nodejs /usr/bin/node

To toggle between each version :

    sudo update-alternatives --config nodejs