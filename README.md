play-2.1-RC1-with-play2war-0.9-RC1
==================================

Sample project using Play 2.1-RC1 with play2war 0.9-RC1 for deployment on Tomcat6 under a subcontext

----
**Steps followed**:

See also: https://github.com/dlecan/play2-war-plugin/wiki/Configuration
 
 
 - 1. In `APP_HOME/project/plugins.sbt`, added:

    resolvers += "Play2war plugins release" at "http://repository-play-war.forge.cloudbees.com/release/"
    
    addSbtPlugin("com.github.play2war" % "play2-war-plugin" % "0.9-RC1")
 
 
 
 - 2. In `APP_HOME/project/Build.scala`, added:
 
 
    ...
    import com.github.play2war.plugin._
 
 
 
 - 3. In `APP_HOME/project/Build.scala`, updated the project's settings:
 
 
    val main = play.Project(appName, appVersion, appDependencies).settings(
      // Add your own project settings here  
      Play2WarKeys.servletVersion := "2.5"    
    ).settings(Play2WarPlugin.play2WarSettings: _*)
 
 
 
 - 4. Created `APP_HOME/conf/logger.xml` and added the below:
 
 
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
 
 
 
 - 5. Created `APP_HOME`/war/WEB-INF/web.xml` added the content of:
 https://github.com/dlecan/play2-war-plugin/blob/develop/sample/servlet25/war/WEB-INF/web.xml
 
----
**Result**:

So far, just "Action not found".

Follow-up on: https://groups.google.com/forum/?fromgroups=#!topic/play-framework/BJMcXGcRvW0 