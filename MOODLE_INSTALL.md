# Moodle installation

Language select continues

## Confirm paths

* Paths are auto populated and were mapped during docker build
  * `/var/www/html/moodle` is mapped to `~/moodle_data`
  * `/var/www/html/moodledata` is mapped to `~/moodledata_data`

### Database driver

* Select `MariaDB` from dropdown menu

### Database settings

| key               | value             | notes                      |
| ----------------- | ----------------- | -------------------------- |
| database host     | `db`            |                            |
| database name     | `moodle`        |                            |
| database user     | `moodle`        |                            |
| database password | `dbPassword123` |                            |
| table prefix      | `mdl_`          |                            |
| database port     | `3306`          |                            |
| unix socket       |                   | *leave this field blank* |

* Confirm database settings and click "next" button

### Setup and installation

* Moodle will set up tables, click "continue" when completed
* Agree to lincense and click "continue"
* wait for installation to complete and click "continue" when done

> Next step takes a few minutes, if you are connected via ssh you'll be disconnected, this is normal behavior. wait a few minutes process should finish about 10-15 minutes

### Create admin account

* Fill out the admin account form, including username, password, email, first name, and last name. Make sure to remember your credentials, as you'll need them to log in and manage your Moodle site
* Click "Update profile" to create the admin account

### Set up front page settings

* Fill out the information for your site, such as the full site name, short name, and description. You can customize other settings, like the front page format and default language
* Click "Save changes" to proceed

---

Moodle setup is now complete. You will be redirected to the front page of your Moodle site.

Visit the [Moodle Docs](https://docs.moodle.org/) and the [Moodle community forums](https://moodle.org/course/) for additional resources and support.

---
