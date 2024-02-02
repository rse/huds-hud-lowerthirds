
LowerThirds HUD
===============

This is a small LowerThirds HUD for the [**Head-Up-Display Server
(HUDS)**](http://npmjs.com/huds). It allows you to edit 8
text fields and render those text fields as simple texts.

Use the following steps to run the HUD:

1.  Install HUDS and this HUD either as global NPM packages with...

    ```sh
    $ npm install -g huds huds-hud-lowerthirds
    ```

    ...or just clone the Git repositories (in case you want to fiddle around
    with the source code) with:

    ```sh
    $ git clone https://github.com/rse/huds
    $ (cd huds && npm install)
    $ git clone https://github.com/rse/huds-hud-lowerthirds
    $ (cd huds-hud-lowerthirds && npm install)
    ```

2.  Start [HUDS](http://npmjs.com/huds) with either...

    ```sh
    $ huds -a 127.0.0.1 -p 9999 -U lowerthirds -P lowerthirds -d lowerthirds:@huds-hud-lowerthirds
    ```

    ...or (in case of cloned repositories):

    ```sh
    $ node huds/src/huds-server-cli.js -a 127.0.0.1 -p 9999 -U lowerthirds -P lowerthirds \
      -d lowerthirds:huds-hud-lowerthirds,huds-hud-lowerthirds/lowerthirds.yaml
    ```

    HUDS will display its logging output to the terminal.

2.  Start [OBS Studio](https://obsproject.com/)
    and add a [Browser Source](https://obsproject.com/wiki/Sources-Guide#browsersource)
    to any scene. As its control URL use:

    ```
    http://lowerthirds:lowerthirds@127.0.0.1:9999/lowerthirds/#mode=control
    ```

    As its render URL use:

    ```
    http://lowerthirds:lowerthirds@127.0.0.1:9999/lowerthirds/#mode=render,fields=<id>:<x>:<y>[+<id>:<x>:<y>[+...]]
    ```

