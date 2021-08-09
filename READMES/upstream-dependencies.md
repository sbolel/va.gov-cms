# Upstream Dependencies

## Background

This document lists known external dependencies for the CMS.
Official points of contact for each service are listed.
Other points of contact for some services are listed on [DSVA Slack](https://dsva.slack.com/archives/CT4GZBM8F/p1628284192216100).

## Runtime

The following services can affect the CMS's functionality or data at any time.

* [GovDelivery](https://granicus.com/solution/govdelivery/)
    * Content
        * Situation updates & alerts are sent to subscribed users via GovDelivery
    * Mode
        * Uses Post API & govdelivery_bulletins modules to post data to the GovDelivery API endpoint
    * Monitoring: 
        * External
            * [https://status.granicus.com/](https://status.granicus.com/)
    * Escalation contact
        * [https://support.granicus.com/s/contactsupport](https://support.granicus.com/s/contactsupport)
* Facility Status (Team Sites)
    * Content
        * Facility operational status & additional information
    * Mode
        * Periodic migration pulls data from TSV endpoints on teamsite ([config](/config/sync/migrate_plus.migration.va_node_health_care_local_facility_status.yml))
    * Notes
        * This should be completely phased out by the end of 2021
    * Monitoring
        * External
            * None known.
    * Escalation contact
        * See [DSVA Slack](https://dsva.slack.com/archives/CT4GZBM8F/p1628284192216100)
* Facility API (via lighthouse)
    * Content
        * Operating hours, Contact information, names for all facilities (VHA facilities, vet centers, cemeteries, business offices)
            * [README](https://github.com/department-of-veterans-affairs/va.gov-cms/blob/master/READMES/migrations-facility.md)
    * Mode
        * Nightly migration pulls data from the Lighthouse API ([specific migrations specified here](/tasks-periodic.yml))
    * Monitoring
        * External
            * https://valighthouse.statuspage.io
* Forms API (nightly DB dump)
    * Content
        * form data (names, filenames, audiences, status) and creates/updates “VA Form” nodes
    * Mode
        * Nightly migration ([README](https://github.com/department-of-veterans-affairs/va.gov-cms/blob/master/READMES/migrations-forms.md))
    * Monitoring
        * External
            *  Unknown
    * Escalation contact
        * #va-forms slack channel
* Jenkins (manual & automatically triggered content releases)
    * Content
        * Editors can trigger a content release either manually or by editing certain content types
    * Mode
        * Calls jenkins webhook ([README](https://github.com/department-of-veterans-affairs/va.gov-cms/blob/master/READMES/cms-content-release.md#automatic))
    * Monitoring
        * External
            * [http://jenkins.vfs.va.gov/computer/](http://jenkins.vfs.va.gov/computer/)
    * Escalation contact
        * Ops team (use #vfs-platform-support)
* Slack (notifications)
    * Content
        * Post API failure alerts, Teamsite facility status failure alerts
    * Mode
        * Drupal calls Slack webhook
    * Monitoring
        * External
            * [https://status.slack.com/](https://status.slack.com/) 
    * Escalation contact
        * Unknown
* Lighthouse
    * Mode
        * Cron uses post api module to process updates queue
    * Data
        * VAMC & Vet Center Facility statuses
        * Cemetery statuses
        * Facility health services (covid only now, all services soon)
    * Monitoring
        * External
            * https://valighthouse.statuspage.io
    * Escalation contact
        * [#vsa-facilities slack channel](https://dsva.slack.com/archives/C0FQSS30V) - Adam Stinton

## Build time

The following services can affect the deployment process' ability to fully build the CMS for testing or production deployment.

* Remi Repo
    * URL
        * rpms.remirepo.org (php)
    * Status
        * none
    * Notes
        * Remi Repo is used to pull in the PHP 7.3 libraries and dependencies in our AMI builds. This won't be used when we switch from Amazon Linux 1 to Amazon Linux 2 when we move to containers on ArgoKube
    * Escalation process
        * Tweet [@RemiRepository](https://twitter.com/RemiRepository) and open issue at [https://forum.remirepo.net/](https://forum.remirepo.net/)
* [Drupal packages](packages.drupal.org)
    * Status
        * none
    * Notes
        * Drupal packages is used to download Drupal contrib modules
* [Packagist](https://packagist.org)
    * Status
        * [https://status.packagist.org/](https://status.packagist.org/)
    * Notes
        * Packagist is used to install our PHP dependencies that are required by Drupal custom and contrib modules.
    * Escalation process
        * Tweet at [@packagist](https://twitter.com/packagist). It is used by thousands of sites so highly likely that someone knows about any issues before we do and that it will be resolved quickly.
* [Github](https://github.com)
    * Status
        * [https://www.githubstatus.com/](https://www.githubstatus.com/)
    * Notes
        * The codebase is stored in github, and the deployment process depends on it to pull code and push status and code quality messages to our pull requests.
    * Escalation process
        * Use the #github_information channel in DSVA slack
* [Docker Hub](https://hub.docker.com/)
    * Status
        * [https://status.docker.com/](https://status.docker.com/)
    * Notes
        * We use Docker Hub to pull down container images for our CI environments in Tugboat.
    * Escalation process
        * Contact support@docker.com and/or tweet [@Docker](https://twitter.com/Docker)