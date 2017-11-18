# Tech Stack

Evacuteer does not have an IT staff, therefore the tech stack that runs needs to be mostly self-managing. Additionally, the project is motivated by the desire to expose computer science students to multiple technologies in a large software ecosystem. These two constraints, in addition to the requirement that the system have high up-time, especially in the case of an evacuation, are the primary drivers behind the system architecture.

## Data Sources

Currently, the primary source of truth for Evacuteer data is a CiviCRM database hosted at http://civicrm.org.

Eventually, we would like for the CiviCRM database to be an input to a centralized database that contains much more data than is practical to store in CiviCRM. The choice of data store is currently a [Google Cloud Datastore](https://cloud.google.com/datastore) instance. This datastore was chosen because it requires minimal IT staff and is visible to [Google App Engine](https://cloud.google.com/appengine), which is a hosting solution that requires minimal IT staff.

Currently, the schema to the datastore is not designed and the data store does not exist.

