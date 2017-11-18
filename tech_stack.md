# Tech Stack

Evacuteer does not have an IT staff, therefore the tech stack that runs needs to be mostly self-managing. Additionally, the project is motivated by the desire to expose computer science students to multiple technologies in a large software ecosystem. These two constraints, in addition to the requirement that the system have high up-time, especially in the case of an evacuation, are the primary drivers behind the system architecture.

## Data Sources

Currently, the primary source of truth for Evacuteer data is a CiviCRM database hosted at http://civicrm.org.

Eventually, we would like for the CiviCRM database to be an input to a centralized database that contains much more data than is practical to store in CiviCRM. The choice of data store is currently a [Google Cloud Datastore](https://cloud.google.com/datastore) instance. This datastore was chosen because it requires minimal IT staff and is visible to [Google App Engine](https://cloud.google.com/appengine), which is a hosting solution that requires minimal IT staff.

Currently, the schema to the datastore is not designed and the data store does not exist.

## Basic Architecture

The system will likely have one central server, [evacucore](https://github.com/Evacuteer/evacucore), surrounded by multiple consumers: [evacuweb](https://github.com/Evacuteer/evacuweb), [evacumac](https://github.com/Evacuteer/evacumac), [evacudroid](https://github.com/Evacuteer/evacumac), and [evacureact](https://github.com/Evacuteer/evacureact). The aforementioned consumers are all “real-time” consumers that serve as front-ends. There will also be an  [evaculize](https://github.com/Evacuteer/evaculize) consumer that does batch data analysis.

The system will be fed/synchronized by [evacusync](https://github.com/Evacuteer/evacusync).

And then [evacuspots](https://github.com/Evacuteer/evacuspots) is the static list of current evacuspots that are presented in geojson.

## Component Breakdowns

The details of each of the subcomponent designs and the motivations for each follow

### Evacuspots
Evacuspots is just a static list of actual physical evacuspots encoded in GeoJson. The exact locations of the evacuspots on the Evacuteer website are approximate. The evacuspot locations encoded in this data set are hopefully closer to the actual location of the evacuspot statue.

### Evacucore

Evacucore is the central API server for the system. Most likely this system will be a Google AppEngine Standard server running a Python binary and serving REST API endpoints and backed by a Google Cloud Datastore. The exact makeup of the API and the data storage layer are yet to be determined.

AppEngine was chosen because it is a system that can pretty much run without an IT department.

Google Cloud Datastore was chosen because it is easily visible within AppEngine and also visible via a published API.

Python was chosen because that is the only AppEnging language the the students at Dillard are familiar with. Java and Go would be fine too, but Python was what the students knew. 

### Evacuweb

One interface for the Evacuteer data is a website. This website should serve multiple types of users:

* The general public
* Evacuteer Volunteers
* Evacuteer Site Lead Volunteers
* Evacuteer Staff
* System Administrators

Each type of user will have a different interaction with the website. For instance, the general public will get evacuation information and be solicited to volunteer. Volunteers will be able to check their training status and set preferences, as well as, report for duty in an evacuation. Site leads will be able to have administrative access to the state of a site during an evacuation. Evacuteer staff will be able to change data across the system and run reports. System administrators will have “root” access and be able to support the system.

At first, evacuweb will likely be served by the same AppEngine Standard server that serves evacucore.

Evacuweb will be HTML5 + whatever framework is necessary, be that raw JavaScript, React, Angular, or something else.
Users will need to authenticate to get access to personalized data.

### Evacumac

Some of the students were interested in iOS development so we started and evacumac project to provide a frontend to Evacuteer in and iOS-native way. This project hasn’t gotten far.

### Evacudroid

Some of the students were interested in Android development so we started an evacudroid project to provide a frontend to Evacuteer in an Android-native way. This project hasn’t gotten far. 

### Evacureact

Code For New Orleans has one or more React Native developers who might be interested in supporting the project if we create the iOS and Android frontends in React Native. Because of this evacureact was born. This project hasn’t gotten far.

### Evaculize

Dr. Chiu is very interested in data analysis and there are actually some interesting question surrounding the locations of evacuspots. Louisiana State University (LSU) did a study that we need to site here. It doesn’t take much to see that the evacuspots and the population centers that will probably need assisted evacuation don’t quite line up. Dr. Chiu is interested in studying this. His expertise is C++, so this project will likely be in C++ if he is part of it. There are some good opportunities to analyze street data from Open Street Map along with evacuspot data from our own data store and overlay it with car ownership, bus ridership, and more.  

### Evacusync

Evacuteer has a few hundred volunteers that they need to keep in contact with to ensure that the volunteer training, evacuspot preferences, and contact information is up to date. Currently this is all stored in a CiviCRM database. This database is difficult to work with, so we need to set up a regular sync job that keeps a cloud datastore in sync with the CivicCRM database.
