<div style="position: absolute; height: 97%; width: 100%;">
  <div style="margin:30px auto 50px 0px; width: 90%; text-align: center">
    <h2 style="font-size:1.8em; font-weight:bold;">
      Test Driven Drupal Upgrades
    </h2>
    <div style="font-size:0.7em;">
      <div style="font-size:1.2em; font-weight:bold;">Dave Vasilevsky</div>
      <div> vasi@evolvingweb.ca </div>
      <div> github.com/vasi </div>
      <div> twitter.com/djvasi </div>
      <div> drupal.org/u/vasi </div>
    </div>
  </div>

  <p><img src="img/ew-good.png" style="height:130px; margin: 10px 0 0 0; position: absolute; bottom: 0;"></p>
</div>

--end--

## Outline

* __About me__
* __Intro__
* __Basics__
  * Minor Updates
  * Major Core Upgrades
  * Testing
* __Tools__
  * behat
  * docker
  * CircleCI
      * __drupal-docker-marriage demo__
  * PHPUnit
  * sitediff
      * __SiteDiff demo__

--end--

## About Evolving Web

* Drupal development, consulting and training since 2007
* Very involved with the Drupal community
* Specialties
  * Large, scalable infrastructure and deployments
  * Multilingual content management
  * Apache Solr search interfaces
  * Content import and synchronization
  * Custom theme development
  * Custom module development
  * Search engine optimization for Drupal (SEO)
  * Integration with legacy systems
  * Expert Drupal training
* Based in Montreal, clients in Canada and USA
* EW is hiring! Let us know if you're interested

--end--

![](img/ew-projects.png)

--end--

## Drupal training program

<img src="https://evolvingweb.ca/sites/default/files/d7/_Q4A1828_0.jpg" width="25%" style="float: right; margin-left: 40px; margin-right: 20px" />

* Public: Montreal, Ottawa, Toronto, DC, Munich, NJ, NYC, Boston, Chicago
* Private: Health Canada, Parks Canada, Tourism Quebec, Trent U, McGill U
* Enterprise teams, dev shops, remote

--end--

## About me

* I've been at Evolving Web since 2008, taught my coworkers about version control
  * Working with Drupal since D6
* Participate in lots of open source projects: Fink, Firefox, grub, pixz, and of course Drupal core!

--end--

## Introduction

* Upgrades are important for security
  * Security bugs are constantly found in Drupal and Drupal modules
  * Eg: Drupalgeddon (SA-CORE-2014-005) fixed in 7.32
* Upgrades bring new features
  * Drupal 7 has many performance improving features over D6
  * Webform 4 brings token support
* Drupal 6 is now EOL! No more security fixes

--end--

## Introduction

* Many Drupal developers aren't great at upgrading. Why?
  * Not everyone knows how
  * It can take time
  * We're afraid of regressions
  * We hate manual testing

--end--

## How to change

* Learn how to do upgrades
* Good tools make it easier and faster
* Testing makes it safe

--end--

## Minor updates vs. major upgrades

* Minor updates: 7.35 -> 7.36
  * Modules need updates too!
  * Perform these as often as possible, to keep up with security fixes
* Major upgrades: 6.28 -> 8.0.5
  * Bring many, many new features and opportunities
  * Necessary before D6 is obsolute

--end--

## Minor update basics

* When to update: Use the Drupal Security Advisories mailing list: https://www.drupal.org/security
  * Or use the update module
* Where to update: In staging
  * Never do an update for the first time in production, you don't know if anything will break
* Watch out for hacks or patches to your modules!
  * Use the ```hacked``` module to find them

![](img/update-module.png)

--end--

## Minor update basics

* How to update in place:
  * ```drush pm-updatestatus``` to list available updates
  * ```drush pm-update``` to perform updates
* For real sites in production:
  * Perform manual update on dev/staging, test
  * Commit
  * Deploy to staging and test
  * Deploy on prod
* Update hooks
  * Keep database in sync with versions of code
  * Eg: new column in database; rename variable
  * Running them: ```update.php``` or ```drush updb```

--end--

## Major upgrade basics

...

--end--

## Major upgrade basics

The first rule of major upgrades is there is no such thing as a basic major upgrade.

--end--

## Major upgrade basics

