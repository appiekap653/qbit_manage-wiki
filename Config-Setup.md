
# Overview

The script utilizes a YAML config file to load information to connect to the various APIs you can connect with.

By default, the script looks at /config/config.yml for the Configuration File unless otherwise specified.

A template Configuration File can be found in the repo [config/config.yml.sample](https://github.com/StuffAnThings/qbit_manage/blob/master/config/config.yml.sample).

**WARNING** <br>
> As this software is constantly evolving and this wiki might not be up to date the sample shown here might not might not be current. Please refer to the repo for the most current version.

# Config File

## Sample

```yaml
# This is an example configuration file that documents all the options.
# It will need to be modified for your specific use case.
# Please refer to the link below for more details on how to set up the configuration file
# https://github.com/StuffAnThings/qbit_manage/wiki/Config-Setup

commands:
  # The commands defined below will IGNORE any commands used in command line and docker env variables.
  dry_run: True
  cross_seed: False
  recheck: False
  cat_update: False
  tag_update: False
  rem_unregistered: False
  tag_tracker_error: False
  rem_orphaned: False
  tag_nohardlinks: False
  skip_cleanup: False

qbt:
  # qBittorrent parameters
  host: "localhost:8080"
  user: "username"
  pass: "password"

settings:
  force_auto_tmm: False  # Will force qBittorrent to enable Automatic Torrent Management for each torrent.
  tracker_error_tag: issue  # Will set the tag of any torrents that do not have a working tracker.
  ignoreTags_OnUpdate: # When running tag-update function, it will update torrent tags for a given torrent even if the torrent has at least one or more of the tags defined here. Otherwise torrents will not be tagged if tags exist.
    - noHL
    - issue
    - cross-seed
directory:
  # Do not remove these
  # Cross-seed var: </your/path/here/>  # Output directory of cross-seed
  # root_dir var: </your/path/here/>  # Root downloads directory used to check for orphaned files, noHL, and RecycleBin.
  # <OPTIONAL> remote_dir var: </your/path/here/>  # Path of docker host mapping of root_dir.
  # Must be set if you're running qbit_manage locally and qBittorrent/cross_seed is in a docker
  # <OPTIONAL> recycle_bin var: </your/path/here/>   # Path of the RecycleBin folder. Default location is set to remote_dir/.RecycleBin
  # <OPTIONAL> torrents_dir var: </your/path/here/>  # Path of the your qbittorrent torrents directory. Required for `save_torrents` attribute in recyclebin

  cross_seed: "/your/path/here/"
  root_dir: "/data/torrents/"
  remote_dir: "/mnt/user/data/torrents/"
  recycle_bin: "/mnt/user/data/torrents/.RecycleBin"
  torrents_dir: "/qbittorrent/data/BT_backup"
  orphaned_dir: "/data/torrents/orphaned_data"

cat:
  # Category & Path Parameters
  # <Category Name> : <save_path>  # Path of your save directory.
  movies: "/data/torrents/Movies"
  tv: "/data/torrents/TV"

cat_change:
  # This moves all the torrents from one category to another category. This executes on --cat-update
  # WARNING: if the paths are different and Default Torrent Management Mode is set to automatic the files could be moved !!!
  # <Old Category Name> : <New Category>
  Radarr-HD.cross-seed: movies-hd
  Radarr-UHD.cross-seed: movies-uhd
  movies-hd.cross-seed: movies-hd
  movies-uhd.cross-seed: movies-uhd


tracker:
  # Mandatory
  # Tag Parameters
  # <Tracker URL Keyword>:    # <MANDATORY> This is the keyword in the tracker url
  # <MANDATORY> Set tag name. Can be a list of tags or a single tag
  #   tag: <Tag Name>
  # <OPTIONAL> Will set the torrent Maximum share ratio until torrent is stopped from seeding/uploading. -2 means the global limit should be used, -1 means no limit.
  #   max_ratio: 5.0
  # <OPTIONAL> Will set the torrent Maximum seeding time (min) until torrent is stopped from seeding. -2 means the global limit should be used, -1 means no limit.
  #   max_seeding_time: 129600
  # <OPTIONAL> Will ensure that noHL torrents from this tracker are not deleted by cleanup variable if torrent has not yet met the minimum seeding time (min).
  #   min_seeding_time: 2000
  # <OPTIONAL> Will limit the upload speed KiB/s (KiloBytes/second) (-1 means no limit)
  #   limit_upload_speed: 150
  # <OPTIONAL> Set this to the notifiarr react name. This is used to add indexer reactions to the notifications sent by Notifiarr
  #   notifiarr: <notifiarr indexer>
  animebytes.tv:
    tag: AnimeBytes
    notifiarr: animebytes
  avistaz:
    tag:
      - Avistaz
      - tag2
      - tag3
    max_ratio: 5.0
    max_seeding_time: 129600
    min_seeding_time: 30400
    limit_upload_speed: 150
    notifiarr: avistaz
  beyond-hd:
    tag: [Beyond-HD, tag2, tag3]
    notifiarr: beyondhd
  blutopia:
    tag: Blutopia
    notifiarr: blutopia
  cartoonchaos:
    tag: CartoonChaos
  digitalcore:
    tag: DigitalCore
    max_ratio: 5.0
    notifiarr: digitalcore
  gazellegames:
    tag: GGn
    limit_upload_speed: 150
  hdts:
    tag: HDTorrents
    max_seeding_time: 129600
  landof.tv:
    tag: BroadcasTheNet
    notifiarr: broadcasthenet
  myanonamouse:
    tag: MaM
  passthepopcorn:
    tag: PassThePopcorn
    notifiarr: passthepopcorn
  privatehd:
    tag: PrivateHD
    notifiarr:
  tleechreload:
    tag: TorrentLeech
    notifiarr: torrentleech
  torrentdb:
    tag: TorrentDB
    notifiarr: torrentdb
  torrentleech:
    tag: TorrentLeech
    notifiarr: torrentleech
  tv-vault:
    tag: TV-Vault
  # The "other" key is a special keyword and if defined will tag any other trackers that don't match the above trackers into this tag
  other:
    tag: other

nohardlinks:
  # Tag Movies/Series that are not hard linked
  # Mandatory to fill out directory parameter above to use this function (root_dir/remote_dir)
  # This variable should be set to your category name of your completed movies/completed series in qbit. Acceptable variable can be any category you would like to tag if there are no hardlinks found
  movies-completed:
    # <OPTIONAL> exclude_tags var: Will exclude torrents with any of the following tags when searching through the category.
    exclude_tags:
      - Beyond-HD
      - AnimeBytes
      - MaM
    # <OPTIONAL> cleanup var: WARNING!! Setting this as true Will remove and delete contents of any torrents that have a noHL tag and meets share limits
    cleanup: false
    # <OPTIONAL> max_ratio var: Will set the torrent Maximum share ratio until torrent is stopped from seeding/uploading
    max_ratio: 4.0
    # <OPTIONAL> max seeding time var: Will set the torrent Maximum seeding time (min) until torrent is stopped from seeding
    max_seeding_time: 86400
    # <OPTIONAL> Limit Upload Speed var: Will limit the upload speed KiB/s (KiloBytes/second) (`-1` : No Limit)
    limit_upload_speed:
    # <OPTIONAL> min seeding time var: Will prevent torrent deletion by cleanup variable if torrent has not yet minimum seeding time (min).
    # Delete this key from a category's config to use the tracker's configured min_seeding_time. Will default to 0 if not specified for the category or tracker.
    min_seeding_time: 43200
    # <OPTIONAL> resume_torrent_after_untagging_noHL var: If a torrent was previously tagged as NoHL and now has hardlinks, this variable will resume your torrent after changing share limits
    resume_torrent_after_untagging_noHL: false
  # Can have additional categories set with separate ratio/seeding times defined.
  series-completed:
    # <OPTIONAL> exclude_tags var: Will exclude torrents with any of the following tags when searching through the category.
    exclude_tags:
      - Beyond-HD
      - BroadcasTheNet
    # <OPTIONAL> cleanup var: WARNING!! Setting this as true Will remove and delete contents of any torrents that have a noHL tag and meets share limits
    cleanup: false
    # <OPTIONAL> max_ratio var: Will set the torrent Maximum share ratio until torrent is stopped from seeding/uploading
    max_ratio: 4.0
    # <OPTIONAL> max seeding time var: Will set the torrent Maximum seeding time (min) until torrent is stopped from seeding
    max_seeding_time: 86400
    # <OPTIONAL> Limit Upload Speed var: Will limit the upload speed KiB/s (KiloBytes/second) (`-1` : No Limit)
    limit_upload_speed:
    # <OPTIONAL> min seeding time var: Will prevent torrent deletion by cleanup variable if torrent has not yet minimum seeding time (min).
    # min_seeding_time: # Not specified for this category; tracker's value will be used. Will default to 0 if not specified for the category or tracker.
    # <OPTIONAL> resume_torrent_after_untagging_noHL var: If a torrent was previously tagged as NoHL and now has hardlinks, this variable will resume your torrent after changing share limits
    resume_torrent_after_untagging_noHL: false

recyclebin:
  # Recycle Bin method of deletion will move files into the recycle bin (Located in /root_dir/.RecycleBin) instead of directly deleting them in qbit
  # By default the Recycle Bin will be emptied on every run of the qbit_manage script if empty_after_x_days is defined.
  enabled: true
  # <OPTIONAL> empty_after_x_days var:
  # Will automatically remove all files and folders in recycle bin after x days. (Checks every script run)
  # If this variable is not defined it, the RecycleBin will never be emptied.
  # WARNING: Setting this variable to 0 will delete all files immediately upon script run!
  empty_after_x_days: 60
  # <OPTIONAL> save_torrents var:
  # If this option is set to true you MUST fill out the torrents_dir in the directory attribute.
  # This will save a copy of your .torrent and .fastresume file in the recycle bin before deleting it from qbittorrent
  save_torrents: true
  # <OPTIONAL> split_by_category var:
  # This will split the recycle bin folder by the save path defined in the `cat` attribute
  # and add the base folder name of the recycle bin that was defined in the `recycle_bin` sub-attribute under directory.
  split_by_category: false

orphaned:
  # Orphaned files are those in the root_dir download directory that are not referenced by any active torrents.
  # Will automatically remove all files and folders in orphaned data after x days. (Checks every script run)
  # If this variable is not defined it, the orphaned data will never be emptied.
  # WARNING: Setting this variable to 0 will delete all files immediately upon script run!
  empty_after_x_days: 60
  # File patterns that will not be considered orphaned files. Handy for generated files that aren't part of the torrent but belong with the torrent's files
  exclude_patterns:
    - "**/.DS_Store"
    - "**/Thumbs.db"
    - "**/@eaDir"
    - "/data/torrents/temp/**"
    - "**/*.!qB"
    - '**/_unpackerred'

apprise:
  # Apprise integration with webhooks
  # Leave Empty/Blank to disable
  # Mandatory to fill out the url of your apprise API endpoint
  api_url: http://apprise-api:8000
  # Mandatory to fill out the notification url/urls based on the notification services provided by apprise. https://github.com/caronc/apprise/wiki
  notify_url:

notifiarr:
  # Notifiarr integration with webhooks
  # Leave Empty/Blank to disable
  # Mandatory to fill out API Key
  apikey: ####################################
  # <OPTIONAL> Set to a unique value (could be your username on notifiarr for example)
  instance:

webhooks:
  # Webhook notifications:
  # Possible values:
  # Set value to notifiarr if using notifiarr integration
  # Set value to apprise if using apprise integration
  # Set value to a valid webhook URL
  # Set value to nothing (leave Empty/Blank) to disable
  error: https://mywebhookurl.com/qbt_manage
  run_start: notifiarr
  run_end: apprise
  function:
    cross_seed: https://mywebhookurl.com/qbt_manage
    recheck: notifiarr
    cat_update: apprise
    tag_update: notifiarr
    rem_unregistered: notifiarr
    tag_tracker_error: notifiarr
    rem_orphaned: notifiarr
    tag_nohardlinks: notifiarr
    cleanup_dirs: notifiarr

bhd:
  # BHD Integration used for checking unregistered torrents
  apikey:
```

# **List of variables**<br>

## **commands:**

---
This section will ignore any commands that are defined via environment variable or command line and use the ones defined in this yaml file instead. Useful if you want to run qbm with multiple configurations files that execute different commands for each qbt instance.

| Variable  | Definition                                | Required           |
| :-------- | :---------------------------------------- | :----------------- |
| `command` | The command that you want qbm to execute. | <center>❌</center> |

## **qbt:**

---
This section defines your qBittorrent instance.

| Variable | Definition                          | Required           |
| :------- | :---------------------------------- | :----------------- |
| `host`   | IP address of your qB installation. | <center>✅</center> |
| `user`   | The user name of your qB's webUI.   | <center>❌</center> |
| `pass`   | Thee password of your qB's webUI.   | <center>❌</center> |

<br>

## **settings:**

---
This section defines any settings defined in the configuration.

| Variable              | Definition                                                                                                                                          | Required           |
| :-------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------- | :----------------- |
| `force_auto_tmm`      | Will force qBittorrent to enable Automatic Torrent Management for each torrent.                                                                     | <center>❌</center> |
| `tracker_error_tag`   | Define the tag of any torrents that do not have a working tracker. (Used in `--rem-unregistered` and `--tag-tracker-error`)                         | <center>❌</center> |
| `ignoreTags_OnUpdate` | When running `--tag-update` function, it will update torrent tags for a given torrent even if the torrent has one or more of the tags defined here. | <center>❌</center> |

## **directory:**

---
This section defines the directories that qbit_manage will be looking into for various parts of the script.

| Variable       | Definition                                                                                                                                                                                                                                                                                                                                                                          | Required                                                      |
| :------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------ |
| `cross_seed`   | Output directory of cross-seed, originally the application [cross-seed](https://github.com/mmgoodnow/cross-seed) was incapable of injecting cross-seed torrent into qB, this was built to inject them for the application. This is no longer required if you're using injects with that software. However, you can find other uses for this as it is more of a watch directory now. | QBT_CROSS_SEED                                                |
| `root_dir`     | Root downloads directory used to check for orphaned files, noHL, and remove unregistered. This directory is where you place all your downloads. This will need to be how qB views the directory where it places the downloads. This is required if you're using qbit_managee and/or qBittorrent within a container.                                                                 | QBT_REM_ORPHANED / QBT_TAG_NOHARDLINKS / QBT_REM_UNREGISTERED |
| `remote_dir`   | Path of docker host mapping of root_dir, this must be set if you're running qbit_manage locally (not required if running qbit_manage in a container) and qBittorrent/cross_seed is in a docker. Essentially this is where your downloads are being kept on the host.                                                                                                                | <center>❌</center>                                            |
| `recycle_bin`  | Path of the RecycleBin folder. Default location is set to `remote_dir/.RecycleBin`.                                                                                                                                                                                                                                                                                                 | <center>❌</center>                                            |
| `torrents_dir` | Path of the your qbittorrent torrents directory. Required for `save_torrents` attribute in recyclebin `/qbittorrent/data/BT_backup`.                                                                                                                                                                                                                                                | <center>❌</center>                                            |
| `orphaned_dir` | Path of the Orphaned Directory folder. Default location is set to `remote_dir/orphaned_data`.                                                                                                                                                                                                                                                                                       | <center>❌</center>                                            |

## **cat:**

---
This section defines the categories that you are currently using and the path's that are associated with them.<br>
> **NOTE** ALL categories must be defined, if it is in your qBit, then it **MUST** be defined here, if not the script will throw errors.

| Configuration | Definition                | Required           |
| :------------ | :------------------------ | :----------------- |
| `key`         | Name of the category      | <center>✅</center> |
| `value`       | Save Path of the category | <center>✅</center> |

The syntax for all the categories are as follows

```yaml
category: <path>/<to>/category
```

## **cat_change:**

---
This moves all the torrents from one category to another category if the torrents are marked as complete.<br>
> **NOTE** **WARNING**: if the paths are different and Default Torrent Management Mode is set to automatic the files could be moved !!!

| Configuration | Definition                    | Required           |
| :------------ | :---------------------------- | :----------------- |
| `key`         | Name of the original category | <center>✅</center> |
| `value`       | Name of the new category      | <center>✅</center> |

The syntax for the categories are as follows

```yaml
old_category_name: new_category_name
```

## **tracker:**

---
This section defines the tags used based upon the tracker's URL.<br>
| Configuration | Definition          | Required           |
| :------------ | :------------------ | :----------------- |
| `key`         | Tracker URL Keyword | <center>✅</center> |

| Variable             | Definition                                                                                                                        | Default Values | Required           |
| :------------------- | :-------------------------------------------------------------------------------------------------------------------------------- | :------------- | :----------------- |
| `tag`                | The tracker tag or additional list of tags defined                                                                                | Tracker URL    | <center>✅</center> |
| `max_ratio`          | Will set the torrent Maximum share ratio until torrent is stopped from seeding/uploading. (`-2` : Global Limit , `-1` : No Limit) | None           | <center>❌</center> |
| `max_seeding_time`   | Will set the torrent Maximum seeding time (min) until torrent is stopped from seeding. (`-2` : Global Limit , `-1` : No Limit)    | None           | <center>❌</center> |
| `limit_upload_speed` | Will limit the upload speed KiB/s (KiloBytes/second) (`-1` : No Limit)                                                            | None           | <center>❌</center> |
| `notifiarr`          | Set this to the notifiarr react name. This is used to add indexer reactions to the notifications sent by Notifiarr                | None           | <center>❌</center> |

If either max_ratio or max_seeding_time is set to `-2` then the the global share limits will be used, `-1` then no share limits will be used.

If you are unsure what key word to use. Simply select a torrent within qB and down at the bottom you should see a tab that says `Trackers` within the list that is populated there are ea list of trackers that are associated with this torrent, select a key word from there and add it to the config file. Make sure this key word is unique enough that the script will not get confused with any other tracker.

## **nohardlinks:**

---
Hardlinking data allows you to have your data in both the torrent directory and your media direectory at the same time without using double the amount of data.

If you're needing information regarding hardlinks here are some excellent resources. <br>

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/AMcHsQJ7My0/0.jpg)](https://www.youtube.com/watch?v=AMcHsQJ7My0)

* [Trash-Guides: Hardlinks and Instant Moves (Atomic-Moves)](https://trash-guides.info/Hardlinks/Hardlinks-and-Instant-Moves/)
* [Wikipedia: Hardlinks](https://en.wikipedia.org/wiki/Hard_link)

Mandatory to fill out [directory parameter](#directory) above to use this function (root_dir/remote_dir)
Beyond this you'll need to use one of the [categories](#cat) above as the key, and should be pointed to the `completed-` directory of your torrents.

| Configuration | Definition                                                       | Required           |
| :------------ | :--------------------------------------------------------------- | :----------------- |
| `key`         | Category name of your completed movies/completed series in qbit. | <center>✅</center> |

| Variable             | Definition                                                                                                                                                                            | Default Values | Required           |
| :------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :------------- | :----------------- |
| `cleanup`            | True = Remove the non-hardlinked torrent data/contents once share limits have been met, False = Use (Automatic Torrent Management) to handle seeding limits                           | False          | <center>✅</center> |
| `max_ratio`          | Will set the torrent Maximum share ratio until torrent is stopped from seeding/uploading. (`-2` : Global Limit , `-1` : No Limit)                                                     | None           | <center>❌</center> |
| `max_seeding_time`   | Will set the torrent Maximum seeding time (min) until torrent is stopped from seeding. (`-2` : Global Limit , `-1` : No Limit) (The `max_seeding_time` will count from the moment the NoHL tag is set)   | None           | <center>❌</center> |
| `limit_upload_speed` | Will limit the upload speed KiB/s (KiloBytes/second) (`-1` : No Limit)                                                                                                                | None           | <center>❌</center> |
| `min_seeding_time`   | Will ensure that torrent is not deleted by cleanup variable if torrent does not meet minimum seeding time (min).                                                                      | None           | <center>❌</center> |
| `exclude_tags`       | List of tags to exclude from the check. Torrents with any of these tags will not be processed. This is useful to exclude certain trackers from being scanned for hardlinking purposes | None           | <center>❌</center> |

## **recyclebin:**

---
  Recycle Bin method of deletion will move files into the recycle bin (Located in /root_dir/.RecycleBin) instead of directly deleting them in qbit.

  This is very useful if you're hesitant about using this script to delete information off your system hingswithout first checking it. Plus with the ability of this script to remove trumped/unregistered torrents there is a very small chance that something may happen to cause the script to go to town on  your library. With the recycling bin in place your data is secure (unless the bin is emptied before this issue is caught). All you'd need to do to recover would be to place the data back into the correct directory, redownload the torrent file from the tracker and recheck the torrent with the tracker from the UI.

| Variable             | Definition                                                                                                                                                                                 | Default Values | Required           |
| :------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------- | :----------------- |
| `enable`             | `true` or `false`                                                                                                                                                                          | `true`         | <center>✅</center> |
| `empty_after_x_days` | Will delete Recycle Bin contents if the files have been in the Recycle Bin for more than x days. (Uses date modified to track the time)                                                    | None           | <center>❌</center> |
| `save_torrents`      | This will save a copy of your .torrent and .fastresume file in the recycle bin before deleting it from qbittorrent. This requires the [torrents_dir](#directory) to be defined             | False          | <center>❌</center> |
| `split_by_category`  | This will split the recycle bin folder by the save path defined in the [cat](#cat) attribute and add the base folder name of the recycle bin that was defined in [recycle_bin](#directory) | False          | <center>❌</center> |
> Note: The more time you place for the `empty_after_x_days:` variable the better, allowing you more time to catch any mistakes by the script. If the variable is set to `0` it will delete contents immediately after every script run. If the variable is not set it will never delete the contents of the Recycle Bin.

## **orphaned:**

---
This section allows for the exclusion of certain files from being considered "Orphaned"

This is handy when you have automatically generated files that certain OSs decide to make. `.DS_Store` Is a primary example, for those who use MacOS.

| Variable             | Definition                                                                                                                                  | Default Values | Required           |
| :------------------- | :------------------------------------------------------------------------------------------------------------------------------------------ | :------------- | :----------------- |
| `empty_after_x_days` | Will delete Orphaned data contents if the files have been in the Orphaned data for more than x days. (Uses date modified to track the time) | None           | <center>❌</center> |
| `exclude_patterns`   | List of [patterns](https://commandbox.ortusbooks.com/usage/parameters/globbing-patterns) to exclude certain files from orphaned             | None           | <center>❌</center> |
> Note: The more time you place for the `empty_after_x_days:` variable the better, allowing you more time to catch any mistakes by the script. If the variable is set to `0` it will delete contents immediately after every script run. If the variable is not set it will never delete the contents of the Orphaned Data.

## **apprise:**

---
[Apprise](https://github.com/caronc/apprise) integration is used in conjunction with webhooks to allow notifications via apprise-api.

| Variable     | Definition                                                          | Default Values | Required           |
| :----------- | :------------------------------------------------------------------ | :------------- | :----------------- |
| `api_url`    | Apprise API Endpoint URL                                            | N/A            | <center>✅</center> |
| `notify_url` | [Notification Services URL](https://github.com/caronc/apprise/wiki) | N/A            | <center>✅</center> |

## **notifiarr:**

---
[Notifiarr](https://notifiarr.com/) integration is used in conjunction with webhooks to allow discord notifications via Notifiarr.

| Variable   | Definition                                                                                              | Default Values | Required           |
| :--------- | :------------------------------------------------------------------------------------------------------ | :------------- | :----------------- |
| `apikey`   | Notifiarr API Key                                                                                       | N/A            | <center>✅</center> |
| `instance` | Optional unique value used to identify your instance. (could be your username on notifiarr for example) | N/A            | <center>❌</center> |

## **webhooks:**

---
Provide webhook notifications based on event triggers

| Variable                                                        | Notification Sent                                                    | Default Values | Required           |
| :-------------------------------------------------------------- | :------------------------------------------------------------------- | :------------- | :----------------- |
| [error](#error-notifications)                                   | When errors occur during the run                                     | N/A            | <center>❌</center> |
| [run_start](#run-start-notifications)                           | At the beginning of every run                                        | N/A            | <center>❌</center> |
| [run_end](#run-end-notifications)                               | At the end of every run                                              | N/A            | <center>❌</center> |
| [cross_seed](#cross-seed-notifications)                         | During the cross-seed function                                       | N/A            | <center>❌</center> |
| [recheck](#recheck-notifications)                               | During the recheck function                                          | N/A            | <center>❌</center> |
| [cat_update](#category-update-notifications)                    | During the category update function                                  | N/A            | <center>❌</center> |
| [tag_update](#tag-update-notifications)                         | During the tag update function                                       | N/A            | <center>❌</center> |
| [rem_unregistered](#remove-unregistered-torrents-notifications) | During the removing unregistered torrents function                   | N/A            | <center>❌</center> |
| [tag_tracker_error](#tag-tracker-error-notifications)           | During the removing unregistered torrents/tag tracker error function | N/A            | <center>❌</center> |
| [rem_orphaned](#remove-orphaned-files-notifications)            | During the removing orphaned function                                | N/A            | <center>❌</center> |
| [tag_nohardlinks](#tag-no-hardlinks-notifications)              | During the tag no hardlinks function                                 | N/A            | <center>❌</center> |
| [cleanup_dirs](#cleanup-directories-notifications)              | When files are deleted from certain directories                      | N/A            | <center>❌</center> |

### **Error Notifications**

Payload will be sent on any errors

```json
{
  "function": "run_error",     // Webhook Trigger keyword
  "title": str,                // Title of the Payload
  "body": str,                 // Error Message of the Payload
  "critical": bool,            // Critical Error
  "type": str                  // severity of error
}
```

### **Run Start Notifications**

Payload will be sent at the start of the run

```json
{
  "function": "run_start",     // Webhook Trigger keyword
  "title": str,                // Title of the Payload
  "body": str,                 // Message of the Payload
  "start_time": str,           // Time Run is started Format "YYYY-mm-dd HH:MM:SS"
  "dry_run": bool              // Dry-Run
}
```

### **Run End Notifications**

Payload will be sent at the end of the run

```json
{
  "function": "run_end",                      // Webhook Trigger keyword
  "title": str,                               // Title of the Payload
  "body": str,                                // Message of the Payload
  "start_time": str,                          // Time Run started Format "YYYY-mm-dd HH:MM:SS"
  "end_time": str,                            // Time Run ended Format "YYYY-mm-dd HH:MM:SS"
  "next_run": str,                            // Time Next Run Format "YYYY-mm-dd HH:MM:SS"
  "run_time": str,                            // Total Run Time "H:MM:SS"
  "torrents_added": int,                      // Total Torrents Added
  "torrents_deleted": int,                    // Total Torrents Deleted
  "torrents_deleted_and_contents_count": int, // Total Torrents + Contents Deleted
  "torrents_resumed": int,                    // Total Torrents Resumed
  "torrents_rechecked": int,                  // Total Torrents Rechecked
  "torrents_categorized": int,                // Total Torrents Categorized
  "torrents_tagged": int,                     // Total Torrents Tagged
  "remove_unregistered": int,                 // Total Unregistered Torrents Removed
  "torrents_tagged_tracker_error": int,       // Total Tracker Error Torrents Tagged
  "torrents_untagged_tracker_error": int,     // Total Tracker Error Torrents untagged
  "orphaned_files_found": int,                // Total Orphaned Files Found
  "torrents_tagged_no_hardlinks": int,        // Total noHL Torrents Tagged
  "torrents_untagged_no_hardlinks": int,      // Total noHL Torrents untagged
  "files_deleted_from_recyclebin": int,       // Total Files Deleted from Recycle Bin
  "files_deleted_from_orphaned": int          // Total Files Deleted from Orphaned Data
}
```

### **Cross-Seed Notifications**

Payload will be sent when adding a cross-seed torrent to qBittorrent if the original torrent is complete

```json
{
  "function": "cross_seed",             // Webhook Trigger keyword
  "title": str,                         // Title of the Payload
  "body": str,                          // Message of the Payload
  "torrent_name": str,                  // Torrent Name
  "torrent_category": str,              // Torrent Category
  "torrent_save_path": str,             // Torrent Download directory
  "torrent_tag": "cross-seed",          // Total Torrents Added
  "torrent_tracker": str                // Torrent Tracker
}
```

Payload will be sent when there are existing torrents found that are missing the cross-seed tag

```json
{
  "function": "tag_cross_seed",         // Webhook Trigger keyword
  "title": str,                         // Title of the Payload
  "body": str,                          // Message of the Payload
  "torrent_name": str,                  // Torrent Name
  "torrent_category": str,              // Torrent Category
  "torrent_tag": "cross-seed",          // Tag Added
  "torrent_tracker": str                // Torrent Tracker
}
```

### **Recheck Notifications**

Payload will be sent when rechecking/resuming a torrent that is paused

```json
{
  "function": "recheck",             // Webhook Trigger keyword
  "title": str,                      // Title of the Payload
  "body": str,                       // Message of the Payload
  "torrent_name": str,               // Torrent Name
  "torrent_category": str,           // Torrent Category
  "torrent_tracker": str,            // Torrent Tracker URL
  "notifiarr_indexer": str,          // Notifiarr React name/id for indexer
}
```

### **Category Update Notifications**

Payload will be sent when updating torrents with missing category

```json
{
  "function": "cat_update",          // Webhook Trigger keyword
  "title": str,                      // Title of the Payload
  "body": str,                       // Message of the Payload
  "torrent_name": str,               // Torrent Name
  "torrent_category": str,           // New Torrent Category
  "torrent_tracker": str,            // Torrent Tracker URL
  "notifiarr_indexer": str,          // Notifiarr React name/id for indexer
}
```

### **Tag Update Notifications**

Payload will be sent when updating torrents with missing tag

```json
{
  "function": "tag_update",                 // Webhook Trigger keyword
  "title": str,                             // Title of the Payload
  "body": str,                              // Message of the Payload
  "torrent_name": str,                      // Torrent Name
  "torrent_category": str,                  // Torrent Category
  "torrent_tag": str,                       // New Torrent Tag
  "torrent_tracker": str,                   // Torrent Tracker URL
  "notifiarr_indexer": str,                 // Notifiarr React name/id for indexer
  "torrent_max_ratio": float,               // Set the Max Ratio Share Limit
  "torrent_max_seeding_time": int,          // Set the Max Seeing Time (min) Share Limit
  "torrent_limit_upload_speed": int         // Set the the torrent upload speed limit
}
```

### **Remove Unregistered Torrents Notifications**

Payload will be sent when Unregistered Torrents are found

```json
{
  "function": "rem_unregistered",          // Webhook Trigger keyword
  "title": str,                            // Title of the Payload
  "body": str,                             // Message of the Payload
  "torrent_name": str,                     // Torrent Name
  "torrent_category": str,                 // Torrent Category
  "torrent_status": str,                   // Torrent Tracker Status message
  "torrent_tracker": str,                  // Torrent Tracker URL
  "notifiarr_indexer": str,                // Notifiarr React name/id for indexer
  "torrents_deleted_and_contents": bool,   // Deleted Torrents and contents or Deleted just the torrent
}
```

### **Tag Tracker Error Notifications**

Payload will be sent when trackers with errors are tagged/untagged

```json
{
  "function": "tag_tracker_error",                   // Webhook Trigger keyword
  "title": str,                                      // Title of the Payload
  "body": str,                                       // Message of the Payload
  "torrent_name": str,                               // Torrent Name
  "torrent_category": str,                           // Torrent Category
  "torrent_tag": "issue",                            // Tag Added
  "torrent_status": str,                             // Torrent Tracker Status message
  "torrent_tracker": str,                            // Torrent Tracker URL
  "notifiarr_indexer": str,                          // Notifiarr React name/id for indexer
}
```

```json
{
  "function": "untag_tracker_error",                 // Webhook Trigger keyword
  "title": str,                                      // Title of the Payload
  "body": str,                                       // Message of the Payload
  "torrent_name": str,                               // Torrent Name
  "torrent_category": str,                           // Torrent Category
  "torrent_tag": str,                                // Tag Added
  "torrent_tracker": str,                            // Torrent Tracker URL
  "notifiarr_indexer": str,                          // Notifiarr React name/id for indexer
}
```

### **Remove Orphaned Files Notifications**

Payload will be sent when Orphaned Files are found and moved into the orphaned folder

```json
{
  "function": "rem_orphaned",          // Webhook Trigger keyword
  "title": str,                        // Title of the Payload
  "body": str,                         // Message of the Payload
  "orphaned_files": list,              // List of orphaned files
  "orphaned_directory": str,           // Folder path where orphaned files will be moved to
  "total_orphaned_files": int,         // Total number of orphaned files found
}
```

### **Tag No Hardlinks Notifications**

Payload will be sent when no hard links are found for any files in a particular torrent

```json
{
  "function": "tag_nohardlinks",            // Webhook Trigger keyword
  "title": str,                             // Title of the Payload
  "body": str,                              // Message of the Payload
  "torrent_name": str,                      // Torrent Name
  "torrent_category": str,                  // Torrent Category
  "torrent_tag": 'noHL',                    // Add `noHL` to Torrent Tags
  "torrent_tracker": str,                   // Torrent Tracker URL
  "notifiarr_indexer": str,                 // Notifiarr React name/id for indexer
  "torrent_max_ratio": float,               // Set the Max Ratio Share Limit
  "torrent_max_seeding_time": int,          // Set the Max Seeing Time (min) Share Limit
  "torrent_limit_upload_speed": int         // Set the the torrent upload speed limit
}
```

Payload will be sent when hard links are found for any torrents that were previously tagged with `noHL`

```json
{
  "function": "untag_nohardlinks",          // Webhook Trigger keyword
  "title": str,                             // Title of the Payload
  "body": str,                              // Message of the Payload
  "torrent_name": str,                      // Torrent Name
  "torrent_category": str,                  // Torrent Category
  "torrent_tag": 'noHL',                    // Remove `noHL` from Torrent Tags
  "torrent_tracker": str,                   // Torrent Tracker URL
  "notifiarr_indexer": str,                 // Notifiarr React name/id for indexer
  "torrent_max_ratio": float,               // Set the Max Ratio Share Limit
  "torrent_max_seeding_time": int,          // Set the Max Seeing Time (min) Share Limit
  "torrent_limit_upload_speed": int         // Set the the torrent upload speed limit
}
```

Payload will be sent when `cleanup` flag is set to true and `noHL` torrent meets share limit criteria.

```json
{
  "function": "cleanup_tag_nohardlinks",    // Webhook Trigger keyword
  "title": str,                             // Title of the Payload
  "body": str,                              // Message of the Payload
  "torrent_name": str,                      // Torrent Name
  "torrent_category": str,                  // Torrent Category
  "cleanup": True,                          // Cleanup flag
  "torrent_tracker": str,                   // Torrent Tracker URL
  "notifiarr_indexer": str,                 // Notifiarr React name/id for indexer
  "torrents_deleted_and_contents": bool,    // Deleted Torrents and contents or Deleted just the torrent
}
```

### **Cleanup directories Notifications**

Payload will be sent when files are deleted/cleaned up from the various folders

```json
{
  "function": "cleanup_dirs",             // Webhook Trigger keyword
  "location": str,                        // Location of the folder that is being cleaned
  "title": str,                           // Title of the Payload
  "body": str,                            // Message of the Payload
  "files": list,                          // List of files that were deleted from the location
  "empty_after_x_days": int,              // Number of days that the files will be kept in the location
  "size_in_bytes": int,                   // Total number of bytes deleted from the location
}
```

## **bhd:**

---
BHD integration is used if you are on the private tracker BHD. (Used to identify any unregistered torrents from this tracker)

| Variable | Definition  | Default Values | Required           |
| :------- | :---------- | :------------- | :----------------- |
| `apikey` | BHD API Key | N/A            | <center>✅</center> |
