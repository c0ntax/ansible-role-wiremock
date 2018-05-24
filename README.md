ansible-role-wiremock
=====================

A simple ansible role that will configure one or more [Wiremock](http://wiremock.org) servers to run.

Requirements
------------

None

Role Variables
--------------

Things that you might want to change:

```yaml
wiremock_install: true
```

By default, this role will install wiremock, but setting this to false will skip installation. (So that you can skip it on production environments for example)

```yaml
wiremock_daemon: true
```

Installs wiremock as a init V script. If this is set to false, this role will simply download the jar file for you and put it somewhere sane

```yaml
wiremock_service_state: started
```

If you set `wiremock_daemon` to `true` then this variable can be used to ensure how that service is run.

```yaml
wiremock_root_dir: /var/wiremock
```

A base directory to install wiremock mappings and the like.

```yaml
wiremock_servers:
  - name: wiremock
    port: 8080
    root_dir: "{{ wiremock_root_dir }}"
```

The important bit. This allows you to create one or more start scripts, so that you can have multiple servers for multiple mocked services.
NOTE: Ensure that the name, port and root_dir are all unique or you're going to have a bad time. An example of multiple services would be:

```yaml
wiremock_servers:
  - name: wiremock-oauth
    port: 8080
    root_dir: "{{ wiremock_root_dir }}/oauth"
    enable_local_response_templating: true
    enable_global_response_templating: false
  - name: wiremock-service-1
    port: 8081
    root_dir: "{{ wiremock_root_dir }}/service-1"
    enable_local_response_templating: false
  - name: wiremock-service-2
    port: 8082
    root_dir: "{{ wiremock_root_dir }}/service-2"
```


Things that you probably don't want to touch:

```yaml
wiremock_version: 2.17.0
wiremock_src: "http://repo1.maven.org/maven2/com/github/tomakehurst/wiremock-standalone/{{ wiremock_version }}/wiremock-standalone-{{ wiremock_version }}.jar"
wiremock_original: "/usr/share/java/wiremock-standalone-{{ wiremock_version }}.jar"
wiremock_dest: /usr/share/java/wiremock-standalone.jar
```

Where you want it all installed to and what version you want to grab.

```yaml
wiremock_user: nobody
wiremock_group: nogroup
```

Who you want wiremock to run as

Dependencies
------------

* geerlingguy.java

Example Playbook
----------------


    - hosts: servers
      roles:
         - { role: c0ntax.wiremock, wiremock_install: true, wiremock_daemon: true }

License
-------

Apache-2.0
