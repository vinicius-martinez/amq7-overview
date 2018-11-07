# Introduction

This is a simple demo showcasing some of AMQ7 features/concepts such as: Broker, Client, Cluster, Security and Interconnect;

For additional details, please refer to [Additional References](#demo-additional-references) section;

## Environment

- [Red Hat AMQ 7.2](https://access.redhat.com/documentation/en-us/red_hat_amq/7.2/)
- [JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
- [Apache Maven](https://maven.apache.org/)

## Demo Script:

1. [Installing AMQ-7.2 Broker](#demo-step-1)
2. [Create a Broker](#demo-step-2)

### Installing AMQ-7.2 Broker <a name="demo-step-1"></a>

* Download [Red Hat AMQ 7.2 Broker](https://developers.redhat.com/products/amq/download/) from [Red Hat Developers](https://developers.redhat.com/):

* After downloading AMQ 7.2 broker, uncompress it:

```
unzip amq-broker-7.2.0-bin.zip
```

* Check if the **amq-broker-7.2.0** directory is successfully created and the following directories are within it:

  * *from now on we're going to refer to $AMQ_HOME as the current location of you amq-7.2 broker location* 

```
$ ls -l
total 72
-rw-r--r--@  1 vmartine  staff  22122 Jun 21 04:10 LICENSE
-rw-r--r--@  1 vmartine  staff    179 Jun 21 04:10 NOTICE
-rw-r--r--@  1 vmartine  staff   5391 Jun 21 04:10 README.html
drwxrwxr-x@  5 vmartine  staff    160 Jun 21 04:40 bin
drwxr-xr-x@  3 vmartine  staff     96 Nov  7 17:10 docs
drwxr-xr-x@  3 vmartine  staff     96 Nov  7 17:10 etc
drwxr-xr-x@  7 vmartine  staff    224 Nov  7 17:10 examples
drwxr-xr-x@ 55 vmartine  staff   1760 Nov  7 17:10 lib
drwxrwxr-x@  7 vmartine  staff    224 Jun 21 04:40 schema
drwxr-xr-x@ 16 vmartine  staff    512 Nov  7 17:10 web
```





## Additional References <a name="demo-additional-references">

[Docker Port](https://docs.docker.com/engine/reference/commandline/port/)

[Realm](https://access.redhat.com/documentation/en-us/red_hat_single_sign-on/7.2/html-single/server_administration_guide/#the_master_realm)

[Managing Clients](https://access.redhat.com/documentation/en-us/red_hat_single_sign-on/7.2/html-single/server_administration_guide/#clients)
[Roles](https://access.redhat.com/documentation/en-us/red_hat_single_sign-on/7.2/html-single/server_administration_guide/#roles)

[Login Page Settings](https://access.redhat.com/documentation/en-us/red_hat_single_sign-on/7.2/html-single/server_administration_guide/#login_page_settings)

[Themes](https://access.redhat.com/documentation/en-us/red_hat_single_sign-on/7.2/html-single/server_administration_guide/#themes)

[Authentication Flow](https://access.redhat.com/documentation/en-us/red_hat_single_sign-on/7.2/html-single/server_administration_guide/#authentication-flows)

[User Required Actions](https://access.redhat.com/documentation/en-us/red_hat_single_sign-on/7.2/html-single/server_administration_guide/#required_actions)

[Social Identity Providers](https://access.redhat.com/documentation/en-us/red_hat_single_sign-on/7.2/html-single/server_administration_guide/#social_identity_providers)

[OTP](https://access.redhat.com/documentation/en-us/red_hat_single_sign-on/7.2/html-single/server_administration_guide/#otp_policies)
