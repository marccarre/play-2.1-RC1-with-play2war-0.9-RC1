play-2.1-RC1-with-play2war-0.9-RC1
==================================

Sample project using Play 2.1-RC1 with play2war 0.9-RC1 for deployment on Tomcat 6 under a sub-context


Sources:
- https://github.com/dlecan/play2-war-plugin/wiki/Configuration
- https://groups.google.com/forum/?fromgroups=#!topic/play-framework/BJMcXGcRvW0 

----
**Configuration**:

1. In `APP_HOME/project/plugins.sbt`, added:
            
        resolvers += "Play2war plugins release" at "http://repository-play-war.forge.cloudbees.com/release/"
    
        addSbtPlugin("com.github.play2war" % "play2-war-plugin" % "0.9-RC1")

 
2. In `APP_HOME/project/Build.scala`, added:


        ...
        import com.github.play2war.plugin._


3. In `APP_HOME/project/Build.scala`, updated the project's settings:


        val main = play.Project(appName, appVersion, appDependencies).settings(
          // Add your own project settings here  
          Play2WarKeys.servletVersion := "2.5"    
        ).settings(Play2WarPlugin.play2WarSettings: _*)


4. In `APP_HOME/conf/application.conf`, add at the sub-context at the of the file end:


        application.context=/test/


5. Created `APP_HOME/conf/logger.xml` and added the below:

		<configuration>
        
          <conversionRule conversionWord="coloredLevel" converterClass="play.api.Logger$ColoredLevel" />
        
          <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
            <encoder>
              <pattern>%date - [%level] - from %logger in %thread %n%message%n%xException%n</pattern>
            </encoder>
          </appender>
        
          <logger name="play" level="INFO" />
          <logger name="application" level="INFO" />
        
          <!-- Off these ones as they are annoying, and anyway we manage configuration ourself -->
          <logger name="com.avaje.ebean.config.PropertyMapLoader" level="OFF" />
          <logger name="com.avaje.ebeaninternal.server.core.XmlConfigLoader" level="OFF" />
          <logger name="com.avaje.ebeaninternal.server.lib.BackgroundThread" level="OFF" />
        
          <root level="ERROR">
            <appender-ref ref="STDOUT" />
          </root>
        
        </configuration>
 
----
**Deployment**:

1. Run `play war`. 

2. Rename the generated package from `test-1.0-SNAPSHOT.war` to `test.war`.

3. Copy the war file to `TOMCAT6_HOME/webapps`

4. Start Tomcat 6 and go to http://localhost:8080/test/ to enjoy your brand new Play 2.1 application:

        Your new application is ready.

