# Use Cases and Requirements

* [Use Cases](#use-cases)
* [Requirements](#requirements)

![ecosystem diagram](https://raw.githubusercontent.com/solid/app-authorization-panel/master/images/app-auth-uc.png)

*Open ecosystem diagram where various parties play different roles*

## Use Cases

### Simple game

 - Alice uses https://simplegame.example to play a singleplayer game
    - Simplegame only needs some file somewhere that it can write its own configuration to. It does not care where it is
    - Simpleapp will also need to access this file again even if it’s being used on another machine

>* [An app can request access to a specific resource](#an-app-can-request-access-to-a-specific-resource)

### Chat with friends
 - Alice uses https://chat.o.team to chat with her friends
    - OChat wants to gain access to all chat related data
    - Wants access to chat related data that was created after it asked for permission to read them
    - Wants to be alerted when new chat related data has been added to the Pod
    - Wants to create chat related data

>* [An app can request access to a specific type of data without knowing the structure of resources on a Pod](#an-app-can-request-access-to-a-specific-type-of-data-without-knowing-the-structure-of-resources-on-a-pod)


### Chat with doctor

 - Alice uses https://doctorChat.example to chat with her doctor
    - DoctorChat wants to create chat related data specifically about medical information
    - DoctorChat wants to ensure Alice has given explicit consent to view the chats that it created before other apps can view this data

>* [It should be possible for an agent with Control access to block/allow certain apps from accessing a specific resource as any agent](#it-should-be-possible-for-an-agent-with-control-access-to-blockallow-certain-apps-from-accessing-a-specific-resource-as-any-agent)

### Discussion boards

- Alice uses iSay deployed on https://isay.alice.example/ to participate in discussion boards
  - She wants to trust iSay to access any discussion boards, including
    - Discussions on https://alice.example/
    - Discussions on https://acme.example/
    - Discussion on https://yoyodyne.example/
  - She wants to receive push notifications on devices she uses when someone replies to discussion she participates in - iSay inculdes a remote client component which provides that feature.
  - Alice, ACME and Yoyodyne want to allow each person with access to discussion boards on their resource servers to decide which applications they trust to participate in those discussions.
  - Alice only wants to allow iSay to access discussions to avoid other apps (eg. games) she tries to have any way of posting spam messges in any of those discussions

>* [The system is not abusable](#the-system-is-not-abusable)
>* [An app can request access to a specific type of data without knowing the structure of resources on a Pod](#an-app-can-request-access-to-a-specific-type-of-data-without-knowing-the-structure-of-resources-on-a-pod)

### RDF editor

 - Alice uses https://edit.o.team to edit her raw RDF
    - OEdit wants to be able to read and write to a file at a specific location

>* [An app can request access to a specific resource](#an-app-can-request-access-to-a-specific-resource)

### Pod administration
 - Alice uses https://admin.example to control her pod
    - Admin wants to be able to read and write to all files on a user’s Pod

>* [An app can request access to a specific resource](#an-app-can-request-access-to-a-specific-resource)

### Photos viewer

 - Alice uses https://decentPhotos.example to view her photos and her friend Bob’s photos
    - Decent photos wants to read to all photos on Alice’s Pod
    - Wants to read all photos on Bob’s Pod that Alice has access to

>* [An app can request access to a specific type of data without knowing the structure of resources on a Pod](#an-app-can-request-access-to-a-specific-type-of-data-without-knowing-the-structure-of-resources-on-a-pod)

### Photos organizer

 - Alice uses https://photoOrganizer.example to organize the photos on her Pod
    - PhotoOrganizer wants to read and write only photos
    - Wants to understand the folder structure of the Pod
    - Wants to modify the folder structure of the Pod

>* [An app can request access to a specific type of data without knowing the structure of resources on a Pod](#an-app-can-request-access-to-a-specific-type-of-data-without-knowing-the-structure-of-resources-on-a-pod)

### Photos processing

 - Alice uses FaceDetectionCronJob to crawl over her photos and Bob’s photos at night and produce facial recognition data
    - FaceDetectionCronJob is not an application and will need access to photos even when Alice is not actively using it
    - Wants to have read access to Alice’s photos
    - Wants to know where it should put its facial detection data
    - Wants to have read access to Bob’s photos that Alice has access to

>* [An app can request access to a specific type of data without knowing the structure of resources on a Pod](#an-app-can-request-access-to-a-specific-type-of-data-without-knowing-the-structure-of-resources-on-a-pod)
>* [Apps can request the ability to write a specific type of data and will be told where it should write it](#apps-can-request-the-ability-to-write-a-specific-type-of-data-and-will-be-told-where-it-should-write-it)

### Evil fitness tracker

 - Alice accidentally uses https://evilfitbit.example to track her fitness data
    - Evilfitbit will try to do everything to get Alice’s financial data while pretending to just track her fitness data (This should not be allowed)

>* [The system is not abusable](#the-system-is-not-abusable)

### Finances tracker

 - Alice uses https://financeTracker.example to view her current finances
    - financeTracker wants to be able to read Alice’s Financial data
    - financeTracker wants to be able to keep a backup of Alice’s data
    - Alice wants to ensure that financeTracker isn’t legally allowed to save the data it receives

### Restaurants tracker

 - Alice uses https://favoriteRestaurants.example to track the restaurants she likes
    - Favorite Restaurants wants to read and write data about restaurants and to get the Alice’s location
    - Alice only wants to allow Favorite Restaurants to read and write restaurant data but not to get her location

### Blogs

 - Alice uses https://blogs.example to read and write blogs
    - Blogs wants to be able to read public blog information and to write blog data to Alice’s Pod
    - Blogs wants to read data about Alice’s interests
    - Alice does not want Blogs to get data about her interests
    - Blogs continually asks Alice to grant it access to her interests and Alice is annoyed with the incessant asking
 - Alice uses, is developing, or is testing an app deployed to http://localhost:8080
    - Note that Alice's Identity Provider can't reach Alice's `localhost:8080`
    - Note that Alice's Pod can't reach Alice's `localhost:8080`
 - Alice uses an app deployed behind a NAT or firewall (while her browser is also behind the same NAT or firewall) that accesses resources outside the NAT or firewall; for example, Alice uses https://coolcode.int.enterprise.example to edit code stored in Customer Bob's Pod
    - *CoolCode* is deployed behind Enterprise.Example's company firewall and is not dereferenceable from the outside, for example because
      - *CoolCode* is proprietary to Enterprise.Example; or
      - *CoolCode* is a commercial product that is deployed on-premises at Enterprise.Example's datacenter
    - Note that Alice's Identity Provider can't reach https://coolcode.int.enterprise.example
    - Note that Customer Bob's Pod can't reach https://coolcode.int.enterprise.example

### Custom TLS CA
 - Alice uses https://photoOrganizer.example to organize photos on her company's private storage server https://storage.private.enterprise.example
    - `storage.private.enterprise.example`'s TLS certificate is signed by Enterprise.Example's private Certificate Authority
    - Alice's web browser is configured to trust Enterprise.Example's private Certificate Authority
    - `storage.private.enterprise.example` is reachable from the public Internet, so Alice's Identity Provider and *photoOrganizer* could reach it; however, neither is configured to trust Enterprise.Example's private Certificate Authority

## Requirements

A successful spec will satisfy the following requirements

### The system is not abusable

* [Evil fitness tracker](#evil-fitness-tracker)

### An app can request access to a specific resource

* [Simple game](#simple-game)
* [RDF editor](#rdf-editor)
* [Pod administration](#pod-administration)

### An app can request access to a specific type of data without knowing the structure of resources on a Pod

* [Chat with friends](#chat-with-friends)
* [Discussion boards](#discussion-boards)
* [Photos viewer](#photos-viewer)
* [Photos processing](#photos-processing)

### Access requests can be sent when the resource owner is not present to be approved once the user is present

### Apps can request the ability to write a specific type of data and will be told where it should write it

* [Photos processing](#photos-processing)

### It should be possible for an agent to block/allow certain apps from accessing a specific resource as that agent

### It should be possible for an agent with Control access to block/allow certain apps from accessing a specific resource as any agent

* [Chat with doctor](#chat-with-doctor)

### It should be easy to allow others accessing your resources to use apps you're okay without requiring your explicit consent.

### Access to specific types of data should extend to new resources that contain that data

### Access to specific types of data should not expose other data that was not requested

### Data should have different levels of requirements for user's conciousness in consent

### The authorization data is easily cachable

### Information about Clients (apps) to which users have granted access and what specific access they have delegated should not be made available to Resource Servers that do not enforce relevant access restrictions on those Clients.
