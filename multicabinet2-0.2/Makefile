TOMCAT_VER=7.0.54
GRAILS_VER=2.3.11
GRAILS=/root/grails-$(GRAILS_VER)/bin/grails

export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-i386
export GRAILS_HOME=/root/grails-$(GRAILS_VER)

MC_VERSION=0.2

newrelease:
	#debchange --changelog debian/changelog --urgency high --newversion $(MC_VERSION)-1 "Releasing Multicabinet $(MC_VERSION)"

newversion:
	debchange --changelog debian/changelog -i --urgency high --distribution stable "Update"

preptomcat:
	tar xzf apache-tomcat-$(TOMCAT_VER).tar.gz
	rm -rf apache-tomcat-$(TOMCAT_VER)/webapps/*
prep: preptomcat
	cd ./builddir; git pull; git checkout $(MC_VERSION)
build:
	cd ./builddir; $(GRAILS) war ../ROOT.war
install: prep build
	mkdir -p $(DESTDIR)/usr/local/
	cp -R usr/local/multicabinet2 $(DESTDIR)/usr/local
	cp -R apache-tomcat-$(TOMCAT_VER) $(DESTDIR)/usr/local/multicabinet2/tomcat
	install -m 655 ROOT.war $(DESTDIR)/usr/local/multicabinet2/tomcat/webapps/ROOT.war
clean:
#	env
	cd builddir; $(GRAILS) clean
	rm -rf apache-tomcat-$(TOMCAT_VER)
	rm -f ROOT.war
