# A Moodle Plugin Repo
Composer-friendly metadata for Moodle plugins that are hosted on moodle.org.

## Description
This repo generates metadata that allow projects to pull in Moodle plugins using Composer, rather than downloading them directly from the [Moodle Plugins directory](https://moodle.org/plugins/index.php). This allows for much better dependency management, including quick installs and updates, all managed via command line.

Under the hood, this repo uses [`micaherne/moodle-plugin-repo`](https://github.com/micaherne/moodle-plugin-repo) (with a patch) to generate metadata.

## How to Use the Metadata
To begin, we assume that you are *not* using the `composer.json` that is included in the Moodle package. We prefer to manage Moodle as a dependency alongside its plugins, so we need a `composer.json` external to the docroot that contains Moodle.

In the `repositories` section of your project's `composer.json`, add this project's url.
```
repositories: [
  {
    "type": "composer",
    "url": "https://civicactions.github.io/moodle-plugin-repo"
  }
]
```
The author of a plugin isn't always obvious. So, to add a plugin from the repo, prefix the plugin's name with the string `moodle-plugin-db/`. For example:
```
composer require moodle-plugin-db/mod_customcert:2022112801
```
Note that Moodle version numbers are not semantic versioning-compatible.

## How to Update the Metadata
Plugins update and the metadata can grow stale. Developers with appropriate credentials can update the metadata distributed here. Regenerating metadata will grab the latest plugin information from Moodle.org and put it in a format friendly to Composer.

1. Clone this repo locally. 
   ```
   git clone git@github.com:CivicActions/moodle-plugin-repo.git my-directory/
   ```
2. Install dependencies. This adds the `micaherne/moodle-plugin-repo` library which will do the heavy lifting.
   ```
   composer install
   ```
3. Run our custom script to regenerate the metadata.
   ```
   composer moodle-plugin-repo-update
   ```
4. Commit the new metadata and push it up to the repo.
   ```
   git add packages.json
   git commit . -m "Updated metadata [mm/dd/yyyy]"
   git push origin main
   ```
GitHub Pages will automatically rebuild when `main` is pushed to.
