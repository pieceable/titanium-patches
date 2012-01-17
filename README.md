
# Titanium Patches

Patches to make Titanium's `builder.py` produce complete, self-contained app builds for the iOS Simulator.  This makes it possible to upload compiled Titanium apps to the [Pieceable Viewer](https://www.pieceable.com) web simulator, or to distribute them as standalone Mac apps with Landon Fuller's [simlaunch](https://github.com/landonf/simlaunch).

When building for the simulator, `builder.py` normally assumes you'll be running the app on the same machine as your project source.  This lets it take shortcuts like not compiling your `app.js`, and symlink'ing to image resources.

These patches add a `buildfull` option to the `builder.py` script that doesn't take any shortcuts.

## Which patch to use?

Run the following to figure out which version of the Titanium SDK you have installed:

```
ls ~/Library/Application\ Support/Titanium/mobilesdk/osx
```

We've included patches for __1.8.0.1__ _(builder.py-1.8.0.1-patch)_ and __1.7.5__ _(builder.py-1.7.5-patch)_.  If we don't have a patch for your specific version, try whichever is closest.  Or, email [support@pieceable.com](mailto:support@pieceable.com) and we'll help.

## Applying the patch

_Be sure to replace the version number with the SDK version you have._

```
cd ~/Library/Application\ Support/Titanium/mobilesdk/osx/1.8.0.1/iphone
curl https://raw.github.com/pieceable/titanium-patches/master/builder.py-1.8.0.1-patch | patch -b -p0 builder.py
```

The `-b` option to patch will create a backup for you at `builder.py.orig`.

## Building your app

Use the new __buildfull__ option:

```
~/Library/Application\ Support/Titanium/mobilesdk/osx/1.8.0.1/iphone/builder.py buildfull \ 
  5.0 "~/path/to/YourProject" com.yourcompany.yourappid YourAppName
```

At this point, your fully compiled app is now ready at:

```
~/path/to/YourProject/build/iphone/build/Debug-iphonesimulator/YourAppName.app
```

## Uploading to the Pieceable Viewer web simulator

_Replace the path with the path to your compiled .app, and fill in your email address._

```
ditto -cj ~/path/to/YourProject/build/iphone/build/Debug-iphonesimulator/YourAppName.app - | \
  curl -F "email=you@somewhere.com" -F "file=@-" http://www.pieceable.com/view/publish
```

More details on this available here: https://www.pieceable.com/start
