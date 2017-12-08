Jenkins CI Role
===============
A Jenkins role for installing and configuring jenkins on Debian/Ubuntu servers.

# Role Variables
Amount of time and number of times to wait when connecting to Jenkins after initial startup, to verify that Jenkins is running. Total time to wait = delay * retries, so by default this role will wait up to 300 seconds before timing out.
```
jenkins_connection_delay: 5
jenkins_connection_retries: 60
```
The Jenkins home directory which, amongst others, is being used for storing artifacts, workspaces and plugins. This variable allows you to override the default /var/lib/jenkins location.
```
jenkins_home: /var/lib/jenkins
```
The system hostname; usually localhost works fine. This will be used during setup to communicate with the running Jenkins instance via HTTP requests.
```
jenkins_hostname: localhost
```
The HTTP port for Jenkins' web interface.
```
jenkins_http_port: 8080
```
The location at which the jenkins-cli.jar jarfile will be kept. This is used for communicating with Jenkins via the CLI.
```
jenkins_jar_location: /opt/jenkins-cli.jar
```
Used for setting a URL prefix for your Jenkins installation. The option is added as --prefix={{ jenkins_url_prefix }} to the Jenkins initialization java invocation, so you can access the installation at a path like http://www.example.com{{ jenkins_url_prefix }}. Make sure you start the prefix with a / (e.g. /jenkins).
```
jenkins_url_prefix: ""
```
This role will install the latest version of Jenkins by default (using the official repositories as listed above). You can override these variables (use the correct set for your platform) to install the current LTS version instead:
```
# For Debian/Ubuntu LTS:
jenkins_repo_url: deb https://pkg.jenkins.io/debian binary/
jenkins_repo_key_url: https://pkg.jenkins.io/debian/jenkins.io.key
jenkins_pkg_url: https://pkg.jenkins.io/debian/binary

```
```
jenkins_java_options: "-Djenkins.install.runSetupWizard=false"
```
Jenkins plugins to be installed automatically during provisioning. (Note: This feature is currently undergoing some changes due to the jenkins-cli authentication changes in Jenkins 2.0, and may not work as expected.)

```
jenkins_plugins: []
```
Default admin account credentials which will be created the first time Jenkins is installed.
```
jenkins_admin_username: admin
jenkins_admin_password: admin
```
Default admin password file which will be created the first time Jenkins is installed as /var/lib/jenkins/secrets/initialAdminPassword
```
jenkins_admin_password_file: ""
```
Changes made to the Jenkins init script; the default set of changes set the configured URL prefix and add in configured Java options for Jenkins' startup. You can add other option/value pairs if you need to set other options for the Jenkins init file.
```
jenkins_init_changes:
  - option: "JENKINS_ARGS"
    value: "--prefix={{ jenkins_url_prefix }}"
  - option: "{{ jenkins_java_options_env_var }}"
    value: "{{ jenkins_java_options }}"
```

# Dependencies

None

# Example Playbook
```
- name: Jenkins Ansible
  hosts: all
  become: yes
  roles:
    - ansible-jenkins
```
