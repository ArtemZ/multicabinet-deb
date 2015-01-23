TOMCAT_VER=7.0.54
TOMCAT_MIRROR=http://archive.apache.org/dist/tomcat/tomcat-7/
GRAILS_VER=2.3.11
GRAILS=/root/grails-$(GRAILS_VER)/bin/grails

export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-i386
export GRAILS_HOME=/root/grails-$(GRAILS_VER)

MC_VERSION=0.2
MC_OSGI_MANAGER_FILE=multicabinet-osgi-manager-1.0-SNAPSHOT.jar

FELIX_MIRROR=http://apache-mirror.rbc.ru/pub/apache//felix/
FELIX_REPOADMIN=org.apache.felix.bundlerepository-2.0.2.jar
FELIX_SCR=org.apache.felix.scr-1.8.2.jar

newrelease:
	#debchange --changelog debian/changelog --urgency high --newversion $(MC_VERSION)-1 "Releasing Multicabinet $(MC_VERSION)"

newversion:
	debchange --changelog debian/changelog -i --urgency high --distribution stable "Update"

preptomcat:
	tar xzf apache-tomcat-$(TOMCAT_VER).tar.gz
	rm -rf apache-tomcat-$(TOMCAT_VER)/webapps/*
prepbasebundles:
	if [ ! -f $(MC_OSGI_MANAGER_FILE) ]; \
	then\
		wget -O $(MC_OSGI_MANAGER_FILE) http://dl.bintray.com:80/artemz/generic/$(MC_OSGI_MANAGER_FILE); \
	fi
	if [ ! -f $(FELIX_REPOADMIN) ]; \
	then\
		wget -O $(FELIX_REPOADMIN) $(FELIX_MIRROR)$(FELIX_REPOADMIN); \
	fi
	if [ ! -f $(FELIX_SCR) ]; \
	then \
		wget -O $(FELIX_SCR) $(FELIX_MIRROR)$(FELIX_SCR); \
	fi 
prep: preptomcat prepbasebundles
	cd ./builddir; git pull; git checkout $(MC_VERSION)
build:
	cd ./builddir; $(GRAILS) war ../ROOT.war
install: prep build
	mkdir -p $(DESTDIR)/usr/local/
	cp -R usr/local/multicabinet2 $(DESTDIR)/usr/local
	cp -R apache-tomcat-$(TOMCAT_VER) $(DESTDIR)/usr/local/multicabinet2/tomcat

	mkdir -p $(DESTDIR)/usr/local/multicabinet2/base-bundles
	mkdir -p $(DESTDIR)/usr/local/multicabinet2/felix-cache

	install -m 655 $(MC_OSGI_MANAGER_FILE) $(DESTDIR)/usr/local/multicabinet2/base-bundles/$(MC_OSGI_MANAGER_FILE)
	install -m 655 $(FELIX_REPOADMIN) $(DESTDIR)/usr/local/multicabinet2/base-bundles/$(FELIX_REPOADMIN)
	install -m 655 $(FELIX_SCR) $(DESTDIR)/usr/local/multicabinet2/base-bundles/$(FELIX_SCR)

	install -m 655 ROOT.war $(DESTDIR)/usr/local/multicabinet2/tomcat/webapps/ROOT.war
clean:
#	env
	cd builddir; $(GRAILS) clean
	rm -rf apache-tomcat-$(TOMCAT_VER)
	rm -f ROOT.war
	rm -f $(MC_OSGI_MANAGER_FILE)
	rm -f $(FELIX_REPOADMIN)
	rm -f $(FELIX_SCR)
