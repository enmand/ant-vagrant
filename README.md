Ant scripts for building Vagrant environments
=============================================================

Prerequisits
----------------
The following applications are required:

* [Apache Ant](https://ant.apache.org/) (1.9+)
* [Vagrant](http://www.vagrantup.com/) (1.4.3)
* [Ruby](https://www.ruby-lang.org/en/) (2.1+)

Use
-----
There are a few ways in which you should use these _build scripts_.

### Including vagrant.xml and ruby.xml
To use the Vagrant build scripts, add an `<include>` to the top of the build file that requires Vagrant.

		<include file="vagrant.xml" as="vagrant" />
		<include file="ruby.xml" as="ruby" />


### Configuration
Change the following properties in your `vagrant.properties` file. You can use `${vagrant.basedir}`, or your own  configuration from _build scripts_.:

* `vagrant.vagrantfile` -- The (full or relative) path to your Vagrantfile.
* `berks.berksfile` -- The (full of relative) path to your Berksfile

Change the following properties in your `ruby.properties` file. You can use `${ruby.basedir}`, or your own  configuration from _build scripts_.:

* `ruby.vagrantfile` -- The (full or relative) path to your Vagrantfile.

### Target dependence
In order to make sure Vagrant, and some useful plugins (_vagrant-omnibus_, _vagrant-berkshelf_, and _vagrant-vbguest_), along with [Berkshelf](http://berkshelf.com).

	<target name="SomethingThatNeedsVagrant" depends="vagrant.install">
		...
	</target>

### Directly
#### Installation
To install Vagrant's plugins, and Berkshelf, you can target `vagrant.install` directly with `ant vagrant.install`.

#### Vagrant control
Using Apache Ant to control your Vagrant environment, and to make sure your recipies are up to date at the same time is easy. 

##### Bringing up Vagrant Environment
Use `ant vagrant.up` to bring up your Vagrant environment

##### Shutdown Vagrant Environment
 `ant vagrant.down` to shutdown your Vagrant down.

##### Destroying a Vagrant Environment
 `ant vagrant.destroy` will completely destroy your Vagrant environment

 Note for OS X
--------------
`ant vagrant.install` will fail miserably in most cases on the vagrant-berkshelf
plugin install because of some strange permission issues with the gem requirements.

To overcome these issues, run `ant vagrant.install` and wait for it to fail, then
issue `sudo vagrant plugin install build/vagrant-berkshelf/pkg/vagrant-berkshelf-1.4.0.dev1.gem"`.

Normally, Apache Ant would be responsible for issuing all the commands, but because
we adon't have a tty (as in, Ant doesn't give us one), we need to do it manually.

This relates to the issues located at https://github.com/mitchellh/vagrant/issues/2402
and https://github.com/berkshelf/vagrant-berkshelf/issues/89.

Once this issue is resolved, this can be included in the ant-vagrant/vagrant.xml.