* Major upgrades can be as hard as a site rebuild
  * Drupal changes in ways that aren't backwards-compatible
  * Modules may not be updated yet, or at all

--end--

## Major upgrades, grandpa style

You might still want to do D6 to D7 upgrades sometimes:

* Avoid D6 EOL
* Can perform the update in-place
* Your custom modules might need only small fixes

--end--

## Major upgrades, grandpa style

In D6:

* Update core and contrib to latest D6 version
* Minimize the site: disable custom theme, modules, some contrib

Do the upgrade:

* Update core and contrib code to highest D7 version
* Run `drush updb`
* Use content_migrate to move from CCK to fields
* Upgrade and re-enable modules
* Fix all the breakage! Lots of ways things can go wrong

--end--

## Major upgrades, D8

No more in-place upgrades!

* Build your brand new D8 site
  * Can't keep your custom modules or theme
* Migrate content from your existing D6 or D7 site

--end--

## Major upgrades, D8

For a simple site, it's easy:

1. Create an empty D8 site, install migrate_upgrade
1. Visit `/upgrade', give D6/D7 DB credentials
1. Go!

All your settings and content will be pulled into D8! But still...

* Lots of things aren't working yet, eg: multilingual content
* Needs to rebuild custom modules and themes
* Contrib modules may not be ready, eg: panels

--end--

## Major upgrades, D8

For a more complex site, you might just want to do a site rebuild.

* Build a nice new site from scratch
* But still get some or all content from D6/D7
  * Run `drush migrate-upgrade --configure` to configure upgrade migrations without running them
  * Then edit/remove migrate config files as desired
  * Use `drush migrate-import` from migrate_tools

--end--

## Testing basics

* Unit testing
* Integration testing
* UI testing
* Continuous integration

--end--

## Unit testing

* Fast, good for standalone functions
* Use fixtures for testing
* Great for your custom modules, or core contributions
* D7: SimpleTest (DrupalUnitTestCase)
* D8: PHPUnit (UnitTestCase)
* But when you update a contrib module, your unit tests won't care at all. They're not super useful for testing updates

--end--

## Integration testing

* Tests how your module integrates with other modules
  * But only the modules that are necessary for your module to work, not the whole site
* D7: SimpleTest (DrupalWebTestCase)
* D8: PHPUnit (KernelTestBase)
* Powerful Drupal integration: Enable modules, create content, add users...
* Can be really slow (esp. D7), may need to setup the DB for each test
* Can't test things like JavaScript, CSS

--end--

## UI testing

* Tests your site by controlling a real browser
* Very powerful and thorough
* Not built-in to Drupal, need external tools
  * Eg: Selenium, CasperJS, behat
  * May be built-in in 8.2?
* Great replacement for manual testing
* Unlike above tests, there's no isolation. Your tests can change your site!

--end--

## Tools

This is informative, but a lot of theory. How do we actually make it work in practice?

* What problems have we encountered on our projects?
* What tools have we used to solve them?

--end--

## Case Study - Certification Tool

An Internet of Things consortium needed to [certify products that met its standards](http://certify.alljoyn.org).

Challenges:

* Complex data, with constraints
* Relationships between products
* Complex permissions: editing and workflow
* Slowly grew into giant, fragile forms of doom
* Several developers

--end--

## Behat

Problem: Every code change could break the form
<br />
Solution: Test the form's behavior

We used [behat](http://docs.behat.org) and its [Drupal extension](https://behat-drupal-extension.readthedocs.org)

* Why BDD?
* Why use behat?
  * UI testing
  * Drupal integration
  * Easily understood tests
* Found tons of bugs

--end--

## behat scenarios

Here's an example of a test for behat:

    Scenario: Show author on hover
      Given I am viewing an "article" content:
      | title | author          | body  |
      | Lorem | bob@example.com | Ipsum |
      When I hover over the "author" region
      Then I should see the text "Bob"

Here's how we implemented the "hover" rule above, in a custom behat context:

    /**
      * @When I hover over the :region region
      */
    public function iHoverOverRegion($region) {
      getRegion($region)->mouseOver();
    }

--end--

## Docker

Problem: Bugs aren't reproducible
<br />
Solution: Put everyone in the same environment

* Easily build and run virtualized containers
* Easy to spin up an exact copy of your site
  * If something breaks, just spin it up again
  * Very useful for minor updates!
* Consistent environment in dev/staging/prod

--end--

## Docker

* Dockerfile build process
  * Starts with clean distro image
  * Installs all necessary packages: mysql, tomcat, solr, nginx, xhprof, xdebug, ...
  * Runs our deploy scripts
* Caching

<pre>
FROM ubuntu:14.04
RUN apt-get update && apt-get install -y mysql apache2 ...
RUN a2enmod ssl
COPY code /var/www/html
...
</pre>

--end--

## Continuous integration

Problem: Tests are slow, devs avoid running them
<br />
Solution: Run tests in the background

* Run your tests automatically for every commit
* Usually uses a build server
* Reports on the results

--end--

## CircleCI

We use [CircleCI](http://circleci.com) for our continuous integration:

* Integrates with GitHub branches and pull requests
* Email notifications when something breaks
* Catches event unexpected bugs, eg: servers disappearing, unmaintained packages
* Allows use of docker, so test environment is consistent with dev/prod

--end--

## CircleCI

CircleCI is configured with a circle.yml file:

<pre class="prettyprint lang-yaml">
machine:
  services:
    - docker

dependencies:
  override:
    - docker build -t myproject .
    - docker run -p 9022:22 myproject

test:
  override:
    - "ssh -p 9022 drupal@localhost 'cd /var/www && drush test-run'"
</pre>

--end--

## Behat, CircleCI, Docker demo

![](img/pull-request-screenshot.png)

[github.com/evolvingweb/drupal-docker-marriage](https://github.com/evolvingweb/drupal-docker-marriage)

[Demo notes](https://github.com/vasi/ew-presentations/blob/gh-pages/midcamp-2016-upgrades/demo-marriage.md)

--end--

## Case Study- McGill calendar

McGill used a D6 site for their course calendar, wanted to go to D7.

Challenges:
* Many custom modules
* Lots of content, complex structure
* Legal requirement that it be correct and complete!
* Logically nested menus: menu + book

--end--

## PHPUnit

Problem: Merging menus is tricky
<br />
Solution: Tests with fixtures

* Single entry point to contain complexity
* List of potential inputs and desired outputs

--end--

## SiteDiff

Problem: Code must change, content must stay the same
<br />
Solution: Compare the content

[github.com/evolvingweb/sitediff](https://github.com/evolvingweb/sitediff)

* Downloads list of pages from two sites
* Computes diff of HTML
* Cleans up spurious changes, like absolute domains
* Reports changes via command-line UI and web report
* Break down huge upgrade into simple tasks

--end--

## SiteDiff configuration

<pre class="prettyprint lang-yaml">
before_url: http://docker:9179
after_url: http://docker:9180
paths:
- /
- /about-us
- /user/3/track
sanitization:
- pattern: http:\/\/[a-zA-Z0-9.:-]+
  substitute: __domain__
</pre>

--end--

## SiteDiff output

![](img/sitediff-report.png)
![](img/sitediff-diff.png)

--end--

## SiteDiff

* Advantages:
  * Thoroughness
  * Black-box
  * Speed
* Limitations:
  * JavaScript
  * Dynamic content
  * Admin UI

--end--

## SiteDiff

SiteDiff turns out to be useful on many projects

* Refactorings
* Dev vs Prod
* Content migration
* Upgrades! Little should change

--end--

## SiteDiff demo

![](img/sitediff-screenshot.png)

[github.com/vasi/sitediff-update-demo](https://github.com/vasi/sitediff-update-demo)

[Demo notes](https://github.com/vasi/ew-presentations/blob/gh-pages/midcamp-2016-upgrades/demo-sitediff.md)

--end--

## Any questions?

* Evolving Web: [evolvingweb.ca](http://evolvingweb.ca)
* SiteDiff: [github.com/evolvingweb/sitediff](https://github.com/evolvingweb/sitediff)
* Demo of SiteDiff: [github.com/vasi/sitediff-update-demo](https://github.com/vasi/sitediff-update-demo)
* Demo of docker, behat, CircleCI: [github.com/evolvingweb/drupal-docker-marriage](https://github.com/evolvingweb/drupal-docker-marriage)

--end--
