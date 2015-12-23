# put.io automator #

This small python application monitors a folder for torrent files. When one is saved there,
it uploads the torrent file to put.io, which starts a transfer. At a configurable interval, it
checks your put.io files collection for new files and downloads them to a downloads folder.

Configure Sickrage to use a Torrent black hole folder. Configure this application to
monitor that folder and download to the same folder used for post-processing in Sickrage.

# Setup #

Create a virtualenv (recommended, assuming you're using virtualenvwrapper):

    mkvirtualenv putio

Install the package requirements (while being in the virtualenv):

    pip install -r requirements.txt

# Configure #

Copy the distributed .ini file:

    cp application.ini.dist application.ini

Edit the config settings in the file. If you do not specify a log_filename, the application will log to the console.

To get a put.io token [register your application](https://put.io/v2/oauth2/register) in put.io, and copy the *Oauth token*.

## Run ##

Run the application:

    python application.py

The application checks if there are files available for download on put.io first. If so, the application downloads them.

If the application has encountered a file before, it warns you and moves on. Downloads are recorded in a sqlite3 database: application.db (configurable).

Once it has done the initial downloads, it checks the torrents folder to see if there are any it should upload, storing a record so it doesn't try to do it again.

It then starts the file watch notification loop, and checks for new files at an interval configured with *check_interval*.

A supervisor conf file is available in etc/supervisor.conf